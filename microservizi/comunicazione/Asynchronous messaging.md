Nella [comunicazione](microservizi/comunicazione/comunicazione.md) dei [microservizi](microservizi.md) quando si parla di comunicazione asincrona si parla di messaggi.
# Architettura
In un sistema IPC basato su messaggi, i partecipanti si basano si scambiano messaggi tramite uno o più canali:
- Messaggio
	È composto da un header e un corpo. L'header è un insieme di coppie chiave-valore che definisce sia i metadati del messaggio e le informazioni necessarie al ricevente per rispondere.
	Ci sono più tipi di messaggio:
	- Documento
		È un generico messaggio che contiene dati. Solitamente è una risposta a un messaggio di tipo comando.
	- Comando
		È un comando da eseguire con i relativi parametri.
	- Evento
		È usato per comunicare una variazione dello stato del mittente.
- Canale
	Ci sono due tipi di canali:
	- Point2Point
		È usato per inviare messaggi direttamente dal mittente al consumatore. È la tecnica usata per implementare lo stile di comunicazione [one2one](microservizi/comunicazione/comunicazione.md#Stili).
	- Publish-Subscriber
		È usato per inviare messaggi a più subscriber dello stesso canale. È la tecnica usata per implementare lo stile di comunicazione [one2many](microservizi/comunicazione/comunicazione.md#Stili). Esiste sia la versione broker-based (si usa solo qeusta) che brokerless.
# Implementazione
La tecnica di usare messaggi è molto potente e permette di implementare tutti gli [stili](microservizi/comunicazione/comunicazione.md#Stili) di IPC:
- Request/Response e Request/Response Asynchronous
	Siamo nella situazione in cui un client invia un messaggio a un singolo ricevente che deve rispondere. Siccome siamo in una situazione one2one si utilizza un canale point2point; nel messaggio si dovrà inserire un id del messaggio usato per accoppiare messaggi.
	Il client che invia il messaggio si può mettere in attesa della risposta (sincrono) oppure proseguire stando in ascolto sul canale (asincrono).
- One-way notification
	Un client utilizza un canale point2point per inviare un messaggio a un ricevente. Prosegue poi senza mettersi in ascolto su nessun canale.
- Publish/subscriber
	Il client pubblica un messaggio su un canale e i vari subscriber si mettono in ascolto su questo canale. Solitamente si usa un canale per tipo o dominio di messaggio.
- Publish/Async Response
	È lo stesso caso di cui sopra ma con il client che deve mettersi in ascolto su un canale (specificato nel messaggio) su cui i subscriber devono rispondere.
# Broker
In un'IPC basato su messaggi si può usare o meno un intermediario (solitamente si), detto broker.
##### Brokerless
Senza un broker i partecipanti si scambiano i messaggi direttamente. Si può avere comunicazioni sia one2one che one2many. Una tecnologia è [ZeroMQ](https://zeromq.org).
- Vantaggi
	- C'è meno traffico sulla rete
	- La latenza è minima.
	- Il broker non è un collo di bottiglia o un SPF.
	- Non si deve configurare il broker.
- Svantaggi
	- I partecipanti devono conoscersi. Spesso è necessario un service discovery.
	- Si riduce l'availability: i partecipanti devono essere raggiungibili quando il messaggio viene mandato e ricevuto.
##### Broker-based
Il broker è un intermediario tra ogni partecipante alla comunicazione. Ci sono varie scelte di broker open source (e.g., Apache Kafka e RabbitMQ) e altrettante per applicazioni cloud-based (e.g., AWS Kinesis e AWS SQS).
Se dobbiamo scegliere un broker ci sono molte cose che dobbiamo considerare e decidere il nostro trade-off: i linguaggi di programmazione supportati, gli standard di comunicazione supportati, l'ordinamento dei messaggi, la garanzia di invio, la persistenza, la durabilità (se un subscriber si disconnette, quando si riconnette può vedere i messaggi che sono stati mandati nel mentre?), la scalabilità e la latenza.
- Vantaggi
	- Loose coupling
	- Buffering
		Permette ai due partecipanti di non essere contemporante disponibili per l'invio di messaggi.
- Svantaggi
	- Bottleneck per le prestazioni
	- Single point of failure
	- Complessità
		Un broker richiede di essere configurato ed è un altro componente nella nostra architettura.