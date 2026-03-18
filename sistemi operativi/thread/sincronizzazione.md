La sincronizzazione è un meccanismo usato in programmazione concorrente che permette di coordinare più [thread](thread.md) che accedono alla stessa area di memoria condivisa (i.e., critical section). Garantisce quindi la proprietà [safety](proprietà.md#Safety) di mutua esclusione.
# Atomicità
Le operazioni per implementare la sincronizzazione devono essere eseguite in modo atomico, cioè si deve garantire che quando un thread vuol bloccare una risorsa sia l'unico che lo può fare (se la risorsa non è già posseduta da altri).
Per garantire l'atomicità è fondamentale che la sincronizzazione avvenga senza interruzioni, e per fare questo è fondamentale che sia l'hw a gestire queste operazioni. Quello che solitamente succede è che l'hw blocca sia la cache che il bus: in questo modo solo un thread può valutare un'area di memoria e modificarla; per questo motivo la sincronizzazione è così onerosa.
A seconda del processore questo viene implementato in modo diverso: in Intel ad esempio le istruzioni assembly possono essere precedute da $lock$.
# Meccanismi
Ci sono vari meccanismi per implementare la sincronizzazione.
##### Lock
Una variabile lock (o mutex) ha due stati: locked e unlocked.
Ci sono invece due operazioni fondamentali: $lock(l)$ permette di acquisire la variabile in modo che nessun thread potrà acquisirla prima che essa sia rilasciata dal thread che la possiede; $unlock(l)$ libera la risorsa e permette ad altri thread di acquisirla.
Siccome gli altri thread che volgiono usare la risorsa bloccata sono in attesa su essa, è fondamentale limitare il codice compreso tra $lock$ e $unlock$.
##### Semaphore
È un'estensione del lock, dove un semaforo ha un contatore (i.e., un intero) associato che rappresenta il numero di risorse libere di quella variabile.
L'operazione che acquisisce un istanza del semaforo, e che quindi decrementa il contatore, è detta $wait$; l'operazione che libera un'istanza del semaforo è detta $signal$.
I semafori sono strutture più complessi di un semplice lock; se non sono necessari è meglio usare il lock.
##### Monitor
È un meccanismo di sincronizzazione di alto livello, complesso e costoso in termini di cicli di efficienza.
Permette la definizione di strutture dati le cui operazioni a supporto sono bloccanti, cioè possono essere eseguite da un solo thread alla volta.