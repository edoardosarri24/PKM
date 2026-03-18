La tecnologia RFID (Radio Frequency Identification) è la prima tecnologia che si è rivolta al mondo degli oggetti.
Il funzionamento è semplice: i tag contengono informazioni su persone od oggetti (sull'ente a cui è associato il tag); il reader legge (tramite onde radio, quindi senza contatto) i dati dal tag e li carica in memoria (server locali o cloud); a questo punto si possono eseguire analisi per identificare, categorizzare e fare tracking.
##### IoT
L'IoT in questo concetto sta nell'associare i tag a oggetti e nel recuperare le informazioni in modo automatico (senza supervisione dell'uomo).
# COMPONENTI
### Tag
Hanno basso costo (0,5/250€) e dimensioni contenute (si possono inserire ad esempio su smart card, cartellini).
Consistono in:
- Un semplice chip con le funzioni di controllo.
- Memoria
	Contiene le informazioni. La sua dimensione va da 16 bit (contiene un semplice ID univoco) a 512kbit (contiene qualche altra informazioni). Può essere read-only oppure read-wirte.
- Un'antenna (solitamente una)
##### Categorie
Si distinguono in:
- Attivi
	I dati sono trasferiti usando la batteria del tag. Il range di comunicazione è alto e si parla di circa 200m.
- Passivi
	I tag riflettono il segnale radio mutuato che proviene dal lettore. La potenza gli viene erogata dal reader. Il range è basso e va dai centimentri ai 3 metri.
- Semi passivi
	La batteria è utilizzata solo per dare potenza a un microprocessore, ma non per il trasferimento dei dati, che si fa in modo passivo. Il range è basso e si parla di circa 20/30m.
### Reader
È un dispositivo mobile o fisso controllato da un microprocessore usato per prendere i dati dal tag.
### Host system
È il sistema collegato al reader. Permette di memorizzare le informazioni lette dagli oggetti tramite il reader e di implementare applicazioni che si basano su queste.
Le applicazioni più utilizzate sono: identificazione, categorizzazione e tracking. Gli ambienti dove vengono utilizzate sono magazzini.
## TRASMISSIONE
La trasmissione può essere:
- Magnetica
	Si ha una distanza di massimo mezzo metro. È usata quando le frequenze richiedono una bassa distanza.
- Elettromagnetica
	La distanza può essere da 1 a 10 metri. I casi d'uso sono quelli in cui le frequenze possono coprire distanze maggiori.
# FREQUENZE
### <150kHz
Sono frequenze buone per le applicazioni dove si ha una distanza bassa, pochi dati e un basso bit rate.
- Vantaggi
	- Sono frequenze libere e senza un regolamento.
	- Penetra bene i materiali come acqua, indumenti e legno.
- Svantaggi
	- Non penetra i metalli, come ferro e acciaio.
	- Le antenne sono grandi rispetto a quelle delle alte frequenze.
### 13,56MHz
È la banda di frequenza universale e usata in tutto il mondo; un esempio sono le smart card.
È una buona frequenza per le applicazioni dove si ha una distanza bassa e pochi dati.
- Vantaggi
	- Penetra bene i materiali come acqua, indumenti e legno.
	- L'antenna è semplice e costa poco.
	- Ha un bit rate maggiore delle frequenze sopra.
	- Permette tag più piccoli rispetto alla frequenza sopra.
- Svantaggi
	- È una frequenza regolamentata.
	- Non penetra i metalli, come ferro e acciaio.
	- Le antenne sono grandi rispetto a quelle delle alte frequenze.
	- I tag sono più complessi.
##### Near Field Communication (NFC)
Questa tecnologia utilizza la frequenza di 13,56MHz e può raggiungere una velocità di 424kbit/s.
Permette una connettività bidirezionale a corto raggio. Quando i due dispositivi vengono in contatto si crea una rete peer2peer e possono scambiarsi informazioni.
### 300MHx<x<1GHz
È una buona frequenza per le applicazioni dove si ha un'ampia distanza, molti dati e un alto bit rate. I casi d'uso sono i magazzini o i centri di logistica
- Trasmissione
	La trasmissione avviene in modo elettromagnetico. Quando l'onda emessa dal reader colpisce l'antenna del tag una parte dell'energia viene assorbita per alimentare il tag e un'altra parte viene riflessa (effetto backscatter). L'energia assorbita è troppo bassa per alimentare il tag e allora i dati sono trasferiti proprio dall'effetto basckscatter: il lettore deve ricavare i cambiamenti del segnale riflesso rispetto all'originale.
- Vantaggi
	- Penetra i metalli.
	- I tag e le antenne sono più piccole di quelli che operano alle frequenze sopra.
	- Permettono comunicazioni non in linea d'aria.
- Svantaggi
	- Non penetra materiali come acqua e tessuti.
	- Ci sono problemi di normative e regolamentazioni. Non sono frequenze libere.
### 2,45GHz
È una buona frequenza per le applicazioni dove si ha un'ampia distanza, molti dati e un alto bit rate.
- Vantaggi
	- Tag e antenna più piccoli della frequenza sopra.
	- Permettono comunicazioni non in linea d'aria.
- Svantaggi
	- Più suscettibile al rumore della frequenza sopra.
	- È la stessa frequenza di altre tecnologie, come WiFi, Bluetooth e ZigBee.
### >5,8GHz
È una buona frequenza per le applicazioni dove si ha una bassa distanza. Un esempio è il telepass.
- Vantaggi
	È una frequenza alta e quindi ci sono pochi dispoisitivi che la usano e quindi poca interferenza.
- Svantaggi
	- Non dispobnibile negli USA.
	- Richiede una direzionalità dell'antenna.
	- Il chip dei tag è complesso da costruire e costoso.
# TOPOLOGIA
In una rete di sensori dove sono presenti anche tag RFID possiamo avere due situazioni:
- Reader-as-sensor architecture
	Il reader è allo stesso livello dei sensori. In questo caso il reader immagazzina tutti i dati dei tag e li manda al server come fanno tutti gli altri sensori; questo non riesce quindi a distinguere le informazioni provenienti dai device (tag) e dai sensori.
- Tag-as-sensor architecture
	Il reader combina funzioni di gateway, mentre i tag e i sensori stanno sullo stesso livello. In questo modo il server può distingue le informazioni degli oggetti (tag) e quelle dei sensori.