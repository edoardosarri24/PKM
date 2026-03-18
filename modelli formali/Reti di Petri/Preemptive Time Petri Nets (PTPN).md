Le PTPN estendono le [TPN](Time%20Petri%20Net%20(TPN).md) con le risorse.
Quando si estende un modello si aumenta il potere descrittivo ma si aumenta anche la complessità.
# SINTASSI
Una TPN è una tupla $\langle P,T,A^-,A^+,A^{\bullet},EFT,LEF, Res,Req, Prio\rangle$, dove:
-  $\langle P,T,A^-,A^+, A^{\bullet},EFT,LEF\rangle$ sono gli elementi della TPN.
- $Res$ è un insieme di risorse. Sono intese risorse su cui si può fare preemption, come il processore; non sono risorse che sono utilizzate in mutua esclusione (sulle quali infatti non si può fare preemption, ma una volta acquisite sono usate finchè il compito non è terminato).
- $Req$ è una funzione che associa a ogni transizione $t$ un sottoinsieme di risorse, cioè $Req(t)\subseteq Res$.
- $Prio$ è una funzione che associa a ogni transizione $t$ una priorità $Prio(t)\in\mathbb{N}$. Come in Unix a priorità più alta corrisponde intero più piccolo. Notiamo che con questo modello una transizione accede a tutte le risorse di cui ha bisogno con la tessa priorità; il modello dove una transizione accede alle risorse con priorità diverse è equivalente ma è molto più error prone (es: inversion priority).
- [Esempio](PTPN%20sintax%20example.jpeg).
### Stato
Lo stato di una TPN è una coppia $\langle m,\tau\rangle$ dove:
- $m$ è una marcatura.
- $\tau$ è una funzione che associa a ogni transizione abilitata $t$ un time to fire (tempo di attivazione) $\tau(t)\in\mathbb{R}_{\ge0}$. Il time to fire di una transizione viene campionato quando la transizione viene abilitata.
##### Osservazione
- Aggiungere le risorse per definire uno stato sarebbe ridondante. La marcatura infatti mi dice quali transazioni sono abilitate; una volta definite le transizioni abilitate la risorsa sarà assegnata alla transizione (= andrà in esecuzione la transizione) che la richiede con proprietà maggiore.
- Quando due transizioni richiedono la stessa risorsa con la stessa priorità si deve definire un modo deterministico per assegnare la risorsa.
# SEMANTICA
[Esempio](esecution%20example%20PTPN.pdf)
##### Transizioni
- Come nelle PN con gli archi inibitori una transizione è abilitata dalla marcatura $m$ se e solo se:
	- $m(p)>0,\forall p\in P|(p,t)\in A^-$, cioè se $m$ assegna almeno un token a ogni posto in ingesso di $t$.
	- $m(p)=0,\forall p\in P|(p,t)\in A^{\bullet}$, cioè se $m$ non assegna alcun token a ogni posto inibitori di $t$.
