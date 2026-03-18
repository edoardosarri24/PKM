In una rete di sensori si possono usare mezzi fisici sia cablati che radio; essendo i secondi più diffusi si parla di WSN (Wireless Sensor Network). Una WSN implementa il protocollo [IEEE 802.15.4](IEEE%20802.15.4.md).
Si tratta di una rete "ad hoc", dove i dispositivi possono parlare direttamente tra loro e che solitamente non è direttamente aperta versa l'esterno. È composta da:
- Sensori
	Eseguono le misurazioni delle grandezze fisiche e le trasmettono al sink.
- Sink
	Raccolgono i dati rilevati dai sensori ed elaborano le informazioni acquisite.
- Attuatori
	Sono dispositivi che eseguono comandi sulla base dei dati racconti ed alaborati.
### Caratteristiche
- Ha un numero molto elevato di sensori, solitamente posti anche vicini tra loro.
- È in grado di configurarsi automaticamente, ma solitamente ha una topologia statica.
- Il traffico della rete è asimettrico e va dai sensori, che spesso svolgono anche la funzione di ripetitore, al link.
- La comunicazione avviene su base broadcast, in quanto avere un'antenna precisa aumenterebbe il costo dei dispositivi.
### Trasmissione
- Address based
	Ogni elemento della rete ha un proprio ID e può essere identificato in modo univoco. È utile se serve attivare solo un determinato dispositivo.
- Data centric
	Tutti gli elementi della rete sono associati ad un unico ID.
### Applicazioni
- Militari
	Sono nate per fungere da supporto: si riesce ad accedere a delle informazioni remote con una prontezza tale da massimizzare la loro rilevanza.
- Ambientale
	Controllo dell’inquinamento, del movimento degli animali, delle maree, delle condizioni meteorologiche e degli incendi.
- Medicina
	Monitoraggio di parametri clinici da remoto e delle situazioni potenzialmente pericolose per i pazienti (Come le cadute).
- Vari
	Allarmi, musei interattivi, giocattoli interattivi, smart home.
### Differenze con WiFi
Una WSN ha delle differenze sostanziali con una rete [WiFi](IEEE%20802.11%20-%20WiFi.md):
- Il numero dei dispositivi in una rete WSN è di solito è molto maggiore.
- I sensori sono posti in modo molto denso e di solito non sono mobili.
- I sensori sono limitati rispetto all’uso dell’energia, cosa che non avviene con i dispositivi normali.
## SENSORI
Un sensore è caratterizzato da un prezzo e una dimensione contenuta. Sono formati da:
- Processore
	Gestisce le funzioni di sensing e di comunicazione. Solitamente è a basso consumo energetico e quindi non ha una grande potenza di calcolo.
- Trasmettitore/Ricevitore
	È l’interfaccia radio del sensore e si occupa delle comunicazioni con l’esterno.
- Batteria
	È la parte critica di un sensore. Di solito i sensori sono posti in luoghi non molto accessibili e quindi deve essere il più efficiente possibile.
- Sensori
	Sono dal parte del dispositivo che effettivamente catturano i dati dall’ambiente e li trasformano in un formato che può essere elaborato o trasmesso a distanza.
### Attivazione
- Push
	Il sensore si attiva e comunica in istanti precisi di tempo. Potrebbe rilevare dati non sempre necessari, ma non necessità di un controllo esterno ed è quindi più autonomo.
- Pull
	Il sensore si attiva e comunica solo dopo una sollecitazione esterna ad essi. Rileva solo i dati strettamente necessari, ma non è completamente autonomo.
## POTENZA
Uno dei motivi per cui una WSN implementa il protocollo [IEEE 802.15.4](IEEE%20802.15.4.md) è la durata della batteria: le fase di riposo, utilizzo della CPU e sensing hanno un consumo di energia trascurabile; le fasi di trasferimento delle informazioni e ricezione dei comandi sono invece le più dispendiose e sono quelle che dovranno essere ottimizzate.
Per risolvere questo problema:
- Conoscendo la distanza con il dispositivo con cui si vuole comunicare si potrebbe adeguare la potenza usata (Una maggiore distanza necessita di una maggiore potenza); il problema è che questo implica conoscere la posizione di ogni dispositivo del sistema.
- Sappiamo che con la tecnica [CSMA](Tecniche%20di%20accesso%20MAC.md#CSMA)esiste la fase preliminare di handshaking, necessaria ad evitare le collisioni con i nodi che non riescono ad accorgersi dell’occupazione del canale ([terminale nascosto](Tecniche%20di%20accesso%20MAC.md#CSMA/CA#Terminale%20nascosto). Quando si invia il messaggio RTS lo si fa con la massima potenza, così come accade per il successivo messaggio di ritorno CTR; a questo punto si può, attraverso il livello di potenza con cui questo viene ricevuto, dosare la potenza necessaria per raggiungere il dispositivo con il quale si è instaurata la connessione.
- Un altro aspetto sul quale possiamo soffermarci è detto In network processing: i sensori spesso sono relativamente vicini e quindi inviano dati praticamente equivalenti; non è efficiente condividere tutti i dati acquisiti, ma è meglio unirli e poi inviarli in un unico messaggio. Il dispositivo incaricato di raccogliere i dati e di inviali al sink è detto clusterhead; questo dispotivo implementa le metodologie per ridurre la ridondanza di dati:
	- Data aggregation. Sono dati derivati direttamente dall'insieme; un esempio sono le informazioni statistiche come la media.
	- Data fusion. Si elaborano i dati raccolti in modo da dedurre informazioni non direttamente disponibili.