Syncio è un framework che frutta la programmazione asincrona con le limitizaioni del single thread imposte dal [GIL](python%20threads.md#GIL) di Python: un task può essere eseguito in background separandosi dall'applicazione principale solo se si gestisce un'operazione di I/O.
Asyncio quindi non evita il GIL, ma lo usa in quelle situazioni dove non abbiamo operazioni non cpu-bound.
# Event loop
Un event loop, che è il cuore di Ayncio, mantiene una coda di task; quando il task in esecuzione affronta un'operazioni di I/O si sospende in favore di un altro task che deve eseguire sulla cpu; quando il task termina l'opera di I/O allora rientra nella coda dei task pronti.
Questo meccanismo ha un grande vantaggio quando ci sono più task che eseguono operazioni non solo cpu bound in momenti diversi.
# Coroutines
Una coroutine è una funzione dichiarata con $async$ che può sospendersi in modo bloccante (usando $await$, i.e., non esegue in parallelo ma si ferma) e continuare in un secondo momento. Si parla in questo caso di cooperative multitasking: è il task che si sospende perché incontra un'operazione che sa essere un'attesa (i.e., una I/O) e permette ad altri task più rapidi di eseguire prima.
##### Esecuzione
Il flusso di esecuzione è il seguente: quando un task sa che la prossima operazione è onerosa allora la chiama con $await$ davanti; questo fa si che la coroutine si blocchi a favore di un'altra e che riprenda da quel punto solo quando viene richiamata dall'[event loop](#Event%20loop).
# Task
Un task è un wrapper di una coroutine (i.e., si crea con $\texttt{asyncio.create\_task(coroutine)}$) che la schedula sull'[event loop](#Event%20loop) in modo non bloccante.
##### Differenza con coroutine
Rispetto a una semplice coroutine, quando essa viene wrappata in un task, le operazioni definite con await diventano non bloccanti: quando ci sono più task che attendono un'operazione I/O, cioè più task che sono sospesi su una chiamata $await$ della coroutine che incapsulano, allora essi la eseguono in parallelo, ed è come se il tempo passasse per entrambi. Quando poi devono eseguire nuovamente operazioni cpu-bounded perché quelle I/O-bounded sono terminale, lo scheduler dell'event loop manda in esecuzione un task che è diventao libero.
##### Stati
I task permettono la sincronizzazione del chiamante rispetto alla loro terminazione:
- Termine
	Si può osservare la terminazione di un task con il suo metodo $\texttt{done()}$.
- Risultato
	Quando la coroutine incapsulata in un task deve restituire un risultato si può fare in più modi:
	- Usando $\texttt{await task}$, dove task era stato restituito da $\texttt{asyncio.create\_task(coroutine)}$. È il metodo migliore che permette di sincronizzare il chiamante del task sul risultato.
	- Dopo aver controllato la terminazione con $\texttt{done()}$ si può chiamare $\texttt{result()}$ sul task. 
##### Cancellazione
- Cancellazione
	Per gestire problemi di rete o di I/O, un task può essere cancellato con $\texttt{cancell()}$. Questa chiamata provoca il sollevamento di una $CancelledError$ che deve essere gestita dalla coroutine (i.e., liberare le risorse di rete), altrimenti si solleva fino al chiamante di $cancell$.
- Timer
	L'impossibilità di svolgere l'operazione di I/O si può gestire con un timer tramite $\texttt{asyncio.await\_for(aw,timer)}$, che prende un awaitable e un tempo: restituisce il risultato di $aw$ se l'oeprazione finisce prima di timer, altrimenti una $asyncio.TimeoutError$.
- Protezione
	Se c'è una parte critica di una coroutine (i.e., una parte della funzione di un task) che non deve essere cancellata quando sta eseguendo, allora si può wrappare con $\texttt{asyncio.await(aw)}$. In questo modo se il task esterno viene cancellato non si verificano problemi allora parte critica.
##### Eccezioni
Quando viene sollevata eccezione sul task, questo viene considerato terminato e il risutlato è l'eccezione stessa.
Se il chiamante non prende il risultato con $result()$ o $await\ task$ allora non ci accorgiamo di possibili problemi.
# Gather
Usando $\texttt{asyncio.gather}$ con più awaitable come parametri si permette di eseguire in più task concorrenti in una sola chiamata.
Il risultato di ogni awaitable è restituiti come lista da questa chiamata nell'ordine in cui sono passati gli awaitable.
##### Eccezioni e risultati
La chiamata ritorna, nell'ordine in cui sono stati passati come parametri, i risultati o le eccezioni degli awaitable lanciati solo quando tutti hanno finito.
Possiamo anche farci restituire un risultato subito quando è pronto e no tutti insieme per iterarne uno alla volta; questo usa i Future.