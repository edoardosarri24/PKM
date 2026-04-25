Le LR-WPAN (Low Rate Wireless Personal Area Network) sono caratterizzate da un basso bit rate (al più 250Kbit/s), un consumo energetico basso (batteria che può durare 10 anni), una bassa complessità e costo e una bassa copertura spaziale (al più 30m).
![stack LR-WPAN](stack%20LR-WPAN.png)
# 802.15.4
Lo standard definisce il livello fisico e MAC.
##### Dispositivi
Ci sono due tipoi di dispositivi:
- Full Function Devide (FFD)
	Può operare come dispositivo, coordinatore o coordinatore PAN (che deve essere almeno uno all'interno della rete).
	Sono i dispositivi che permettono ogni configurazione della rete (sia star che mesh che combinata), perché può parlare con ogni altro dispositivo.
	I vantaggi di una rete mesh sono: estensione del range, permettere il multi-hopping e l'automatic route discovery.
- Reduce Function Device (RFD)
	Può operare solo come dispositivo.
	Permettono solo la configurazione star perché parla solo con il coordinatore.
##### Addressing
I dispositivi possono avere due tipi di indirizzi: a 16 bit o a 64.
- Solitamente gli indirizzi a 16 bit sono usati all'interno della rete, cioè i dispositivi (FFD e RFD) si parlano con gli indirizzi a 16 bit.
- L'indirizzo a 64 bit è usato solo dal coordinatore PAN (che funge da gateway: raccoglie i dati e li comunica ad un qualche server) per parlare con l'esterno, visto che è ottenuto tramite EUI-64 e quindi dovrebbe essere univoco a livello globale.
##### Trasferimento dati
Osserviamo che in una topologia a stella solo le prime due possono esere usate, perché i dispositivi possono parlare solo col coordinatore.
- Device to coordinator
	Il device manda il dato e il coordinator manda l'ACK.
	Se siamo nella modalità beacon allora prima il coordinator manda il beacon.
- Coordinator to device
	Il device da una request; il coordinato manda l'ACK e poi i dati; il device manda l'ACK.
	Se siamo nella modalità beacon allora prima il coordinator manda il beacon.
- device to device
### Fisico
Le sue funzionalità sono:
- Attivazione e disattivazione della trasmittente in base allo stato in cui il dispositivo si trova.
- Indicazione della qualità del canale e quindi dei pacchetti ricevuti. 
	Si valuta tramite la potenza del segnale ricevuta o una stima del rapporto informazione/rumore. Più si ha un valore alto più chi sta trasmettendo è vicino.
- Valutazione della libertà del canale tramite CSMA-CA.
##### Direct Sequence Spread Spectrum (DSSS)
È la tecnica di comunicazione utilizzata. Permette al segnale trasmesso di occupare più banda di quella che servirebbe normalmente per trasmettere l'informazione.
Il vantaggio è che usare una banda larga permette una trasmissione più affidabile e permette accessi multipli.
Ci sono due fisici che utilizzano questa tecnica: usare una frequenza di 868MHz, che implica avere 1 canale con un data rate di 20Kb/s; usare 16 canali con una frequenza di 2,4GHz, che implica avere 16 canali con un data rate di 250Kb/s.
##### Pacchetti
![phy packet 802.15.4](phy%20packet%20802.15.4.png)
I pacchetti trasmessi sono chiamati PHY Protocol Data Unit (PPDU).
- Preambolo (4 byte)
	Serve per la sincronizzazione.
- Start of packet delimiter (1 byte)
- Frame lengt (7 bit)
	Indica la lunghezza del payload in byte.
- Reserve (1 bit)
- Payload
### MAC
##### CSMA-CA
Il meccanismo che permette a più dispositivi di accedere al canale è il Carrier Sense Multiple Access with Collision Avoidance (CSMA-CA).
Prima di trasmettere un dispositivo valuta il canale (CCA) per capire se è libero oppure no: se è libero per due ascolti consecutivi allora trasmette, altrimenti aspetta un lasso di tempo, detto backoff e incrementato esponenzialmente a ogni tentativo, e poi riprova.
Siccome ascoltare il canale consuma energia, per avere un basso consumo energetico un dispositivo può fare al più 3 tentativi per ogni beacon frame ricevuto.
##### Accesso al canale
Il meccanismo di accesso al canale utilizza unità di tempo dette backoff. Ci sono due meccanismi di accesso:
- [Beacon](beacon%20MAC%20802.15.4.png)
	È un meccnismo usato quando si ha bisogno bassa latenza. Il tempo è diviso in superframe.
	Il superframe è diviso in active, dove i dispositivi possono comunicare, e in inactive, dove i dispositivi si dovrebbero trovare in sleep mode.
	All'inizio di ogni superframe il PAN coordinator invia il beacon, cioè un pacchetto che detta la sincronizzazione (setta il clock di ogni device), che stabilisce la lunghezza della parte activate e inactive.
	La prima fase della fase active è una fase a contesa (dove si usa CSMA-CA) ed è detta CAP.
	La fase successiva è detta CFP: se nella precedente CAP il device ha ancora dati da trasmettere allora può richiedere al PAN coordinator di allocargli in modo riservato uno o più slot. Se la richiesta è andata a buon fine il device lo scopre nel beacon del prossimo superframe.
- Beaconless
	È un meccanismo totalmente a contesa. Si usa quando la latenza non è un problema.
##### Pacchetti
I pacchetti trasmessi sono chiamati MAC Protocol Data Unit (MPDU) e sono contenuti nella parte Payload del pacchetto di livello fisico (a causa dell'incapsulamento). Ci sono 4 tipi di pacchetti:
- [Beacon frame](beacon%20frame%20MAC%20802.15.4.png)
	- Frame control
		Contiene le informazioni sul tipo di pacchetto, se gli indirizzi sono a 16 o 64 bit e altri controlli relativi alla sicurezza.
	- Sequence number
		Indica il numero progressivo del pacchetto.
	- Address
		Campo che contiene gli indirizzi del mittente e del destinatario.
	- MSDU
		Contiene le informazioni di trasmissione, visto che siamo in un beacon frame.
	- MFR
		È un checksum e viene usato per controllare gli errori di trasmissione. È generato a partire da tutti campi precedenti e il destinatario può controllare se ci sono errori rigenerandolo.
- [Data frame](data%20frame%20MAC%20802.15.4.png)
	Stessa cosa del pacchetto sopra ma con un payload al posto delle informazioni di trasmissione.
- [ACK frame](ack%20frame%20MAC%20802.15.4.png)
	Sequence number contiene il pacchetto a cui l'ACK si riferisce.
- [Command frame](command%20frame%20MAC%20802.15.4.png)
	Serve per implementare il controllo e la configurazione di tutti i nodi della rete.
	- Command type
		È l'identificatore univoco del comando.
	- Payload
		Contiene tutti i parametri richiesti.
# ZigBee Alliance
È un insieme di 45 aziende che collaborano per specificare i layer sopra il livello MAC (dettato da 802.15.4) per le LR-WPAN.
Non hanno uno standard brevettato; questo vuol dire che si può copiare le loro idee e realizzare qualcosa di simile.
### Stack
##### Network
- Gestisce la creazione di una nuova rete, con il relativo assegnamento degli indirizzi.
	I primi dispositivi che si collegano alla rete saranno collegati al nodo che crea la rete, che sarà anche il PAN coordinator; via via che arrivano nodi questi saranno a più hop dal nodo root.
	Successivamente il nodo root può essere cambiato ed eletto tramite una votazione basata sulla valutazione degli RSSI.
- Gestisce la sincronizzazione tra device e PAN coordinator.
- Stabilisce le regole di routing.
	La rete può avere varie topologie: nella rete a stella un dispositivo può parlare solo con il coordinator; nella rete mesh, tra due nodi sono disponibili più percorsi (che sono scelti dinamicamente); nella rete mesh tree (la radice è il nodo più intelligente ed è il PAN coordinator) via via si diramano figli che hanno sempre meno capacità e i nodi intermedi devono fare da tramite dai nodi foglia all’edge router (in questa topologia si chiama così)) tra due dispositivi c'è un solo percorso.
- Gestisce l'entrata e l'uscita dalla rete di un dispositivo.
	Quando un nodo che ancora non ha l'accesso alla rete si connette, lo fa connettondosi al nodo da cui riceve più segnale: manda un Beacon e guarda la potenza RSSI ricevuta e sulla basa di questa potenza sceglie il nodo a cui legarsi.
- Scoperta dei dispositivi one-hop, cioè dei vicini.
##### Security
È trasversale rispetto a ZDO e APS.
##### ZigBee Device Objects (ZDO)
- Definisce il ruolo di un dispositivo nella rete.
- Scopre i dispositivi nella rete e determinano quali servizi questi forniscono.
- Gestisce (l'avvio) della richiesta di associazione alla rete di un dispositivo.
##### Application Support (APS)
- Memorizza le tabelle di binding.
- Filtra i messaggi duplicati che potrebbero arrivare dal layer sottostante.
##### Application
- Esegue il binding, che è l'abbinamento di due dispositivi in base alle loro esigenze di comunicazione e ai servizi che forniscono e di cui hanno bisogno.
### Profili
I profili (es: home automation, smart energy) sono degli agreement (accordi) sul formato dei messaggi e sulle tecniche di elaborazione che permettono alle applicazioni di usare dispositivi di produttore diversi; l'obiettivo è quindi aumentare l'interoperabilità dell'applicazione.
Oltre ai profili definiti da ZigBee i produttore possono creare il loro profilo personalizzato; questo poi può essere reso pubblico e usato da altri produttori dopo che la ZigBee Alliance lo ha revisionato.