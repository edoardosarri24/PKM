LoWPANs (Low-power Wireless Personal Area Networks) sono reti wireless progettate per dispositivi con bassa potenza e capacità limitate.
### Caratteristiche
- Data rate
	Lo standard IEEE [802.15.4](LRWPAN.md#802.15.4) usa pacchetti con una dimensione di soli 81 byte. Il data rate è nell'ordine delle centinaia di kbps.
- Bassa consumo energetico
	Il trasferimento di dati è l'operazione che consuma più energia nei dispositivi che appartengono a questi tipi di rete. Per risparmiare energia i dispositivi possono interrompere le comunicazioni entrando in sleep mode.
- Poche risorse
	I dispositivi che appartengono a questo tipo di reti hanno poche risorse: poca potenza computazione e poca memoria. Sono quindi economici.
- Inaffidabilità
	La connettività radio è incerta, ci può essere un esaurimento della batteria, i dispositivi si possono bloccare e ci possono essere delle manomissioni fisiche.
- Locazione
	Spesso si utilizzano sensori in zone poco raggiungibili per non dover andarci di persona.
### IPv6
Introdurre IPv6 nel mondo IoT ha delle conseguenze.
##### Vantaggi
- L'infrastruttura è già esistente (e si conosce molto bene) e quindi si può gestire i device tramite internet. Inoltre si possono collegare i dispositivi a reti già esistenti e che usano il protocollo TCP/IP.
- Si offrono molti indirizzi globali e quindi si possono collegare molti dispositivi.
##### Svantaggi
- I pacchetti IPv6 non sono compatibili con IEEE 802.11.4: il primo ha un MTU di 1280 byte, mentre il secondo di 127 byte; inoltre solo l'header IPv6 (40 byte) userebbe 1/3 del pacchetto.
- Gli algoritmi di sicurezza di IPv6 (IPsec) sono troppo complessi per dispositivi constrained. Devono quindi essere implementati algoritmi specifici per queste reti.
- IPv6 permette la connessione multicast (da uno a un gruppo), mentre la rete mesh (solitamente) non lo permette ed è un tipo di comunicazione difficile da implementare.
- La rete mesh con i device contrained impone che gli algoritmi di routing siano semplici.
# 6LoWPAN
IPv6 over Low Power Wireless Personal Area Networks è uno standard definito dalla IETF per risolvere i problemi precedenti all'interno delle LoWPAN (risiede quindi sopra il livello fisico e MAC di [802.15.4](LRWPAN.md#802.15.4)): si vogliono creare reti di dispositivi che usano il protocollo IP (IPv6) per comunicare in modo nativo, cioè senza passare da un gateway.
Per fare questo si introduce un layer all'interno della pila protocollare di TCP/IP detto adaptation layer. Gli obiettivi dell'adaptation layer sono:
- Frammentazione
- Compressione dell'header IPv6 e UDP, in modo da recupare byte per il payload.
- Supporto al routng di livello MAC. È necessario infatti un algoritmo che permetta il multi hop disponibile nelle reti mesh.
### Architettura
Connette delle [stub network](architettura%206LoWPAN.png) (chiamate isole). Una stub network è una sotto rete che si trova ai margini della rete e che si interfaccia verso l'esterno tramite un solo punto; il dispositivo di connessione è detto Edge Router (è un FFD).
I dispositivi sono: host, che sono RFD; router, che sono FFD che non comunicano con l'esterno della rete; edge router, che sono FFD e comunicano con l'esterno della rete (sarebbero i PAN coordinatore delle [LRWPAN](LRWPAN.md)).
Ci sono tre tipi di reti: simple LoWPAN, dove c'è un singolo edge router; exteded LoWPAN, dove ci sono più edge router, i quali sono collegati da un canale detto backbone link; ad-hok LoWPAN, che è una rete isola che si gestisce in autonomia e non comunica con l'esterno.
### Addressing
L'indirizzo IPv6 è ottenuto automaticamente dall'interface ID e dal MAC addres. Possiamo però avere due modalità:
- Se i dispositivi all'interno della rete usano l'indirizzo a 64 bit, ottenuto dal proprio indirizzo MAC (48 bit) con l'EUI-64, allora non abbiamo problemi: possiamo concatenarlo all'interface ID e ottenere l'indirizzo a 128 bit.
- Se i dispositivi all'interno della rete usano l'indirizzo a 16 bit allora si deve arrivare a 48 bit e poi usare l'EUI-64; per arrivare a 48 bit si concatenano i 16 bit con 16 zeri e l'indirizzo che il PAN coordinator (16 bit anche quello) esponde all'interno della rete.
### Routing
Ci sono due tecniche di routing. Queste sono usate per permettere il multihopping tramite IPv6.
- Mesh-under
	Si usa un routing di livello 2, cioè a livello MAC di 802.15.4.
	Ogni dispositivo è individuato dall'indirizzo a 16 bit, che è univo all'interno della rete.
	La frammentazione viene fatta dal livello MAC della sorgente e il pacchetto viene riassemblato alla destinazione dallo stesso livello; i nodi intermedi non devo riassemblare, perché sarà usato l'indirizzo a 16 bit e non si coinvolge il livello di rete. Se un frammento viene perso allora si deve rimandare l'intero pacchetto a partire dalla sorgente.
	È utile per reti piccole senza molti dispositivi.
- Route-over
	Si usa un routing di livello 3, coinvolgendo quindi i livello rete 6LoWPAN.
	Ogni dispositivo è individuato dall'indirizzo IPv6, che quindi è unico a livello globale.
	La frammentazione avvieni in ogni nodo: nei nodi intermedi il pacchetto deve essere riassemblato e frammentato nuovamente perché si usa l'indirizzo IPv6. Se un frammento viene perso si deve inviare tutto il pacchetto dal nodo precendete.
	È utile per scalare nel numero di dispositivi.
### Pacchetti
Prima del payload possiamo avere uno stack di header che descrivono le caratteristiche del pacchetto trasmesso. Quando ci sono tutti gli header lo stack è ordinato come: header di supporto per la comunicazione mesh; header per la comunicazione broadcast; header per la gestione dei frammenti.
Siccome questi header sono opzionali e hanno una lunghezza variabile per risolvere l'ambiguità e riconoscerli si usa il meccanismo del dispatch, un byte posto prima dell'header in questione che descrive il tipo e ne consente l'interpretazione.
- Dispatch
	I primi due bit (o al più 5) di ogni header identificano che tipo di header seguirà dopo il dispatch (mesh, broadcast, frammentazione). I restanti 6 specificano dettagli aggiuntivi.
- Mesh header
	- I primi due bit sono 10.
	- Il terzo e quarto bit indicano se gli indirizzi del mittente e del destinatario sono a 16 o 64 bit.
	- Altri 4 bit per indicare gli hop possibili che rimangono.
	- Finisce con gli indirizzi della sorgente e della destinazione. Questi sono fondamentali perché durante la frammentazione nell'header Ipv6 compaiono solo gli indirizzi dei due nodi che si scambiano il pacchetto (come giustamente vuole IPv6), ma non quelli della sorgente e destinazione.
- Broadcast header
	- Primi due bit sono 01.
	- 6 bit fissi.
	- Campo sequence number settato dall'origine che server per riconoscere i pacchetti duplicati.
- Frammentazione
	- Nel caso del primo frammento i primi 5 bit sono 11000; nel caso di un frammento successivo sono 11100.
	- Datagram size, di 11 bit, specifica la dimensione del pacchetto non frammentato e senza gli header opzionale 6LoWPAN. Serve a destinazione per allocare la memoria per tutti i frame del pacchetto.
	- Datagram tag serve per identificare i frammenti dello stesso pacchetto.
	- Datagram offset, che non compare nel primo pacchetto, serve per ricomporre il pacchetto.
### Compressione
Siccome i pacchetti in 802.15.4 dovranno essere molto più piccoli rispetto a IPv6, allora ci dovranno essere molte frammentazioni. Per ridurle al minimo si vuole aumentare la dimensione del payload rispetto a quella dell'header. Per fare questo si vuole ridurre sia l'header IPv6 che l'Header UDP.
##### IPv6
Il meccanismo di compressione dell'header IPv6 è detto HC1 ed è possibile se i dispositvi fanno già parte della rete, cioè se possiedono un'interface ID.
Per interpretare l'header compresso si usa un byte iniziale seguito dai campi dell'header IPv6 che non sono stati eliminiati (come ad esempio hop limit, che non sarà mai toccato).
I campi che possono essere eliminati sono:
- Version, perché ovviamente è la versione 6.
- Traffic class, perché è sempre inizializzato a 0.
- Flow label, perché è sempre inizializzato a 0.
- Payload lenght, perché è ricavabile dal frame lenght del pacchetto PPDU (livello fisico di 802.15.4).
##### UDP
Il meccanismo di compressione dell'header UDP è detto HC_UDP e permette di risparmiare 4 byte tramite la compressione dei campi source e destination port e lenght. Il campo checksum deve essere inserito in modo non compresso.
Per l'interpretazione dell'header UDP compresso si usa un byte (HC2) dopo HC1.
### Creazione
Per far si che una rete 6LoWPAN possa funzionare si deve seguire una serie di processi:
- Commissioning
	È la configurazione dei livelli fisico e collegamento (L2 e L3) dei dispositivi in modo che siano questi siano compatibili e possano parlare tra loro.
	Questo può essere fatto o nel momento della fabbricazione del dispositivo oppure durante l'installazione (tramite USB).
- Bootstrapping
	Si esegue come prima cosa il neaightbor discovery, cioè si interrogano i nodi per ottenere informazioni sulla rete.
	Si genera l'indirizzo dei nodi: l'edge router invia nella rete il router advertisement. Una volta ricevuto, i nodi possono generare il proprio indirizzo IPv6; siccome questo è generato dall'indirizzo a 16 bit è possibile che ci siano indirizzi duplicati. Si fa quindi un Duplicate Address Detection (DAD); questa operazione viene fatta dall'edge router in modo centralizzato tramite una lista di indirizzi detta whiteboard, ed è la fase detta registration.
# RPL
RPL è il protocollo di routing standard all'interno delle reti LoWPAN. È stato ideato dal Routing over Low Power and Lossy network workin group (ROLL) di IETF.
Si tratta di un protocollo di routing vettoriale (ogni nodo conosce il nodo a cui mandare il pacchetto per raggiungere la radice, cioè l'edge router) adattivo (non fisso) che usa Object Function (OF) per trovare il percorso migliore da ogni nodo alla radice secondo una metrica.
### DODAG
Il protocollo crea una topologia Destination Oriented Directed Acyclic Graph (DODAG), cioè un grafo orientato che dalle foglie dirige verso l'edge router.
L'obiettivo è creare una topologia logica su una topologia fisica: anche se dei collegamenti esistono, una volta che il grafo viene creato, essi non saranno usati.
##### Control packet
Per creare e mantenere il grafo si usano i pacchetti di controllo:
- DIO
	È il DODAG Information Object ed è usato per stabilire il percorso che va dai nodi foglia verso l'edge router.
- DAO
	È il DODAG Advertisment Object ed è usato per stabilire i percorso dall'edge router ai nodi foglia.
	Se siamo nella modalità non storage allora il DAO deve arrivare alla radice; altrimenti il DAO viene mandato direttamente al nodo parent.
- DIS
	È il DODAG Information Solicitation ed è usato dai nodi interni per sollecitare l'invio del DIO. Viene inviato da un nodo quando entra nella rete.
- DAO-ACK
	È il pacchetto di ACK.
##### Creazione
- Viene designato un nodo come radice, il quale inizia a mandare in broadcasting il DIO.
- Finché i nodi ricevono il DIO calcolano il rank del mittente tramite la metrica usata.
- Scelgono come parent il nodo che ha rank minore.
- Questo processo continua finché tutti i nodi hanno ricevuto il DIO e hanno scelto il proprio parent.
- Una volta che il DODAG è stato creato, il DIO viene mandato periodicamente per aggiornare i percorsi migliori.
### OF e metriche
Le OF sono le metriche usate per trovare il percorso migliore e stabiliscono come calcolare il rank e come selezionare il nodo parent.
RPL permette sia l'impiego delle metriche predefinite sia la creazione di nuove metriche; in questo modo si possono creare nuove Object Function (OF) per lo scopo che si desidera.
Ci sono due metriche di routing predefinite:
- Objective Function zero (OF0)
	Usa il conto degli hop per arrivare al nodo, se un nodo ha rank minore allora è più vicino in termini di hop al nodo radice.
	Questa OF non considera la congestione a livello di nodo: se un nodo è molto vicino alla radice rispetto agli altri allora tutti i messaggi passeranno di esso e quindi sarà congestionato.
- Minimum Rank with the Hysteresis Objective Function (MRHOF)
	È la metrica considerata quando la qualità della connessione è critica: si basa sul numero di tentativi necessari perché il trasferimento abbia successo.
	Per fare queso si usa una sogna: non basta un tentativo fallito per scartare un collegamento, perché il collegamento potrebbe essere buono ma solo non funzionante in quel tentativo.