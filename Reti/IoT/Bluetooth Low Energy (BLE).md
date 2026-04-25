BLE è stato introdotto dalla quarta versione [Bluetooth](IEEE%20802.15.1%20-%20Bluetooth.md) (studia solo inizio e topologia) nel 2010 per scenari di IoT ed è retro compatibile con tutte le versioni precedenti.
L'idea di BLE è stata quella di ottenere una comunicazione a bassissima potenza, con un alto bit rate e un idle time massimizzato.
# ARCHITETTURA
Solitamente i livelli controller si riferiscono al chipset (la scheda BLE), mentre i livelli host stnno sulla CPU principale.
![Architettura BLE](Architettura%20BLE.png)
### Physical layer
BLE usa la frequenza 2.4GHz ISM (non commerciale e senza licenza per Industrial, Scientidica Medical) con 40 canali: 3 di questi (Primary Advertising Channels) sono dedicati alla discovery phase, mentre gli altri 37 (Secondary Advertising Channels) sono destinati al trasferimento di dati. Il data rete del physical layer è di 1Mbps.
Per selezionare il canale di comunicazione si utilizza AFH (Adaptive FHSS), che è affidabile e robusto rispetto alle interferenze. Quando due dispositivi si connettono scambiano una sequenza di canali; ogni istante di tempo i dispositivi trasmettono su un canale e poi passano a quello successivo.
### Link layer
Tramite HCI, un'interfaccia tra i livelli controller e i livelli host, mettere in relazione i livelli superiori con i livelli inferiori, in modo che questi possano parlarsi anche se di tipi diversi.
I compiti principali sono:
- Stabilire la connessione, l'advertising e lo scanning.
- Definisce il formato dei pacchetti di advertising e dati.
- Ritrasmissione dei pacchetti.
- Security.
##### Advertising
I pacchetti di advertising sono inviati a un intervallo fisso da 20ms a 10,24s con un incrementi di 625μs.
I canali utilizzati per questo scopo sono solamente 3, contro i 32 di Bluethoot; questo permette ai dispositivi BLE nello stato di advertising di consumare dalle 10 alle 20 volte meno rispetto a Bluetooth.
I pacchetti inviati sono ricevuti dai dispositivi nello stato scanner che possono inviare un messaggio per accettare la connessione; il dispositivo in fase advertising può comunque non connettersi, in modo che possa essere trovato da più dispositivi in fase scanning; in questo modo però si perde la possibilità di avere una comunicazione bidirezionale e si perde in sicurezza.
##### Scanning
Il dispositivo in fase scanning ascolta in modo round-robin (uno alla volta in ciclo) i 3 canali dove circolano i pacchetti di advertising. L'obiettivo è quello di rilevare dispositivi che si vogliono connettere. Ci sono due modalità di scanning:
- Attiva
	Dopo aver ricevuto il pacchetto di advertising, il dispositivo invia un messaggio di $scan\_request$; il ricevente invierà uno $scan\_response$ per comunicare altri dati non inclusi nel pacchetto di advertising originale.
- Passiva
	Il dispositivi si limita ad ascoltare sui 3 canali di advertising, e non può inviare alcuna richiesta ulteriore al dispositivo che sta inviando i pacchetti di advertising. Permette di risparmiare energia.
