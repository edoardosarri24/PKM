Il multiprocessing in python è uno dei modi insieme al [multiprocessing](python%20multiprocessing.md) per evitare il [GIL](python%20threads.md#GIL). Questo crea processi con tutte le conseguenze: più complessi; comunicazione tramite IPC e non shered memory.
Per usarlo su usa il modulo python $multiprocessing$.
# Process
Dato un main eseguito da un processo, il pattern seguito è il fork-join: si costruisce il processo; si chiama $start$ su un processo per inizialo e si chiama $join$ per sincronizzarci alla sua fine.
Ci sono due tecniche per creare [processi](processo.md). Dato 
- Funzioni
	Si crea un oggetto della classe $Process$ e gli si passa un $target$ (i.e., la funzione da eseguire) e un $args$ (i.e., i parametri della funzione).
- Classe
	Si estende la classe $Process$ ridefinendo il costruttore e il metodo $run$.
##### Demone
Un processo demone, oggetto di $Process$,  termina quando il processo padre termina, ma il processo padre non deve aspettare la sua terminazione per terminare.
# Comunicazione
Per comunicare i processi devono usare l'IPC.
##### Queque
Sono code di lavoro che implementano il pattern produttore-consumatore in modo thread-safe: il main process inserisce i task che i processi prelevano ed eseguono. Solitamente la terminazione è fatta inserendo un task $None$: il codice del processo dovrà controlla sia passato un task e non un $None$.
Deve essere creata nel main e passati ai processi quando questi sono creati. Una coda è un oggetto di $Queque$ o di $JoinableQueue$: la seconda permette di tenere traccia dei task completati e no.
##### Pipe
Permette di stabile una connessione punto-punto. Si usa solitamente solo nelle situazioni in cui si deve definire una pipeline.
##### Shered memory
Possiamo condividere dati semplici di tipo $Value$ (i.e., $int$ o $double$) o vettori di tipo $Array$ (solo tramite $numpy$ che permette la serializzazione e deserializzazione).
Solitamente queste è meglio usare il contesto $with$ rispetto ai lock o semafori.
# Pool
Invece che creare processi manualmente e avere il costo di creare singoli processi, si possono creare pool di processi tramite la classe $Pool$. Questo ha vantaggi se i processi creati vengono riutilizzati frequentemente.
Ci sono dei metodi per far eseguire funzioni ($apply$) o applicare mappature ($map$) a tutti i processi del pool.