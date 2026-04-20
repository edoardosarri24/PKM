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
	Sono tabelle che permettono di definire dei gruppi, dove ogni gruppo è composto da più bucket, dove un busket è una lista di azioni.
	A seconda del tipo di gruppo la selezione dell'azione è diversa:
	 - ALL
		Si eseguono tutti i bucket.
	 - SELECT
		Lo switch deve decidere un solo bucket.
	 - FAST FAILOVER
		Si seleziona un bucket; se questo non è disponibile allora si passa al successivo. Richiede almeno due bucket.
		Si seleziona l'azione secondo priorità; se la prima azione non può essere eseguita allora si passa a quella dopo.
	 - INDIRECT
		È un'astrazione per regole che hanno comportamenti comuni. In questo modo se vogliamo modificare il comportamento di tutte queste regole dobbiamo toccare un solo punto.
- Meter tables
	È una tabella che permette di implementare la [QoS](Reti/misure.md#QoS) tenendo conto di alcune metriche (e.g., throughtput). L'idea è di raccogliere informazioni sulle performance dello switch, e di comportaresi di conseguenza per evitare che la velocità di inoltre diminuisca di un certo valore soglia.
##### Entry filed
Le tabelle sono comporte da regole dette entry. Ogni entry ha dei cambi che dicono allo switch come essa deve essere gestita.
- Piority
	Se un pacchetto fa matching con più regole si seleziona quella a priorità più alta.
- Timeouts
	Evita di dover cancellare le regole a mano.
- Cookie
	È un'etichetta che il controller associa alle regole per ritrovarle velocemente.
- Counters
	Sono indicatori statistici usati solitamente dal controller.
# Pipeline
Quando entra un pacchetto si segue una pipeline ben precisa:
- Il pacchetto inizia a cercare un match nella tabella 0.
- Matching
	Il matching avviene su campi specifici (OpenFlow può vedere tutti i livello dello stack [TCP/IP](TCP%20IP.md)): porta di ingresso, indirizzo MAC, indirizzo IP e porte TCP o UDP. 
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