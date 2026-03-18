Pattern architetturale per [sistemi distribuiti](sistemi%20distribuiti.md). C'è uno o più server che gestiscono le richieste dei vari client. L'utente interagisce con un programma sul proprio dispositivo e quest'ultimo interagisce con un server remoto, che contiene la logica applicativa.
## Layer
I sistemi client-server sono costituiti da più layer:
- Presentazione
	Presenta le informazioni e gestisce l'interazione con i client.
- Data handling
	Gestisce i dati ed esegue i vari controlli su di essi. Condivide le informazioni tra i client.
- Application processing
	Contiene la logica applicativa e realizza le funzioni per l'utente.
- Database
## Client
- Thin-clinet model
	Lato client è presente solo il layer di presentazione, mentre tutto il resto è implementato lato server. Porta ad avere un un carico di lavoro elevato sul server e sulla rete.
- Fat-client model
	Lato server sono implementati i layer di data handling e databse. Il layer di presentazione è implementato lato client. Il layer di processing può essere implemento in entrambi i lati. Porta ad avere una buona parte (O tutta) dell'esecuzione in locale; inoltre ogni aggiornamento deve essere scaricato su ogni client.
## Server
- Two-tier
	Logicamente c'è un solo server (Fisicamente può essere un componente ridondato) e un insieme di client che usano il server.
- Multi-tier
	Logicamente ci sono più server (Fisicamente possono essere sulla stessa macchina o su macchine diverse) e su ognuno di essi può essere installato uno o più layer. Evita problemi di scalabilità in modelli thin-client e problemi di gestione in modelli fat-client. Alcuni esempi sono l'IoT e gli shop online.
## Code migration
- Normale
	Non c'è mobilità ne di codice ne di esecuzione. Si tratta di thin-client.
- Remote evaluation
	C'è mobilità di codice ma non di esecuzione. Il client produce del codice e questo viene eseguito sul server. Si tratta di thin-client.
- Code on demand
	C'è mobilità di codice ma non di esecuzione. Il server fornisce il codice al client in base all'operazione da eseguire; l'esecuzione avviene lato client. Si tratta di fat-client.
- Mobile agent
	C'è mobilità di codice e di esecuzione. Sia il client che il server sono in grado di gestire il tutto; questo può portare a sospendere l'esecuzione da un lato e continuarla dall'altro (Supposta la compatibilità). Si tratta di fat-client. Un esempio sono le macchine virtuali, dove si può mettere in pausa e riprendere.
## Conseguenze
- Vantaggi:
	- Il server può trovarsi in ogni luogo e può essere acceduto anche da remoto.
	- Le funzionalità sono implementate solo nel server e non molte volte quanti sono i client.
- Svantaggi:
	- Ogni server è un single point of failure.
	- Le performance possono diminuire se il server è difficile da raggiungere. Inoltre dipendono dalla congestione della rete nel caso di sistemi distribuiti.