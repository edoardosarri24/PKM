OpenFlow permette la [riprogrammazione](data%20plane.md#Riprogrammazione) dei dispositivi del [data plane](data%20plane.md) da parte del [control plane](control%20plane.md).
OpenFlow fornisce sia il protocollo di comunicazione tra il controller e il dispositivo di rete, sia le API per loro programmazione.
Il suo compito è astrarre l'hardware del dispositivo al controller: chi deve definire le regole di forwarding non deve sapere il costruttore del dispositivo di rete, ma gli basta sapere che si tratta di un OpenFlow switch.
# Switch
Il dispositivo di rete, noto come OpenFlow switch, può essere un hardware di qualunque tipo, ma deve avere delle [caratteristiche essenziali](OpenFLow%20switch.png) perché dal controller sia visto come un dispositivo OpenFlow.
##### Porte
Più di una porta in ingresso e in uscita per la ricezione e inoltro dei pacchetti di rete.
Possono essere sia fisiche che logiche: le prime corrispondono a una porta nel dispositivo; le secondo possono essere ad esempio aggregazione di porte fisiche viste come un'unica porta o una porta che richiede di trattare un pacchetto in un modo particolare.
Ci sono delle porte che sono riservate e che compiono azioni predefinite: $all$ è la porta logica di invio a tutte le porte fisiche tranne quella di ingresso del pacchetto; $controller$ è la porta relativa all'OpenFlow channel.
##### OpenFlow channel
Su questo canale circolano solo pacchetti di controllo. Separando questa interfaccia con le porte si ottiene il decoupling tra controller e pacchetti di rete.
Ci sono più tecniche di comunicazione:
- Controller to switch
	È il controller che parla con lo switch.
	- Aggiornare e/o eliminare una o più regole.
	- Inviare un pacchetto allo switch chiedendogli di inviarlo.
- Asynchronous
	È lo switch che parla con il controller.
	- Inviare un pacchetto al controller perché non sa cosa farne.
	- Avvisare il controller che una regola è scaduta.
- Symmetric
	È una comunicazione bidirezionale.
	- Comunicazione $Hello$ per stabilire la connessione.
	- Ping per vedere se l'altro è attivo.
##### Tabelle
Le tabelle contengono le regole di inoltro che devono essere applicate ai pacchetti. Non è presente una singola tabella, ma c'è pipeline che viene seguita.
- Flow tables
	Uno switch può avere più tabelle (si parte dalla tabella 0). Questo permette di realizzare il [Single responsability principle](Single%20responsability%20principle.md): ogni tabella gestisce il controllo di una sola parte. Inoltre si evita di avere regole molto complesse con molte combinazioni logiche.
- Group tables
	Un gruppo è un insieme di bucket, dove un bucket è un insieme di azioni; se un pacchetto viene assegnato a un gruppo (tramite l'action in sequito a un match su una flow table) allora viene scelto uno o più (se più il pacchetto viene duplicato) buckets da cui far elaborare il pacchetto. Il tipo di gruppo permette di definire come scegliere il bucket:
	 - ALL
		Il pacchetto viene duplicato su tutti i bucket.
	 - SELECT
		Lo switch deve decidere un solo bucket.
		Possiamo stabilire un peso con cui scegliere il bucket (e.g., 50% da una parte e 50% dall'altra).
	 - FAST FAILOVER
		Ogni bucket ha una priorità; si parte da quella con priorità più alta; se questa non è disposibile allora si passa a quella dopo. Richiede quindi almeno due bucket.
	 - INDIRECT
		È un'astrazione per regole che hanno comportamenti comuni. In questo modo se vogliamo modificare il comportamento di tutte queste regole dobbiamo toccare un solo punto.
- Meter tables
	È una tabella che permette di implementare la [QoS](Reti/misure.md#QoS) tenendo conto di alcune metriche (e.g., throughtput con pacchetti/secondo). L'idea è di raccogliere informazioni sulle performance dello switch, e di comportaresi di conseguenza per evitare che la velocità di inoltre diminuisca di un certo valore soglia.
	Un possibile uso è implementare degli $\texttt{if-then}$ basati sulle performance: se il pacchetto fa eccedere la soglia della banda allora si modifica un campo dell'header IPv4 (i.e., parte del campo [type of service](IPv4.md#HEADER)); a questo punto si può mandare il pacchetto ad un'altra tabella che fa il match su questo campo cambiato e quindi inviare il pacchetto al controller per poi prendere decisioni.
##### Entry filed
Le tabelle sono composte da regole dette entry. Ogni entry ha dei campi che la definiscono.
- Campi per il maching.
- Piority
	Se un pacchetto fa matching con più regole si seleziona quella a priorità più alta.
- Timeouts
	Evita di dover cancellare le regole a mano.
- Cookie
	È un'etichetta che il controller associa alle regole per ritrovarle velocemente.
- Counters
	Sono indicatori statistici usati dal controller.
- Istruzione
	L'istruzione da eseguire se il matching è verificato.
# Pipeline
Quando entra un pacchetto si segue una pipeline ben precisa:
- Il pacchetto inizia a cercare un match nella tabella 0.
- Matching
	Il matching avviene su campi specifici (OpenFlow può vedere tutti i livello dello stack [TCP/IP](TCP%20IP.md)): porta di ingresso, indirizzo MAC (e.g., solitamente ethernet) sorgente e destinazione, indirizzo IP sorgente e destinazione e porte TCP o UDP.
	Se un match viene trovato allora: si incrementano i contatori associati all'entry; si eseguono le azioni relative a tale entry (e.g., aggiornare i metadati).
- Non matching
	Se non abbiamo un match e non abbiamo una default entry allora il pacchetto viene scartato.
- Se non abbiamo un match ma abbiamo una deafult entry allora si esegue una delle sue azioni: si manda il pacchetto al controlle, che può scartarlo o definire una nuova regole; si invia il pacchetto a un'altra tabella con un $GoTo$; si scarta il pacchetto.
- Quando il pacchetto ha visitato tutte le tabelle che doveva allora si eseguono le azioni raccolte fino a questo momento, eventualmente si eseguono i processi egress, e poi si inoltra il pacchetto.
##### Processi
A seconda di quando le cose vengono fatte possiamo categorizzare due tipi di processi:
- Ingress
	È sempre svolto ed è il processo che inizia quando il pacchetto entra nella porta dello switch e finisce quando si determina la porta di uscita.
	Consiste nell'attraversamento della pipeline di tutte le tabelle e della gestione delle azioni.
- Egress
	È eseguito dopo aver determinato la porta di uscita e prima dell'invio del pacchetto.
	Consiste in azioni dell'ultimo minuto sul pacchetto, spesso associate a una determinata porta di output.
##### Azioni
Durante la pipeline ogni pacchetto raccoglie una serie di azioni in una lista. Quando il pacchetto attraversa le tabelle e fa match con più entry, le varie azioni sono scritte in questa lista e poi sono eseguite quando il processo di attraversamento delle tabelle è finito; eventualmente è possibile che un'azione venga eseguita subito e non scritta nella lista.
Le possibili azioni sono:
- $GoTo$
	Per mandare il pacchetto a una nuova tabella. Questa non necessariamente deve essere una flow table.
- Write metadata
	I metadati sono informazioni che non fanno parte del pacchetto ma che sono correlate al pacchetto durante il suo ciclo di vita all'interno dello switch.
- Clear action
	Serve per rimuovere un'azione dalla lista.
- Write header
	Serve per modificare direttamente il pacchetto.