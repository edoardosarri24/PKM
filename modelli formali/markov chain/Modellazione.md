Vediamo quali sono le principali caratteristiche quando si parla di modelli.
Solitamente quando si fa un modello si parte dal [processo di arrivo di Poisson](processo%20di%20arrivo%20di%20Poisson.md) e poi si migliora introducendo complessità considerando ad esempio una [whitt approximation](whitt%20approximation.md) o una [continuos phase type distribution](continuos%20Phase%20Type%20(PH)%20distribution.md) oppure tramite [distribuzioni generali](general%20distribution.md).
##### Conseguenze
- Vantaggi
	- Spesso a inizio processo non abbiamo i dati, ma con i modelli si può esplorare lo spazio degli stati in una situazione in cui non abbiamo mai eseguito il sistema prima (perché non lo abbiamo).
	- I modelli permettono di prevedere e di spiegare il comportamento.
- Svantaggi
	- Si basano su delle supposizioni. Ad esempio si le distribuzioni sono IID e quindi non si riesce a mettere in correlazione le durate di due transizioni successive.
	- Sono difficili da definire: si deve conoscere bene sia le tecnologie che i modelli (è una figura difficile da trovare).
	- Scalano male quando i sistemi sono molto complessi.
# Stabilità
La condizione di stabilità in un sistema a coda generico ci dice che il servizio gestisce le richieste più velocemente di quanto queste arrivano in media. Questo vuol dire che se $\lambda$ è il tasso di arrivo e $\mu$ è il tasso di servizio allora perché la condizione sia rispettata deve essere $\lambda<\mu$.
Il concetto di aumentare il tasso del server equivale al vertical scaling: si aumenta la CPU che diamo al singolo srver.
##### Traffic intensity
Il rapporto $\rho=\tfrac{\lambda}{\mu}$ è detto traffic intensity:
- Se $\rho=1$ allora il sistema è sempre occupato e la coda può crescere molto.
- Se $\rho<1$ allora il sistema è stabile.
- Se $\rho>1$ allora il sistema è instabile.
##### Esempio
Consideriamo un sistema M/M/1 di [questo tipo](MM1.png).
Prendiamo il caso in cui il tasso di arrivo è $\lambda=\tfrac{40}{100}$ e il tasso di servizio è $\mu=\tfrac{41}{100}$: in questo caso $\rho=\tfrac{40}{41}$. La dimensione della coda può variare molto, come si vede in [questo esempio](borderline%20stabilità.png).
Se adesso per il tasso di arrivo diventa $\lambda=\tfrac{45}{100}$ allora vediamo da [questo esempio](MM1%20stabile.png) che la lunghezza della coda varia molto meno ed è più piccola.
##### Problema
Consideriamo un sistema stabile, cioè dove $\lambda<\mu$, dove però $\lambda\sim\mu$: questo vuol dire che in media arrivano e vengono consumati più o meno lo stesso numero di job.
Il problema in questo tipo di sistemi sta nel considerare la media: se in un determina lasso di tempo arrivano pochi job, allora in un momento futuro ne arriveranno molti (perché si deve rientrare nella media).
Questo porta a poter avere code molto lunghe anche in sistemi stabili, soprattutto in condizioni alto bursting.
# Finite state space
La transient analysis e la steady state analysis possono essere eseguite solo se lo spazio degli stati è finito, visto che richiedono la loro enumerazione.
Spesso questa cosa non è possibile e quindi si deve fare una simulazione.
##### Rejecting
Una soluzione per limitare il numero di token è fare rejection se si supera un certo numero di job in coda; un modello di questo tipo è detto a coda aperta, perché teoricamente possono arrivare infiniti token.
Se guadiamo [questo esempio](MM1%20con%20rejection%20e%20servizio%20bounded.png) di MM1 con rejection e facciamo una [simulazione](MM1%20con%20rejection.png) possiamo osservare che il numero di job in coda non è mai superiore a 16, cioè del valore di $QueueSize$ definito come parametro. È importante che la transizione di rejections sia immediata e abbia un'enabling funciton che definisca il numero massimo di token che possono stare nella coda; le transizioni immediate hanno la priorità sulle altre e se non mettiamo la funzione nessun job sarebbe mai completato.
Il valore di $queueSize$ deve essere ben stabilito: se volgiamo che ci permetta solo di eseguire l'analisi, cioè di implementare uno spazio delle marcature finito, allora può essere anche alto; se invece abbiamo un sistema che deve effettivamente avere un limite alla coda allora il numero deve prendere il valore dei requisiti.
##### Coda chiusa
Un altro modo è non permettere token infiniti, ma definirli tramite la coda chiusa. Si parla di coda chiusa quando il numero di job (e quindi di token) all'interno del modello è un'invariante.
Se osserviamo [questo modello](MM1%20closed%20queue.png) e facessimo un simulazione allora osserviamo che il numero di token è limitato in ogni momento. Se osserviamo il tasso di unlock vediamo è moltiplicato per il numero di token in Idle: questo vuol dire che li serve in parallelo: non ha molto senso infatti pensare che un i job che devono arrivare si mettano in qualche coda ad aspettare.
# Idiomi di servizio
Per gestire i job e l'efficienza del sistema abbiamo due tecniche.
In questo contesto si parla di orizzontal scheling: si aggiungono server, ognuno con la stessa potenza di calcolo.
- Parallelo
	In questo modo è come se avessimo infiniti server, uno per ognuno dei job che arriva.
	L'implementazione di questo è moltiplicare il tasso di gestione per il numero di token nel posto in ingresso.
