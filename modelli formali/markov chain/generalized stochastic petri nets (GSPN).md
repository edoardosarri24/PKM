Le Generalized Stochastic Petri Nets (GSPN) possono essere viste delle [reti di petri](Petri%20Net%20(PN).md) che sostituiscono il non determinismo con la stocasticità.
Sono risolte risolvendo la [markov chain](markov%20chain.md) che è associata alla rete. Questo è possibile visto che i tempi di esecuzione delle transizioni sono esponenziali; inoltre soddisfano anche la [condizione di time-homogeneous](condizione%20di%20time-homogeneous.md).
# Definizione
Formalmente sono definite come $GSPN=\langle P,T,A^-, A^+, A^{\bullet}, M_0, R\rangle$, dove rispetto alle PN abbiamo:
- $T$ è l'insieme delle transizioni ed è partizionato in $T^{IMM}$ e $T^{EXP}$.
- $R$ è una funzione che associa un reale a ogni transizione a seconda sella marcatura in cui ci troviamo, cioè $R:T\times\mathbb{N}^{\#P}\to\mathbb{R}$.
##### Stato
Lo stato coincide con la marcatura.
##### Transizioni
Ci sono due tipi di transizioni ed entrambe dipendono dalla marcatura.
- Immediate
	Sono rappresentate con una barra sottile. Se da un posto escono più transizioni (cioè c'è una scelta, uno switch), allora a ognuna di esse è legato un peso, cioè una probabilità.
	In questo caso, se $t_e\in T^{IMM}$, allora $R(t_e,m)$ rappresenta la il peso probabilistico associato di $t_e$; detto questo abbiamo che $Prob\{t_e\ fires\}=\tfrac{R(t_e,m)}{\sum_{t_h\in T^{IMM},en(m)}R(t_h,m)}$, dove il denominatore è la somma dei pesi di tutte le transizioni abilitate dalla marcatura $m$.
- Esponenziali
	Sono rappresentate con una barra larga. Una volta che sono state abilitate il loro firing time è campionato da una [distribuzione esponenziale](distribuzione%20esponenziale.md) con un dato rate.
	In questo caso, se $t_e\in T^{EXP}$, allora $R(t_e,m)$ rappresenta il rate di $t_e$; detto questo abbiamo che $Prob\{t_e\ fires\}=\tfrac{R(t_e,m)}{\sum_{t_h\in T^{EXP},en(m)}R(t_h,m)}$, dove il denominatore è la somma dei rate di tutte le transizioni abilitate dalla marcatura $m$.
# Tempo di soggiorno
Se voglio calcolare il tempo di soggiorno in un posto che ha più rami uscenti, allora in teoria dovrei campionare un valore dalle distribuzioni e vedere quale è il minimo; questo è lineare nel numero di nodi, dove ogni operazione comporta il campionamento in $[0,1]$ da una distribuzione uniforme e poi il calcolo del logaritmo (questo per la [generazione](distribuzione%20esponenziale.md#Random%20generator) di un numero casuale).
C'è una tecnica che ci permette di calcolare il tempo di soggiorno in un dato stato in tempo costante.
# Parallelizzazione
Se abbiamo più job da eseguire, rappresentati da più token in un posto, allora possiamo avere tre situazioni.
##### Parallelo
![parallelo](parallelo.png)
In questo caso il rate diventa pari al rate base (nell'esempio 1) moltiplicato per il numero di token presenti nel posto in ingresso.
Questa modellazione rappresenta 4 server che con lo stesso tasso gestiscono 4 job: si hanno più risorse rispetto alle altre due soluzioni, è la soluzione più veloce e quindi non paragonabile.
##### Sequenziale
![sequenziale](sequenziale.png)
Ogni job viene servito con lo stesso rate (nell'esempio sempre 1).
##### Parallelo equalizzato
![parallelEqualized](parallelEqualized.png)
In questo caso abbiamo un singolo server che gestisce più job ma con tassi diversi: il tasso di ogni job è dato dal prodotto del valore per il numero di gettoni; quel valore (nell'esempio 0,52) è tale che la media su tutti i job è la stessa del sequential.
Osservando che il primo job ha tasso 2 e l'ultimo ha tasso 0,5, allora questo tipo di modellazione è utile, rispetto alla sequenziale, quando la deadline è più lontana della metà, visto che gli ultimi job hanno tasso più basso. In pratica vuol dire che inizia più piano del sequential, ma finisce più veloce.
# Esempio
![esempio](esempio-modello.png)
Supponiamo di avere questo scenario, dove la corretta esecuzione è data da $p0:t_0\to t2\to t4\to t3\to t5:p6$. Ci possono essere due fallimenti: se $t1$ è attivata al posto di $t2$ la richiesta è rifiutata; se $t3$ è attivata prima di $t4$ il sistema va in deadlock.
Un esperimento valuta la probabilità di terminare in un nodo rispetto al tempo. Tale probabilità è ottenuta da un tool tipo [ORIS 2](ORIS%202.md) e si può visualizzare in modo efficace tramite un grafico.
![esempio-grafico](esempio-grafico.png)
In questo caso si può osservare che:
- La posizione $p1$ non è rappresentata perché le transizioni uscenti sono immediate; si tratterà di quale scegliere ma di sicuro il tempo di soggiorno in questo stato è pari a 0.
- Abbiamo detto che le transizioni $EXP$ campionano un valore da una distribuzione esponenziale, ma il grafico risultato è chiaramente non esponenziale. Questo è dovuto al fatto che la rete è un insieme di [proprietà](distribuzione%20esponenziale.md#Proprietà) che non restituiscono un esponenziale.
- Un esperimento di questo tipo è chiarament ottimo ad esempio per valutare dopo quanto tempo conviene (a livello probabilistico) un ripristino del sistema.