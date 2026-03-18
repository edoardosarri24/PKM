Le TPN estendono le [PN con archi inibitori](Petri%20Net%20(PN).md#Archi%20inibitori) (per semplicità, ma anche con qualunque altra estensione delle PN) con il tempo.
# SINTASSI
Si riferisce alla descrizione struttura del sistema, cioè alle regole formali che definiscono come il modello è costruito. È statica e definisce quindi gli elementi e le loro relazioni.
##### Time Petri Net
Una TPN è una tupla $\langle P,T,A^-,A^+,A^{\bullet},EFT,LEF\rangle$, dove:
-  $\langle P,T,A^-,A^+, A^{\bullet}\rangle$ sono gli elementi della PN con archi inibitori.
- EFT è una funzione che associa a ogni transizione $t\in T$ un earliest firing time $EFT(t)\in\mathbb{Q_{\ge0}}$.
- LFT è una funzione che associa a ogni transizione $t\in T$ un latest firing time $LFT(t)\in\mathbb{Q_{\ge0}}$.
- Si ha che $EFT(t)\le LFT(t),\forall t\in T$. L'intervallo che definiscono è detto intervallo di firing; se $EFT(t)=LFT(t)$ allora la transizione è detta immediata. Stiamo dicendo che il tempo di esecuzione della transizione è sicuramente in questo intervallo, ma non sappiamo quale.
- [Esempio](TPN%20example.jpeg).
##### Stato
Lo stato di una TPN è una coppia $\langle m,\tau\rangle$ dove:
- $m$ è una marcatura.
- $\tau$ è una funzione che associa a ogni transizione abilitata $t$ un time to fire (tempo di attivazione) $\tau(t)\in\mathbb{R}_{\ge0}$. Il time to fire di una transizione viene campionato quando la transizione viene abilitata.
- [Esempio](time%20to%20fire%20TPN%20example.png).
# SEMANTICA
Si riferisce al comportamento del sistema, ovvero a cosa gli elementi evelvono nel tempo. È dinamica e descrive le regole che definisocono l'esecuzione del modello.
##### Transizioni attivabili
- Una transizione $t$ è attivabile nello stato $s=\langle m,\tau\rangle$ se e solo se:
	- $t$ è abilitata dalla marcatura $m$.
	- $\tau(t)\le\tau(t'),\forall t'\in T|t'$ è abilitata da $m$, cioè se il time to fire di $t$ è non maggiore del time to fire di ogni altra transizione abilitata.
	- L'attivazione delle transizioni avviene quando il firing time è a zero.
- Una transizione $t$ non può essere attivata prima che questa sia stata abilitata continuamente per un tempo $T_{min}$ non inferiore al suo earlist fire time, cioè deve essere $T_{min}\ge EFT(t)$. Una transizione $t$ non può evitare di essere attivata quando è stata abilitata continuamente per un tempo $T_{max}$ non pari al suo latest fire time, cioè non dopo $T_{max}\le LFT(t)$.
- In questo [esempio](time%20to%20fire%20di%20TPN%20example.png) vediamo che in ogni stato iniziale $t0$ e $t2$ sono entrambe abilitate, ma solo $t0$ è attivabile perchè il suo time to fire è in ogni stato iniziale inferiore di quello di $t2$.
##### Cambio stato
L'attivazione di una transizione $t$ cambia lo stato da $s_0=\langle m_0,\tau_0\rangle$ in $s_1=\langle m_1,\tau_1\rangle$ in questo modo:
- $m_1$ è derivata da $m_0$ come nell PN:
	- Rimuovendo un token da ogni posto di ingresso di $t$. Formalmente $m_{temp}=m_0(p)-1,\forall p|(p,t)\in A^-$, dove $m_{temp}$ è la marcatura temporanea.
	- Aggiungere un token in ogni posto di uscita di $t$. Formalmente $m_1=m_{temp}(p)+1,\forall p|(t,p)\in A^+$, dove $m_1$ è la nuova marcatura.
- Definizioni
	- Una transizione abilitata da $m_1$ è detta:
		- Persistente
			Se è diversa da $t$ ed è abilitata sia da $m_0$ che da $m_{temp}$.
		- Newly enabled
			Se è abilitata da $m_1$ ma non da $m_0$ o da $m_{temp}$ o se è la transizione $t$ che ha sparato ed è abilitata da $m_1$.
	- Una transizione è detta disabilitata se è abilitata da $m_0$ ma non da $m_1$, cioè se dopo l'attivazione di $t$ è non abilitata.
- $\tau_1$ è derivata da $\tau_0$ come:
	- Riducendo il time to fire di ogni transizione persistente del valore di $\tau_0(t)$, cioè $\tau_1(t')=\tau_0(t')-\tau_0(t), \forall t'\in T|t'$ è persistente in $m_1$.
	- Eliminando il time to fire di ogni transazione disabilitata dall'attivazione di $t$.
	- Campionando il time to fire di ogni transizione newly enabled $t'$ nel suo intervallo di firing. Formalmente $\tau_1(t')\in[EFT(t'),LFT(t')],\forall t'\in T|t'$ è newly enable in $m_1$.
- [Esempio](TPN%20example%20di%20cambio%20stato.png).
##### [Esecuzione](TPN%20execution.pdf)
# ESTENSIONE
Come le PN anche le TPN possono essere estese con le enabling function, le update function e le priority esattamente nello stesso modo delle PN.
# ANALISI
Le domande generali a cui la fase di analisi vuole rispondere sono due:
- Time reachability
	Dati alcuni vincoli temporali, quali sono gli ordinamenti delle transizioni possibili?
- Min/Max duration
	Quale è il tempo minimo e quello massimo in cui una data sequenza di transizioni viene completata?
### Classe degli stati
Una classe degli stati è $S=\langle m,D\rangle$ (stessa marcatura e molti time to fire), dove:
- $m$ è una marcatura.
- $D$ è detto dominio di firing ed è l'insieme (continuo) dei valori dei time to fire delle transizioni abilitate. Vuol dire un dominio contiene tutti gli infiniti time to fire che hanno senso di stare insieme. I time to fire sono infiniti perché ci sono infiniti campionamenti dei time to fire delle transizioni, cioè questi sono tutti i valori che possono assumere nei loro intervalli di firing.
##### Stato
Uno stato $s=\langle m_s,\vec\tau_s\rangle$ è incluso nella classe $S=\langle m,D\rangle$ se:
- Hanno la stessa marcatura, cioè e $m=m_s$.
- I time to fire di $\vec{\tau_s}$ rispettano i vincoli di $D$, cioè se $\vec{\tau_s}\in D$.
Gli stati che appartengono alla stessa classe di non sono equivalenti, ma possono essere raggiunti dalla stessa sequenza di firing. All'interno di una DBM (supponiamo che descriva due transizioni), se traccio una retta a 45°, gli stati che stanno sotto abilitano le transizioni in cui è minore il time to fire della transizione rappresentata dalle ascisse, viceversa con la transizione rappresentata dall'asse delle ordinate.
##### Raggiungibilità
Una classe di stati $S'$ è raggiungibile da $S$ tramite una transizione $t$ se e solo se contiene tutti e soli gli stati che sono raggiungibili da qualche stato in $S$ tramite qualche possibile attivazione di $t$, cioè se e solo se:
- $\forall s'\in S'\ \exists s\in S|s\xrightarrow{t}s'$.
- $s\in S\land s\xrightarrow{t}s'\Rightarrow s'\in S'$.

La raggiungibilità debole (detta AE reachebility) non garantisce che ogni stato $s'\in S'$ sia raggiungibile da tutti gli stati $s\in S$ (questa sarebbe la raggiungibilità forte); garantisce invece che per ogni $s'\in S'$ esiste almeno uno stato $s\in S$ e un tempo $\tau$ tale che $t$ può essere attivata con time to fire $\tau$ producendo lo stato $s'$.
Questa condizione tuttavia è abbastanza forte per decidere se valgono le proprietà di raggiungibilità sotto i vincoli temporali del modello:
- Uno stato $s'$ è raggiungibile dalla classe iniziale $S_0$ se e solo se $s'$ è contenuto in una classe di stati $S$ raggiungibile da $S_0$.
- Una sequenza di firing $\rho$ è attivabile se e solo se esiste una classe $S'$ raggiungibile da $S_0$ dalla quale esiste un percorso riproducibile da $\rho$.
### Difference Bound Matrix (DBM)
È una struttura usata per gestire i vincoli temporali delle transizioni.
Una DBM $D$ nello spazio di un vettore di valori sconosciuti $\tau=\langle\tau_0,\cdots,\tau_N\rangle$ è un insieme di disuguaglianze lineari che vincolano la differenza tra elementi di $\tau$: $D=\begin{cases} \tau_i-\tau_j\le b_{ij},\ \forall i,j\in\{0,\cdots,N-1\}\cup\{\star\}\mbox{ con } i\ne j,b_{ij}\in\mathbb{R}\\\tau_{\star}=0\end{cases}$. Osserviamo che le diseguaglianze composte da due time to fire effettivi corrispondono a bordi a 45 gradi, mentre quelle composte dal ground ($\tau_{\star}$) sono bordi orizzontali o verticali.
In una DBM l'insieme delle soluzioni delle diseguaglianze è detto DBM zone.
Osserviamo che $\tau_{\star}$ è una variabile fittizia, chiamata ground, che rappresenta l'ingresso nella classe degli stati, ed è usata per:
- Trasformare i vincoli della forma $\tau_i\le b_{i\star}$ in vincoli diagonali della forma $\tau_i-\tau_{\star}\le b_{i\star}$.
- Trasformare i vincoli della forma $\tau_i\ge-b_{\star i}$ in vincoli diagonali della forma $\tau_{\star}-\tau_i\le b_{\star i}$.
##### Forma normale
Siccome più insiemi di diseguaglianze possono rappresentare la [stessa DBM](DBM%20normal%20form%20example.pdf), allora si introduce la forma normale.
Una DBM $D$ è in forma normale se $b_{ij}$ è il massimo valore di $\tau_i-\tau_j$ che può essere raggiunto da qualunque soluzione dell'insieme di disequazioni $\forall i,j\in\{0,\cdots,N-1\}\cup\{\star\}|i\ne j$.
- La forma normale può essere calcolata con l'algoritmo di [Floyd-Warshall](Floyd-Warshall%20algorithm.pdf) in un tempo $\mathcal{O}{(N^3)}$ (ci sono 3 cicli annidati ognuno di lunghezza $N$).
##### Rappresentazione con grafo
La DBM definita dalla matrice dei coefficienti $b_{ij}$ è associata in modo biunivoco a un [grafo diretto](DBM%20to%20graph.png), etichettato e completo $\langle V,E,b\rangle$, dove
- L'insieme dei vertici $V$ è composto dai valori non noti $\tau_i$.
- Un arco diretto esiste sempre tra due nodi, cioè $E=V\times V$.
- L'arco dal nodo $\tau_j$ al nodo $\tau_i$ è etichettato come $b_{ij}$.
##### Lemmi
- Forma normale
	- Una DBM è in forma normale se e solo se $b_{ij}$ è il cammino più corto dal vertice $\tau_j$ al vertice $\tau_i$, $\forall i,j\in\{0,\cdots,N-1\}\cup\{\star\}$ per $i\ne j$.
	- I coefficienti $b_{ij}$ sono i cammini più corti tra i vertici $\tau_j$ e $\tau_i$ se e solo se $b_{ij}\le b_{ik}+b_{kj}, \forall i,j,k\in\{0,\cdots,N-1\}\cup\{\star\}$ con $i\ne j,\ i\ne k,\ j\ne k$.
	- Teorema
		Una DBM è in forma normale se e solo se $b_{ij}\le b_{ik}+b_{kj}, \forall i,j,k\in\{0,\cdots,N-1\}\cup\{\star\}$, con $i\ne j,\ i\ne k,\ j\ne k$. Cioè se vale la disuguaglianza triangolare per le etichette.
- Decidere se una DBM zone è vuota:
	- Se una DBM zone non è vuota allora il grafo corrispondente non ha nessun ciclo dove la somma delle etichette dei lati attraversi è negativa (cicli negativi).
	- Se nel grafo corrispondente a una DBM non ci sono cicli negativi allora la DBM zone è non vuota.
	- Teorema
		Una DBM zone è non vuota se e solo se il corrispondente grafo non ha cicli negativi. Con questo lemma l'assenza di cicli negativi diventa condizione necessaria e DBM è vuota o no.
##### Proiezione
Data una DBM $D$ nello spazio dei valori sconosciuti $\tau=\langle\tau_0,\cdots\tau_{N-1}\rangle$, la proiezione di $D$ sullo spazio dei valori sconosciuti $\tau_{\downarrow0}=\langle\tau_1,\cdots\tau_{N-1}\rangle$ è $D_{\downarrow0}=\{\langle\tau_1,\cdots\tau_{N-1}\rangle|\exists\tau_0\in\mathbb{R}\cup\{\infty\}\mbox{ tale che }\langle\tau_0,\cdots\tau_{N-1}\rangle\in D\}$.
- La proiezione si può implementare facilmente in una DBM zone in forma normale.
- Lemma
	Se una DBM $D$ nello spazio dei valori sconosciuti $\tau=\langle\tau_0,\cdots\tau_{N-1}\rangle$ è in forma normale allora il grafo $D_{\downarrow0}$, cioè della proiezione di $D$, è ottenuto dal grafo $D$ eliminando il nodo $\tau_0$.
### Come funziona l'analisi nella teoria
Supponiamo che il dominio $D$ della classe degli stati iniziale $S=\langle m,D\rangle$ sia un insieme di disequazioni lineari e sia in forma DBM.
##### Successore
Una classe di stati $S=\langle m,D\rangle$ con $D$ in forma normale DBM ha un successore tramite la transizione $t_0$ se e solo se $t_0$ è abilitata da $m$ e $D$ accetta soluzioni dove $\tau(t_0)$ è minore o uguale di tutti gli altri time to fire delle transizioni abilitate, cioè se il dominio ristretto $D_{t_0}=\begin{cases} \tau(t_i)- \tau(t_j)\le b_{ij}\\\tau(t_0)-\tau(t_h)\le0\\\forall t_i,t_j\in T_e(m)\cup t_{\star}\mbox{ con }t_i\ne t_j,\forall t_h\in T_e(m)-\{t_0\}\end{cases}$, dove $T_e(m)$ sono le transizioni abilitate dalla marcatura $m$, ha un insieme di soluzioni non vuoto.
- Capire se una classe ha un successore ha una complessità lineare rispetto al numero di transizioni abilitate.
- Come calcolare il dominio ristretto $D'$: [teoria](analisi%20nelle%20TPN.pdf), [esempio](TMP%20analysis%20example.pdf).
### Enumerazione dello spazio degli stati
L'enumerazione dello spazio ([esempio](enumerazione%20dello%20spazio%20degli%20stati%20TPN.jpeg), non da sapere ovviamente) degli stati produce lo state class graph (SCG).
Un symbolic run è un percorso tra le classi di stato del grafo. Rappresenta una possibile sequenza di eventi, cioè una possibile sequenza di transizioni attivate. Non specifica i dettagli quantitativi, ma solo quelli qualitativi: non ci dice quando le transizioni sono attivati (cioè i time to fire), ma solo l'ordine con il quale le transizioni si attivano.
##### Dominio dei tempi di un symbolic run
Consideriamo un symbolic run $\rho=S_0\xrightarrow{t^0_{f(0)}}S_1\cdots S_{N-1}\xrightarrow{t^{N-1}_{f(N-1)}}S_N$, dove:
- $S_0=\langle m_0,D_0\rangle$ è la classe di stato iniziale.
- $S_n=\langle m_n,D_n\rangle$ è l'$n-esima$ classe di stato visitata dal $\rho$, $\forall n\in\{1,\cdots,N\}$.
- $f(n)$ è l'indice della transizione che ha sparato nella classe $S_n$, $\forall n\in\{0,\cdots,N-1\}$.
- $t^k_h$ è il time to fire della transizione $t_h$ nella classe di stato $S_k$.

Con queste premesse il dominio dei tempi di $\rho$, $D_{\rho}=\begin{cases} D_n\\\tau(t^n_{f(n)})-\tau(t^n_i)\le0\\\tau(t^n_{f(n)})=\tau(t^{n+1}_{\star})\\\forall n\in\{0,\cdots,N-1\},\forall t^n_i\in T_e(m)\end{cases}$, è in forma DBM:
- La prima riga indica i domini di tutte le classi che sono state visitate dal percorso $\rho$.
- La seconda riga ci dice che il time to fire della transizione che ha sparato è minore o uguale di quello di ogni altra transizione.
- La terza riga ci indica classe dopo classe chi è il nuovo ground (il nuovo ground è la transizione che spara).

Dobbiamo osservare che:
- Il caso peggiore del tempo di completamente di $\rho$ non è la somma dei tempi peggiori (intendo il valore più alto nell'intervallo di firing) delle transizioni che hanno sparato in ogni classe. Questo perché il tempo che una transizione passa nella classe $i+1$ dipende dal time to fire della transizione che ha sparato della classe $i$: supponiamo che l'intervallo di firing nella classe $i$ è tra 1 e 2 e nella classe $i+1$ è tra 6 e 12; se nella classe $i$ il time to fire effettivo è 1 allora la transizione non può nella classe $i+1$ sparare a 12, ma sparerà al più a 11; se invece spara a 2 nella classe $i$ allora si che può sparare al tempo 12 nella classe $i+1$.
- Il calcolo di $D_{\rho}$ ha una complessità $\mathcal{O}{(N^3)}$ con $N$ che è il numero di transizioni che hanno sparato durante il percorso.
# SIMULAZIONE
Nell'analisi stiamo considerando tutti i comportamenti possibili; questo viene fatto tramite lo spazio degli stati. Quando facciamo simulazione invece campioniamo i possibili comportamenti.
# MODELLAZIONE
Vediamo un [esempio](modeling%20of%20TPN%20example.pdf) dove modelliamo una situazione di tre task tramite le TPN. Usando il grafo delle classi degli stati a runtime possiamo capire a runtime se accettare o rifiutare un job del task aperiodico; se siamo in un percorso dove i task periodici possono mancare le deadline allora rifiutiamo a prescindere il job, altrimenti lo accettiamo.
### Mancanze
##### Deadline
Le TPN non rappresentano le deadline relative e non possono essere quindi aggiunte al modello. Il loro rispetto sarà un proprietà che dovremmo verificare: dopo l'analisi dello spazio degli stati andrà verificato che il tempo che va dal rilascio del job alla sua concusione sia minore della deadline relativa del task.
##### Non determinismo
Ci sono due tipi di non determinismo in questo modello:
- Il campionamento del time to fire di ogni transizione. Non è un aspetto che il designer può controllare (magari sono task che si attivano a seguito di stimoli esterni).
- L'accettazione o il rifiuto dei job del task $\tau_3$. È sotto il controllo dello sviluppatore: le domande ci dobbiamo fare sono sicuramente i casi estremi, cioè cosa succede ai task 1 e 2 (le loro deadline vengono sforate o sono rispettate) se accetto sempre o rifiuto sempre i job del task 3; si dovrà poi definire delle politiche secondo cui accettare o rifiutare un job del task 3 in relazione a cosa succede ai task 1 e 2.
### [Esempio BioMerieux](esempio%20TPN%20per%20BioMerieux.pdf)