- Definizioni
	- Una transizione $t$ è progressing (equivale a un task in [stato running](Real%20time/task.md#STATI), cioè che ha l'uso della cpu ed è in esecuzione) nella marcatura $m$ se e solo se:
		- $t$ è abilitata da $m$.
		- $Req(t)\cap Req(t')\ne\emptyset\Rightarrow Prio(t)<Prio(t'),\ \forall t'\in T_e(m)$, cioè se $t$ richiede e risorse con priorità maggiore rispetto alle altre transizioni abilitate da $m$.
	- Una transizione $t$ è suspended (equivale a un task nella coda dei task pronti) nella marcatura $m$ se e solo se:
		- $t$ è abilitata da $m$.
		- $t$ non è progressing in $m$.
- Una transizioni $t$ è attivabile (può sparare) nello stato $s=\langle m,\tau\rangle$ se e solo se:
	- $t$ è progressing in $m$.
	- $\tau(t)\le\tau(t'), \forall t'\in T|t' \mbox{ è progressing in } m$, cioè se ha il time to fire minore o uguale del time to fire di ogni altra transizione in stato progressing.
##### Cambio stato
L'attivazione di una transizione $t$ cambia lo stato da $s_0=\langle m_0,\tau_0\rangle$ in $s_1=\langle m_1,\tau_1\rangle$ in questo modo:
-  $m_1$ è derivata da $m_0$ come nell PN:
	- Rimuovendo un token da ogni posto di ingresso di $t$. Formalmente $m_{temp}=m_0(p)-1,\forall p|(p,t)\in A^-$, dove $m_{temp}$ è la marcatura temporanea.
	- Aggiungere un token in ogni posto di uscita di $t$. Formalmente $m_1=m_{temp}(p)+1,\forall p|(t,p)\in A^+$, dove $m_1$ è la nuova marcatura.
- Definizioni
	- Una transizione abilitata $t'$ da $m_1$ è detta, come nelle TPN:
		- Persistente
			Se è diversa da $t$ ed è abilitata sia da $m_0$ che da $m_{temp}$.
		- Newly enabled
			Se è abilitata da $m_1$ ma non da $m_0$ o da $m_{temp}$ o se è la transizione $t$ che ha sparato ed è abilitata da $m_1$.
	- Una transizione è detta disabilitata se è abilitata da $m_0$ ma non da $m_1$, cioè se dopo l'attivazione di $t$ è non abilitata (come nelle TPN).
- $\tau_1$ è derivata da $\tau_0$ come:
	- Riducendo il time to fire di ogni transizione persistente e progrssing del valore di $\tau_0(t)$, cioè $\tau_1(t')=\tau_0(t')-\tau_0(t), \forall t'\in T|t'$ è persistente e progressing in $m_1$.
	- Lasciando invariato il time to fire di ogni transizione persistente e suspended, cioè $\tau_1(t')=\tau_0(t'), \forall t'\in T|t'$ è persistente e suspended in $m_1$.
	- Eliminando il time to fire di ogni transazione disabilitata dall'attivazione di $t$.
	- Campionando il time to fire di ogni transizione newly enabled $t'$ nel suo intervallo di firing, cioè $\tau_1(t')\in[EFT(t'),LFT(t')],\forall t'\in T|t'$ è newly enable in $m_1$.
### [Esecuzione](PTPN%20esection.pdf)
# ESTENSIONE
Come per TPN e le PN, anche le PTPN possono essere estese con le enabling function, le update function e le priority esattamente nello stesso modo delle altre due.
- In questo caso un modo per decidere quale transizione spara a parità di priorità si possono usare le priorità dell'estensione (comunque da non confondere con le priorità delle PTPN).
# MODELLAZIONE
Vediamo due [esempi](modellazione%20PTPN%20example.pdf): nel primo si ha una situazione classica, nel secondo si introducono risorse da usare in mutex.
# ANALISI
Come per le [TPN](Time%20Petri%20Net%20(TPN).md) le domande generali a cui la fase di analisi vuole rispondere sono due:
- Time reachability
	Dati alcuni vincoli temporali, quali sono gli ordinamenti delle transizioni possibili?
- Min/Max duration
	Quale è il tempo minimo e quello massimo in cui una data sequenza di transizioni (cioè il tempo di completamento di un task) viene completata?
### Classe degli stati
Una classe degli stati è $S=\langle m,D\rangle$ (stessa marcatura e molti time to fire), dove (come nelle TPN):
- $m$ è una marcatura.
- $D$ è detto dominio di firing ed è l'insieme (continuo) dei valori dei time to fire delle transizioni attive. Vuol dire un dominio contiene tutti gli infiniti time to fire che hanno senso di stare insieme. Ii time to fire sono infiniti perché ci sono infiniti campionamenti dei time to fire delle transizioni, cioè tutti i valori che possono assumere nei loro firing time.
##### Stato
Uno stato $s=\langle m_s,\vec{\tau_s}\rangle$ è incluso nella classe $S=\langle m,D\rangle$ se (come nelle TPN):
- Hanno la stessa marcatura, cioè e $m=m_s$.
- Il vettore dei time to fire $\vec{\tau_s}$ rispetta i vincoli di $D$, cioè se $\vec{\tau_s}\in D$.
##### Raggiungibilità
Una classe di stati $S'$ è raggiungibile da $S$ tramite una transizione $t$ se e solo se contiene tutti e soli gli stati che sono raggiungibili da qualche stato in $S$ tramite qualche possibile attivazione di $t$, cioè se e solo se (come nelle TPN):
- $\forall s'\in S'\ \exists s\in S|s\xrightarrow{t}s'$.
- $s\in S\land s\xrightarrow{t}s'\Rightarrow s'\in S'$.

Come nelle TPN consideriamo la raggiungibilità debole (detta AE reachebility), quella che non garantisce che ogni stato $s'\in S'$ sia raggiungibile da tutti gli stati $s\in S$ (questa sarebbe la raggiungibilità forte); garantisce invece che per ogni $s'\in S'$ esiste almeno uno stato $s\in S$ e un tempo $\tau$ tale che $t$ può essere attivata con time to fire $\tau$ producendo lo stato $s'$.
Questa condizione tuttavia è abbastanza forte per decidere se valgono le proprietà di raggiungibilità sotto i vincoli temporali del modello.
- Uno stato $s'$ è raggiungibile dallo stato iniziale $S_0$ se e solo se $s'$ è contenuto in una classe $S$ raggiungibile da $S_0$.
- Una sequenza di firing $\rho$ è attivabile se e solo se esiste una classe $S'$ raggiungibile da $S_0$ dalla quale esiste un percorso riproducibile da $\rho$.
### Funzionamento della teoria formale
##### Dominio $D$
Come per le TPN supponiamo che il dominio di firing $D$ della classe iniziale $S=\langle m,D\rangle$ sia rappresentato da un insieme disequazioni lineari in forma DBM, cioè $D=\tau(t_i)-\tau(t_j)\le b_{ij}, \forall t_i,t_j\in T_e(m)\cup\{t_{\star}\}$ con $i\ne j$, dove:
- $b_{ij}\in\mathbb{R}\cup\{\infty\}$.
- $T_e(m)$ è l'insieme delle transizioni abilitate dalla marcatura $m$.
- $\tau(t_i)$ è il time to fire della transizione $t_i,\forall t_i\in T_e(m)$.
- $t_{\star}$ è un evento fittizio, detto gound, che corrisponde al tempo di ingresso nella classe.
##### Esistenza del successore
Una classe di stati $S=\langle m,D\rangle$ dove $D$ è in forma normale DBM ha uno stato successore tramite $t_0$ se e solo se $t_0$ è progressing in $m$ e di accetta le soluzione dove $\tau(t_0)$ è minore o uguale dei time to fire delle altre transazioni progressing, cioè se il dominio ristretto
$D_{t_0}=\begin{cases} \tau(t_i)-\tau(t_j)\le b_{ij}\\\tau(t_0)-\tau(t_h)\le0\\\forall t_i,t_j\in T_e(m)\cup\{t_{\star}\}\mbox{ con }t_i\ne t_j,\forall t_h\in T_{pr}(m)-\{t_0\}\end{cases}$ è non vuoto, dove $T_{pr}(m)$ è l'insieme delle transizioni progressing in $m$.
- Trovare il successore ha una complessità lineare nel numero delle transizioni abilitate.
### Approssimazione della DBM
- [slides](approssimazione%20della%20DBM%20nelle%20PTPN.pdf).
##### Dominio dei tempi di un symbolic run
Consideriamo un symbolic run $\rho=S_0\xrightarrow{t^0_{f(0)}}S_1\cdots S_{N-1}\xrightarrow{t^{N-1}_{f(N-1)}}S_N$, dove:
- $S_0=\langle m_0,D_0\rangle$ è la classe di stato iniziale.
- $S_n=\langle n,D_n\rangle$ è l'$n-esima$ classe di stato visitata dal $\rho$, $\forall n\in\{1,\cdots,N\}$.
- $f(n)$ è l'indice della transizione che ha sparato nella classe $S_n$, $\forall n\in\{0,\cdots,N\}$.
- $t^k_i$ è una transizione newly enabled nella classe $S_k$.
- $I(t^k_i)$ è l'indice dell'ultima classe in cui $t^k_i$ è stata persistente.
- $c_n(t_i)$ vale 1 se e solo se $t_i$ è progressing nella classe $S_n$.
- $\sigma_n$ è il tempo di soggiorno nella classe $S_n$.

Con queste premesse il dominio dei tempi di un symbolic run è [questo](dominio%20dei%20tempi%20di%20un%20symbolic%20run%20PTPN.pdf).
- Se $D_\rho$ non ammette soluzioni allora $\rho$ è un percorso sbagliato.
- Se $D_\rho$ ammette soluzioni allora si devono eliminare i falsi comportamenti introdotti con l'approssimazione; questo si può fare risolvendo un problema di programmazione lineare (tramite ad esempio il metodo del simplesso).