##### Connessione
Dopo la fase di ricerca, il dispositivo in fase inititing e quello in fase advertising possono stabilire una connessione sullo stesso canale fisico. Quando l'initiator riceve il pacchetto di advertising, invia all'advertiser un pacchetto di $connect\_req$ che contiene:
- Il pattern di frequency hopping, cioè i tempi di salto e i canali in cui saltare (all'interno dei 37 disponibili).
- L'intervallo tra due connessioni consecutive in cui i dispositivi scambiano i dati. Questo può essere tra 7,5ms e 4s.
- La latenza con cui lo slave può collegarsi, cioè gli eventi di connessione che può saltare al fine di risparmiare energia.
- Il tempo massimo che si aspetta tra due pacchetti consecutivi prima che la connessione sia considerata persa.
### Logical Link Control and Adaptation Protocol (L2CAP)
È un livello E2E, che si può dire adatta i livelli superiori ai livelli inferiori tramite la framentazione. I pacchetti prodotti dai livelli superiori sono infatti frammentati in pacchetti supportati dal livello radio di BLE; in ricezione deve poi assemblare nuovamente tali pacchetti.
### Attribute Protocol (ATT)
Definisce il protocollo per lo scambio di dati tra server e client basato su attributi: il primo li possiede e li espone, il secondo li richiede.
Oltre all'ovvia modalità di request e response, ce n'è una che permette l'invio di dati non richiesti: notification, dove il client non deve mandare messaggi di conferma; indications, dove è necessaria la conferma di ricezione da parte del client.
##### Attributi
In BLE lo stato di un device è definito da attributi; ATT definisce come questi attributi sono acceduti. Un attributo è definito da:
- Handle
	È un'identificatore che permette di accede all'attributo.
- Type
	Permette di identificare in modo univoco il tipo dell'attributo tramite un UUID definito dal Bluetooth SIG.
- Value
	È il valore effettivo dei dati.
- Permission
	Definisce i permessi di lettura e scrittura dell'attributo.
### Security manager
Genera e gestisce le chiavi per la cifratura e l'idenità.
### Generic Attribute Profile (GATT)
GATT definisce come i profili sono organizzati per costruire applicazioni complesse. I profili possono essere standard in modo che i fornitori di dispositivi e sw possano creare applicazioni interoperabili.
Un profilo è un caso d'uso di alto livello, cioè definisce ciò che un particolare dispositivo può fare. Un profilo è formato da strutture gerarchiche:
- Servizio
	Sono raccolte di dati (caratteristiche) utilizzati per svolgere funzioni specifiche del dispositivo.
	Sono funzionalità specifiche e sono formate da più caratteristiche.
- Caratteristica
	Rappresenta un singolo dato all’interno di un servizio.
- Descrizione
	Sono dettagli della caratteristica, cioè dei dati; ad esempio formato, scala o se il server può notificare automaticamente al client in caso di modifiche.
##### Esempio
- Il profilo è il "Heart Rate Service".
- All'interno del profilo, ci sono diversi servizi, come "Heart Rate Measurement" e "Body Sensor Location".
- "Heart Rate Measurement" ha un valore aggiornato in tempo reale con il battito cardiacoe un descrittore che indica il formato del dato (es; bpm).
### Generic Access Profile (GAP)
Descrive se e come due dispositivi possono comunicare tra loro, cioè le modalità di scoperta, connessione e comunicazione.
Inoltre definisce i ruoli dei dispositivi, cioè se possono solo trasmettere, solo ricevere o entrambi.
# TOPOLOGIA
La topologia della rete BLE è una topologia mesh (ogni dispositivo può parlare con l'altro), auto configurabile in modo dinamico. In questa configurazione ci possono essere anche nodi master e nodi slave come nel classico [Bluetooth](IEEE%20802.15.1%20-%20Bluetooth.md).
Le reti mesh funzionano tramite inoltro: ci sono dispositivi che possono ricevere dati e inoltrarli ad altri dispositivi; un vantaggio di questo è l'estensione della rete e la portata che un singolo dispositivo può avere.
### Nodi
Ci sono nodi di vario tipo:
- Relay
	Possono ritrasmettere il messaggio ad altri nodi. Sono i dispositivi che permettono di estendere la rete oltre la portata del singolo dispositivo.
- Proxy
	Permettono la comunicazione con la rete a dispositivi che non supportano la modalità mesh.
- Friend/low powers
	Sono strettamente collegati ai nodi low power: durante il tempo in cui quest'ultimi sono idle (inattivi) i nodi friend memorizzano nella cache i messaggi; una volte che il nodo low power si riattiva gli inviano i dati.
# RUOLI
I dispositivi si possono differenziare in vari ruoli:
- Advertiser (Broadcaster)
	È il nodo che può inviare in broadcast, ma non ricevere, pacchetti di advertising. Inviando pacchetti di advertising sta comunicando con la rete la sua presenta pur non essendo attivo.
	Ad esempio sono i dispositivi che comunicano alla rete un dato ogni istante di tempo.
- Scanner
	Un nodo che ascolta se ci sono pacchetti di advertising. Può permettere il collegamento di un advertiser. Ad esempio sono i dispositivi che ricevono dati dalla rete per viasulizzarli.
- Peripheral
	Un nodo che è collegato solo al nodo centrale o a un isieme di nodi centrali (dal BLE 4.1). È solitamente il dispositivo che invia i pacchetti di advertising.
- Central
	È un nodo collegato a uno o più peripheral. È solitamente il dispositivo che riceve i pacchetti di advertising. È inoltre il dispositivo che definisce l'intervallo e la sequenza di canali tra due nodi.
	Teoricamente può essere collegato a infiniti nodi, ma nella pratica si parla di 4/20 nodi al massimo.
- Hybrid
	È un nodo che include più di una delle funzionalità superiori.
# STATO
I dispositivi possono trovarsi in vari stati:
- Standby/Idle
	Sono i nodi inattivi. I nodi tenderanno ad andare in questo stato per risparmiare energia.
- Advertising
	I dispositivi in questo stato sono chiamati advertiser. Stanno inviando pacchetti di advertising per comunicare dati o trasmettere la propria posizione, in modo che i dispositivi in stato scanning o initiating possano riceverli.
- Scanning
	Sono i nodi che sono in ascolto di pacchetti di advertising.
- Initiating
	È il nodo che ha deciso di accettare la connessione con un nodo in stato advertising.
- Connection/Active
	È un nodo che ha appena stabilito la connesione con un altro. Il nodo che era in stato initiating diventa il master, quello che era in advertising diventa lo slave.
# PACCHETTI
In generale i pacchetti di advertising e dei dati sono molto simili.
- Preambolo (1 byte)
	È un byte usate dal ricevente per stabilire la sincronizzazione. Per sincronizzazione si intende far partire la comunicazione nello stesso momento, cioè aver settare il tempo zero nello stesso istante.
	Questa sincronizzazione è una seguenza di 8 bit scelta randomicamente all'interno di una lista.
- Access Address (4 byte)
	- Se siamo nel caso di un pacchetto di advertising contiene un indirizzo costante.
	- Se siamo nel caso di un pacchetto dati allora contiene un indirizzo casuale che identifica la connessione corrente.
- PDU
	Contiene i dati nel caso di un pacchetto dati o le informazioni per la connessione nel caso di un pacchetto di advertising.
- CRC (24 bits)
	Indica il metodo per calcolare il checksum, che serve poi per l'integrità dei dati.
##### Advertising
Nel caso di un pacchetto di advertisng, oltre a quando descritto, dopo l'access address si specifica:
- Header (2 byte)
	Contiene: le informazioni sul tipo di indirizzo MAC, cioè se è randomico o pubblico, e le informazioni sulla lunghezza dell'AdvData.
- Advertising Address (AsvA) (6byte)
	Contiene l'indirizzo MAC del dispositivo che ha generato il pacchetto di advertising. Può essere di due tipi:
	- Pubblico, allora si trova all'interno del device.
	- Randomico, allora è generato dinamicamente durante la connessione. In questo caso può essere o scelto all'interno di una lista programmata all'interno del dispositivo, oppure essere generato da zero (più sicuro).
- Advertisign data (AdvData) (max 31 byte)
	Contiene i dati necessari per stabilire la connesione.
# CASI D'USO
### Proximy solution
Sono applicazioni base sui punti di interessi (PoI) (es: museo che fornisce le informazioni sulle opere presenti nella stanza), oppure la ricerca di oggetti che si dovrebbero trovare vicino (es: airtag).
In tutte queste situazioni i tag BLE, detti Beacon, trasmettono periodicamente le proprie informazioni (fase di advertising e quindi in modalità broadcast); il dispositivo che riceve (fase di scanning) sta quindi ricevendo le informazioni necessarie per identificare univocamente il beacon (es: ID) e non ha bisongo di instaurare una connessione; trasmette i risultati al server dell'applicazione la quale erogherà un servizio sulla base dell'informazione ricevuta (es: un quadro comunica il proprio id e il server dice che quadro è).
### Real Time Locating System (RTLS)
Sono quesi servizi basati sulla posizione specifica del dispositivo; ad esempio si vuole individuare un dispositivo per monitorarlo in tempo reale.
##### Trilaterazione
È un metodo che si basa sulla potenza del segnale ricevuto (RSSI).
Se avessi un solo Beacon tramite l'RSSI dal dispositivo si riesce a calcolare la vicinanza con una precisione del metro; si riesce quindi a tracciare una circonferenza sulla quale il dispositivo si trova, ma non una posizione precisa. Tramite l'utilizzo di più Beacon (due, ma per una buona precisione ne servono almeno tre) posso invece [tracciare più circonferenze](Trilaterazione%20BLE.png); tramite la loro intersezione delimiterò un'area entro la quale il dispositivo di trova (motivo per cui la precisione è di 2-3 metri).
- Il problema principale, che determina anche che la precisione sia entro 2/3 metri, è che il segnale RSSI può essere disturbato dalle condizioni ambientali (pareti o muri) oppure da altri segnali.
##### Triangolazione
Funziona sull'angolo di ricezione del segnale.
Devono essere noti più elementi: la distanza nota tra due o più beacon; per ogni beacon l'[angolo](Triangolazione%20BLE.png) formato dalla congiungente con un altro beacon e con il dispositivo (per fare questo ci servono delle antenne particoalri).
In questo modo si possono calcolare gli angoli che il dispositivo forma con due beacon e determinare poi la posizione nello spazio.
- Per aumentare la precisione ci servono più Beacon e si devono usare tecniche complesse.