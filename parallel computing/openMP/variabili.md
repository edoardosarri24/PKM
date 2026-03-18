La gestione delle variabili in [openMP](openMP.md) è semplificata dalle keyword delle pragma che possiamo aggiungere ad esse.
# Tipo
Con riferimento al momento in cui il main thread fa una fork tramite la pragma [parallel](parallel%20region.md), le variabili si dividono in tre tipi.
##### Globali
Le variabili globali possono essere accedute da qualunque thread con il loro nome , sono quindi comune a tutti i thread.
Si deve stare attenti alla [sincronizzazione](sistemi%20operativi/thread/sincronizzazione.md).
##### Locali al main thread
Se una variabile è dichiarata nel main thread prima del fork allora è anche nello scope dei fork thread; è una variabile locale al main thread, ma dal punto di vista dei fork thread è come se fosse globale.
Si deve stare attenti alla [sincronizzazione](sistemi%20operativi/thread/sincronizzazione.md).
##### Locali ai fork thread
Tutte le variabili dichiarate all'interno del blocco parallel sono locali ai singoli thread in quanto si trovano nello [stack del thread](thread.md#Stack) stesso thread; possono essere accedute e modificate senza problemi.
# Dichiarazioni
Quando si creano i thread paralleli con la pragma [parallel](parallel%20region.md) possiamo avere controllo sulle variabili:
- $shared(list)$
	Indica che le variabili sono condivise tra tutti i thread. Si deve stare attenti alla sincronizzazione.
- $private(list)$
	Permette di avere una copia con lo stesso nome all'interno dello stack del thraed.
	Il problema può essere l'inizializzazione, che in questa situazionenon avviene in automatico. Ci sono tre modi per inizializzare la variabile privata:
	- Eslicitamente nel thread.
	- firstprivate
		Quando la variabile viene definita viene anche inizializzata con lo stesso valore di quella del main thread.
	- lastprivate
		Quando il fork thread termina, il valore della variabile privata viene copiato nella variabile del main thread.
		È importante considerare che se abbiamo più thread allora il valore della variabile nel main thread diventerà quello della variabile dell'ultimo fork thread che ha completato.
		La variabile viene solo dichiarata e non inizializzata automaticamente.
- $default(list)$
	È il comportamento di default per le variabili che non sono dichiarate come $shared$ o $private$.
	In C e C++ può essere solo $shared$ o $none$. È una buona pratica usare $deafult(none)$ in modo da essere forzati a dichiararle tutte.
# Threadprivate
La direttiva è $\#pragma\ omp\ threadprivate(list)$ permette di allocare nello stack dei [thread](thread.md) una copia locale della variabile globale specificata persistente tra le regioni parallele (dichiarata con [parallel](parallel%20region.md)).
##### Funzionamento
Quando una regione parallela finisce (i.e., si fa un join) tutto lo stato (i.e., le variabili locali) dei thread viene solitamente deallocato.
Tramite $threadprivate$ stiamo dicendo che, all'inizio della prossima regione parallela, i thread avranno ancora la propria compia di tale variabile globale e che questa copia sarà inizializzata con il valore che avevano definito prima dell'ultimo join.
##### Inizializzazione
Anche se una variabile globale viene specifica nella direttiva $thradprivate$, è il fork thread che si deve occupare di definirla, cioè di assegnargli un valore.
##### copyin
Assegna alla copia locale persistente della variabile globale il valore attuale di tale variabile globale.
Può essere usato per l'inzializzaione della variabile locale.