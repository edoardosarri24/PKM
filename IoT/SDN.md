# PRE SDN
Internet è costituito da una rete di router, i quali hanno l'obiettivo di collegare ogni coppia di host in modo global connectivity best effort, cioè senza nessuna garanzia di banda e di affidabilità.
Nel 1983 si è imposto che qualunque nodo volesse partecipare allo scambio di informazioni doveva implementare a livello kernel TCP/IP; questo permise di liberalizzare internet, il quale divenne uno strumento libero a livello commerciale.
I principi fondativi di internet sono i seguenti:
- Packet switching
	In internet i dati tra due host vengono trasmessi tramite pacchetti, composti da header e payload.
	I router decidono il percorso dei pacchetti basandosi sull'algoritmo che implementano, il quale è incorporato nel firmware ed è deciso dal produttore; i router non mettono a disposizione un API per la riprogrammazione, ma solo una GUI con cui possono essere configurati (cioè non sono riprogrammabili, ma si possono cambiare solo alcuni parametri per un po' di personalizzazione).
- Distributed control
	Il controllo della rete non è in mano a un solo dispositivo, ma la si può osservare solo se tutti collaborano tramite lo scambio di informazioni.
	Le informazioni sulla rete e sui router attivi sono scambiate dai router sotto forma di pacchetti, detti control packet; i data packet invece sono quelli che portano le informazioni che gli host si vogliono scambiare.
	Lo stato di un router, cioè l'informazione scambiata tramite control packet, si trova all'interno delle tabelle di routing: le tabelle sono scritte (cioè aggiornate) quando arriva un control packet; sono invece lette quando arriva un data packet per eseguire lo switching (operazione nell'ordine dei millisecondi).
	Uno dei problemi di questa soluzione classica è la convergenza: ogni router deve arrivare a conosce tutta la rete a partire dalla sola conoscenza dei vicini; un cambiamento è percepito dal router tanto più lentamente quanto è più lontano dal cambiamento stesso.
- Intelligence at edge
	Una maggiore intelligenza (cioè complessità, cioè livelli TCP/IP implementati) sta negli host, cioè nella periferia della rete. La rete è quindi più stupida (ha meno funzioni) degli host.
##### SDN
Il modo standard di pensare internet impone che tutti i router avessero una parte sw che prende le decisioni in base a come è stata programmata dal produttore all'interno del firmware.
Questo porta a impiegare molto tempo per portare un aggiornamento sulla rete, perché si dovevano cambiare tutti i router che definiscono l'instradamento.
Con il Software-Define Networking (SDN) si permette di costruire un ambienti che si adatta velocemente alle nuove richieste. SDN non è un protocollo: non definisce come le cose siano fatte in modo rigido, ma stabilisce solamente un'idea.
# ACRCHITETTURA
SDN [disaccopia](SDN%20architecture.png) il piani di forwarding e di controllo. Il data plane veicola il traffico dati; il control plane decide il metodo di routing.
##### Data plane
Il data plane gestisce solo il traffico dati: gli Switch non si scambiano mai pacchetti di controllo e non prendono mai decisioni che non gli sono state assegnate dal control plane.
I dispositivi che eseguono questo compito sono gli Switch. Questi dispositivi sono molto semplici e tutti uguali perchè il loro unico compito è quello di commutare i pacchetti da una porta di ingresso a una di uscita; non ci sono più le specializzazioni come prima (es: router, NAT e gateway). Espongono delle API e possono essere quindi programmabili: possiamo cambiare il loro comportamenti di routing in base alle necessità.
Nel data plane si rivede il concetto di switching: non si parla più di packet switching ma di flow switching; un flusso è un insieme di pacchetti che hanno in comune almeno un campo dell'header (es: tutti i pacchetti che provengono dalla stessa sorgente; tutti i pacchetti che hanno la stessa porta).
##### Control plane
Il controllo non è più distribuito tra i componenti della rete (router), ma viene centralizzato logicamente (fisicamente poi possono essere anche più dispositivi che lo gestiscono). Questo controllo è in mano a un processo detto [coontroller](SDN.md#CONTROLLER).
# VANTAGGI
- Avere un controllo centralizzato permette di avere una viste globale dell'intera rete in un unico punto.
- Si può bilanciare il carico a livello dei singoli Switch.
- I fallimenti della rete possono essere recuperati più velocemente.
- Si può fornire una QoS, cosa che in internet classico non era permessa. Questo è possibile perchè ogni piano (DP e CP) può scalare in modo indipendente: se si ha bisogno di una banda elevata per il traffico video allora si scalerà il data plane; se si vuole realizzare un'applicazione M2M allora sarà scalato il control plane.
- Si può costruire una sola rete e poi usarla per molte applicazioni anche molto diverse tra loro. Questo permette ai service provider avere molta più flessibilità (es: possono affittare la propria rete per poermette a terze parti di installare le proprie applicazioni).
# CONTROLLER
Il compito del controllore è quello di fornire un'interfaccia uniforme e centralizzata dell'intera rete, in modo da permettere di controllare in modo efficiente e flessibile l'instradamento degli Switch.
### Interfacce
- Sud
	È l'interfaccia verso la rete.
	- Permette al controllore di ottenere le informazioni della rete fisica, cioè le Network Information Base (NIB); le informazioni comprendono i nodi, la topologia, le porte, gli host e i collegamenti e le statistiche.
	  Lo stato del sistema, cioè queste informazioni, è poi rappresentato in un grafo.
	- Permette al controller di installare le regole sui vari Switch.
- Nord
	È un'interfaccia di tipo [REST](REST.md) che permette ai livelli superiori di interfacciarsi con la rete.
	- Le applicazioni utilizzano il grafo, il NIB, per prendere decisioni e gestire la rete.
	- Essendoci un solo grafo questo sarà standard per tutte le applicazioni.
### Componenti
I [componenti](SDN%20controller%20component.png) del controllor possono essere separati in:
- Interni
	- Gestore dei protocolli, che si occupa dei classici protocolli internet e permette l'interazione con rete legacy.
	- Applicazioni, che usano il NIB e le librerie per programmare la rete.
	- Librerie, forniscono astrazioni per usare l'interfacce sud.
- Sopra il controller ci sono i tool per l'orchestratore, l'Operational Support System (OSS) e il Business Support System (BSS).
- Sotto il controller c'è l'interfaccia per parlare con gli Switch, come OpenFlow. Questa deve essere installata anche sugli Switch, per permettere di poter comunicare.
### Deployment
Logicamente il controllore è uno solo (la vista sulla rete è unica); fisicamente questo può essere composto da più processi che girano sullo stesso server o in server diversi. Le modalità con cui si può fare il deployment del controllore sono:
- Hot-standby
	C'è un controllore master e uno slave, dove il secondo entra in gioco solo se il primo ha deti problemi. Possono girare sia su server distribuiti che sullo stesso server.
	- È una soluzione semplice.
	- Il controllore è il collo di bottiglia rispetto al numero degli Switch, perchè da solo deve gestire tutto il carico di lavoro.
- Dstributed NIB
	Avere un solo controllore porta a problemi di scalabilità e fault tollerance. Per miglioare questi problemi si aggiunge un cluster di controllori, ognuno dei quali gestisce parti diverse ma sovrapposte della rete.
	- Siccome le informazioni di rete sono ridondate, se uno si rompe nessuna parte della rete rimane scoperta.
	- Usato quando ci sono molti Switch. Permette di aumentare infatti la disponibilità del sistema.
	- C'è un overhead dovuto alla comunicazione tra controller. Questa avviene tramite interfacce dette est e ovest.
- Hybrid mode
	È una combinazione delle due sopra: ci sono più master e più slave che controllano parti diverse.
# OPEN FLOW
È il control plane standard signalling protocol, cioè il protocollo più utilizzato. Permette la coperazione tra data plain e control plain; i due si scambiano messaggi tramite l'OpenFlow Protocol.
### Switch
L'hw dello Switch non è importante, ma ognuno di essi deve contenere i seguenti [componenti](OpenFlow%20switch%20component.png):
- Porte
	Più di una porta in ingresso e in uscita. Gli unici dispositivi che hanno solo una porta sono gli host.
- OpenFlow channel.
	Il traffico che va dal controllore allo Switch è permanente e cifrato, e quindi di tipo TCP.
	Su questo canale circolano solamente pacchetti di controllo; il controllore non deve ricevere mai pacchetti dati (li scarta a prescindere).
- Flow table
	Le flow table servono a contenere le regole per il maching. Più tabelle servono per applicare regole diverse ad un pacchetto dati ricevuto dallo Switch in modo semplice e veloce (senza che ci debba essere una cascata di and).
- Tabelle di gruppo
	Servono per 
### Funzionamento
Ogni tabella ha delle regole che sono applicate a ogni pacchetto ricevuto dallo Switch. Una regola è una entry della tabella che ha le seguenti [caratteristiche](funzionamento%20openflow%20SDN.png):
- Match
	La prima cosa che lo Switch controlla è se il pacchetto ricevuto matcha con una delle regole stabilite. Essendo un match a livello di flusso si controllano gli attributi fino al livello 4 (trasporto); si controlla che il pacchetto appartenga allo stesso flusso di pacchetti già passati.
- Action
	- Se si trova una regola allora si forwarda il pacchetto alla porta corretta.
	- Altrimenti si trasforma il pacchetto da pacchetto dati a pcchetto di controllo e lo si invia al controllore (perché il controllore riceve solo pacchetti di controllo).
- Counter
	Ogni volta che una regola viene matchta da un pacchetto si incrementa un contatore, utile poi per le statistiche.
- Priority
	Ogni regola ha una priorità, per cui se più regole matchano si esegue quella a priorità maggiore; questo è utile quando più applicazioni sono in contrasto.
	Il controller però non è in grado di aiutare in questa situazione perchè ha una visione global della rete ma non delle applicazioni.
- Time out
	Il time out viene rinfrescato quando si usa la regola. In questo modo le regole più vecchie e non usate non resteranno in memoria ma saranno eliminate.
### Messaggi
##### Controller/Switch
- Send-packet
	Invia il pacchetto a una specifica porta. Viene mandato dopo che lo Switch non aveva una regola per il pacchetto in esame.
- Flow-mod
	Cambia una regola della tabella dello Switch.
- Read-state
	Il controllore legge lo stato dello Switch, cioè vuole le informazioni relative alle porte, statistiche e flussi.
- Features
	Mandato dal controllore quando uno Switch si collega per la pria volta alla rete.
##### Switch/Controller
- Packet-in
	Manda al controllor un pacchetto per cui non sono state stabilite regole.
- Flow-removed
	Mandato quando una regola scade.
- Error
	Mandato quando c'è un errore.
##### Bidirezionale
- Hello
	Mandato per iniziale la comunicazione.
- Echo
	Mandato periodicamente per controllare la latenza, la banda o il fatto che l'altro sia vivo.
# CASI D'USO
- Intenet provider
	Gli internet provider fisici, cioè quelli che possiedono effettivamente i dispositivi di rete, possono vendere una parte della rete agli operatori virtuali; questa poi può essere riprogrammata per gli scopi del cliente in modo che questo possa offrire lo stesso servizio a condizioni diverse.
- Parental control
	Il parental control può essere sviluppato direttamente sul dispositivo, in modo che non faccia passare certi tipi di pacchetto. Essendo il forwarding molto più veloce di un'analisi sw questa soluzione gode di prestazioni più elevate.
- Data center
	- Non usando dispositivi statici si può migliorare il carico di lavoro di ogni dispositivo di transito.
	- Gli Swtich SDN permettono una migrazione dei server molto più veloce grazie alla riprogrammabilità dei dispoitivi.