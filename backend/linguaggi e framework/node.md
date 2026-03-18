Si tratta di un runtime environment per [javascript](javascript.md) [open-source](https://nodejs.org/en) basato sul motore V8, quello che esegue codice JavaScript in Google Chrome.
Si distingue perché, grazie al suo modello di I/O asincrono e event-driven, è perfetto da usare sul [backend](backend.md) con lo stesso linguaggio del [frontend](frontend.md).
# Conseguenze
##### Vantaggi
Si deve usare quando:
- Un'app è I/O bound, visto che è asincrono.
- Abbiamo poche risorse a disposizione, visto che è single thread.
- Vigliamo avere la semplicità di un single language.
##### Svantaggi
Non è da usare quando:
- Il backend fa molte operazioni CPU bound.
- Vogliamo un server con un'alta resilienza: se il main thread crusha allora il server si ferma.
# Event loop
Si tratta del main thread di Node.
Si occupa principalmente di delegare le operazioni di I/O, che saranno quindi eseguite in modo asincrono, e passare alla prossima istruzione.
# I/O asincrono
Nei classici server per gestire più utenti si devono avere più [thread](thread.md): quando un thread inizia a gestire una richiesta la porta fino alla fine. Questo vuol dire che se abbiamo una chiamata a un DB allora il thread rimane in attesa.
##### Pipeline
In Node si ha un paradigma a singolo thread asincrono:
- Quando incontra una richiesta lenta come un'operazione di I/O la delega a [Libuv](#Libuv) e passa all'istruzione successiva.
- Quando tale richiesta termina si inserisce in una coda di eventi detta task queque.
- L'event loop nel suo ciclo deve controllare se ci sono callback pronte e le esegue.
##### Libuv
Sostanzialmente Node è wrapper di V8 e Libuv, una libreria  [C++](C++.md) che permette di realizzare il meccanismo di asincronia.
Ci sono due modi con Libuv realizza questa asincronia:
- Kernel
	Il kernel espone delle API per gestire le operazioni di rete (i.e., TCP, UDP e HTTP) in modo asincrono. 
- Pool thread
	Quando il kernel non ha nelle API asincrone per l'operazione che stiamo aspettado (e.g., su un file system spesso ci sono solo API bloccanti) o per quei casi in cui si devono fare dei calcoli CPU bound che bloccherebbe l'event loop, allora si usa il pool thread. Di deafult questi sono 4 ma si possono aumentare fino al numero di core della macchina.
# Dati
Tutto quello che riguarda l'I/O in Node è gestito come un flusso di dati e non come una stringa o un oggetto.
##### Observer
Il paradigma dell'[observer](observer.md) è implementato dalla classe $\texttt{EventEmitter}$: un emitter invia un segnale che è raccolto da uno o più listener.
##### Streams
Nei classici server abbiamo dei problemi quando passano dati molto pesanti visto che la memoria sarà saturata; in Node questo è risolto con il meccanismo degli streams.
Ci sono quattreo tipi di streams: sola lettura, sola scrittura, lettera/scrittura, transofrm che modifica i dati mentre passano.
Un'implementazione interesante di Node è la backpressure: se uno stream veloce invia i dati a uno lento, la velocità è definita da quello lento.
##### Middleware
In node i dati possono essere gestiti da può responsabili: si implementa perfettamente il [chain of responsability](chain%20of%20responsability.md).
Esempio: Una richiesta HTTP può essere gestita da più oggetti, ognuno dei quali è detto middleware.
# Database
In Node abbiamo tre modi per gestire la persistenza:
- Native drivers
	È la soluzione più efficiente, ma richiede di scrivere le query direttamente nel linguaggio del DB scelto.
- Query builder
	Interfaccia per scrivere query con un livello di astrazione maggiore.
- ORM
	Come in Java l'[ORM](ORM.md) è l'oggetto che mappa oggetti in tabelle. Lo standard de facto per TypeScritp è Prisma.
##### Connessioni
Non è fattibile a ogni richiesta al database aprire e chiudere una connesione HTTP, ci sarebbe troppo overhead.
Quello che si fa è tenere un pool di connesioni aperte: quando arriva una richiesta viene presa una connessione e restituita quando la richiesta ritorna.
##### Caching
Per evitare di fare la stessa query al database più volte in poco tempo, si può aggiungere un layer di cache in-memory usando Redis.
# NPM
Il Node Package Manager (NMP) è il registry software più grande al mondo e anche l'ecosistema di Node. Si tratta di una raccolta di moduli già pronti che permettono di realizzare app con Node molto velocemente.