- Bounded server
	Abbiamo un numero limitato di server. Per implementare questo si moltiplica il tasso di gestione per il minimo tra il numero di token nel posto in ingresso e il numero massimo di server.
	In questa situazione quando arriva l'$n+1$-esimo job si hanno due possibilità: va in coda; entra in esecuzione e condivide le risorse con quelli già in esecuzione. Questa seconda possibilità è buona quando abbiamo pochi server e molti job in arrivo: i job che entrano e condividono le risorse non si accorgono del degrado delle prestazioni.
# Service police
Ci sono varie tecniche con cui un job può subire preemption.
Se consideriamo al [sistributed component architecture](Distributed%20component%20architecture.md) queste si equivalgono.
- Preemptive resume (PRS)
	Se io ti interrompo quando te riprendi il servizio allora lo riprendi da dove ti eri interrotto, cioè ti manca quanto ti mancava.
- Preemptive Repeate Different (PRD)
	Quando riparti ricampioni il tempo che devi eseguire.
- Preemptive Repeat Identical (PRI)
	Se io ti interrompo quando riparti devi ripartire con lo stesso valore che avevi campionato la prima volta. Questo è significativo quando si ha un gioco dove perde chi estree il valore più piccolo; se uno estrae un valore alto è fregato.
# Reward
Le reward, cioè le quantità caratterizzanti, quelle a cui siamo interessati, possono essere considerate da più punti di vista. Consideriamo come [esempio](MM1%20con%20rejection%20e%20parallelismo%20bounded.png) un coda aperta MM1 con rejection e un numero di server limitato.
- Tasso di rigetto
	Si usa $If(queue==queueSize,\lambda,0)$.
- Qualità del servizio lato utente.
	In questo caso si deve calcolare il tempo medio di soggiorno nella coda. Nell'esempio un possibile modo è usare la condizione  $If(queue==queueSize,1*ARate/ARateDiv,0)$: se la coda è piena paghiamo il rate di arrivo dei job, altrimenti 0.
- Efficienza lato server
	Si deve calcolare il numero medio di risorse non sfruttate. Nell'esempio un possibile modo è usare la condizione $If(queue==queueSize,1*ARate/ARateDiv,If(queue<poolsize,poolsize-queue,0))$: se la coda è piena allora paghiamo il rate di arrivo dei job, altrimenti se ci sono meno job dei server disponibili si paga il numero di server che non stiamo usando.