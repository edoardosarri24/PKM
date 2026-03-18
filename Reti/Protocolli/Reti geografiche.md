## RACCOMANDAZIONE X.25
È stata la prima rete pubblica concepita per la trasmissione dei soli dati da parte degli utenti retail.
Lo standard identifica i dispositivi utente come DTE (Data Terminal Equipment) e il nodo della rete come DCE (Data Circuit Equipment); la raccomandazione X.25 definisce solo le modalità con cui DTE e DCE si interfacciano.
##### Architettura
Prevede un’[architettura protocollare a livelli](Architettura%20a%20livelli.md):
- Pacchetto (Rete)
	È detto anche PLP ed è il corrispettivo del livello di rete nel protocollo [ISO/OSI](ISO%20OSI.md). Definisce le procedure con cui i pacchetti vengono inviati: vengono loro aggiunte le informazioni riguardanti il percorso da seguire e il numero di sequenza all’interno del flusso dati a cui il pacchetto appartiene. L’instradamento dei pacchetti può essere di due tipi:
	- Chiamata virtuale (VC)
		Necessita della fase di set-up per ogni richiesta di collegamento; il percorso tra due utenti è quindi scelto di volta in volta.
	- Chiamata virtuale permanente (PVC)
		Il percorso tra due utenti è scelto solo al primo collegamento; viene successivamente reso disponibile senza ritardo, cioè senza la fase di set-up.
- Collegamento
	Si occupa di garantire l’integrità della trasmissione su base L2L; in questo contesto questo livello è particolarmente importante perché solitamente i mezzi fisici sono di bassa qualità; per farlo questo livello crea la trama LAPB (Etichetta particolare). I campi flag all’inizio e alla fine sono composti da una sequenza conosciuta e fissa di 8 bit (01111110) e servono per delimitare l’inizio e la fine della trama; tale sequenza potrebbe essere presente anche nel campo dati e potrebbero essere interpretati come il campo flag alla fine; per impedire questo si usa la tecnica del bit stuffing, che comporta l’inserimento di un bit fittizio 0 dopo una sequenza di 5 bit a 1; in ricezione si eseguirà l’operazione opposta.
- Fisico
	Definisce le modalità di  accesso al canale. 
## FRAME RELAY
Sono nate per aggregare reti separate in reti WAN (Wide Area Network) in modo efficiente. La raccomandazione X.25 gestisce la commutazione al livello di rete e presenta le seguenti criticità:
- Velocità di riferimento di 64Kbps bassa.
- Il livello collegamento, responsabile dell’integrità dei dati, introduce una complessità elevata a causa del mezzo fisico di scarsa qualità.
- I pacchetti devono seguire lo standard imposto e questo comporta l’incapsulamento dei pacchetti [TCP/IP](TCP%20IP.md) e un aumento dei tempi di elaborazione.
##### Vantaggi
La rete frame relay risolve questi problemi e ha le seguenti caratteristiche:
- Permette velocità di trasferimento elevate: la velocità di riferimento è 1,5Mbps.
- Ha un’architettura semplificata e quindi è efficiente per collegare reti [TCP/IP](TCP%20IP.md).
- Consente l’allocazione di banda su richiesta degli utenti.
- Permette di inoltrare pacchetti di grandi dimensioni.
- Gestisce l’integrità dei dati in modo [non affidabile](Architettura%20a%20livelli.md#SERVIZIO): i pacchetti corrotti sono scartati. La correzione degli errori è affidata a layer superiori, che non fanno parte di questo protocollo.
##### Percorso virtuale
Il percorso che i pacchetti devono seguire si basa sul [circuito virtuale](Trasmissione.md#Circuito%20virtuale); ad ogni percorso è associata un’etichetta che lo identifica, denominata DLCI (Data Link Connection Identifier). Il collegamento per la trasmissione di dati può essere di due tipi:
- Temporaneo
	È reso disponibile su richiesta per il tempo necessario al trasferimento. Necessita di una fase di set-up e dopo il trasferimento di dati non esiste più.
- Permanente
	La fase di set-up si esegue una volta sola e il collegamento è sempre disponibile senza attesa. Ha un costo maggiore ed è limitato ad  una sola coppia di utenti. 
## ISDN
Per realizzare questo tipo di reti si è pensato di ottimizzare le infrastrutture già esistenti senza crearne di nuove. L’obiettivo era definire un’interfaccia standard con cui dispositivi diversi, che magari usavano anche tipi di [commutazione](Trasmissione.md) diversi (Di pacchetto, di circuito e di tipo specifico ed esclusivo), si potevano interfacciare alla rete in modo omogeneo (Figura 9.8).
##### Canali
Il trasporto delle informazioni è strutturato in base del tipo di canale usato. I canali sono:
- B
	È il canale base di un utente ISDN e ha una velocità di 64Kbps. Di solito è dedicato ad un collegamento [telefonico](Telefonia.md#TELEFONIA%20NUMERICA/DIGITALE) o per la trasmissione di dati.
- D
	Ha una velocità di 16/64Kbps. È usato per il trasferimento delle informazioni di segnalazione relative al canale B o per il trasferimento di dati a basso bit rate.
- H
	È usato per il trasferimento di dati con prestazioni più elevate rispetto a quelle del canale B.
##### Servizi
I canali ISDN sono strutturati in modo diverso per offrire servizi e prestazioni diverse in base alle necessità degli utenti. I tipi di accesso sono:
- Base
	Si aggrega in [full duplex](Reti%20di%20telecomunicazioni.md#DEFINIZIONI#Full%20duplex) 2 canali B e un canale D, per una velocità totale di 144Kbps. I clienti di riferimento sono i retail che hanno bisogno di due canali voce o di un canale voce e un collegamento a commutazione di pacchetto.
- Primario
	È pensato per clienti con esigenze più specifiche. Sono previsti 30 canali B e un canale D o un canale H a seconda delle esigente del cliente. La velocità tipica è di 2048Kbps.
##### Architettura
L'architettura del sito di un utente della rete ISDN è strutturata in gruppi funzionali, separati da punti di riferimento.
- Gruppo funzionale
	È un insieme di dispositivi. I gruppi funzionali sono:
	- NT1
		È il "Network Termination 1", cioè l’ultimo dispositivo appartenente alla rete più vicino all’utente.
	- NT2
		È il "Network Termination 2"; non è sempre presente e fornisce le funzionalità aggiuntive di commutazione locale e di multiplazione.
	- TE
		È il "Terminal Equipment". Sono i dispositivi degli utenti e si dividono in type 1, se sono nativamente compativi con lo standard ISDN, e type 2, se non sono nativamente compatibili con la rete ISDN; per quest’ultimi è necessario un ulteriore dispositivo, detto TA (terminal Adapter), che gli permetta di potersi interfacciare in modo corretto.
- Punti di riferimento
	Rappresentano la separazione logica dei gruppi funzionali. Questi sono:
	- T
		È la separazione tra i dispositivi utente e dispositivi di rete.
	- S
		È l’interfaccia dei dispositivi nativamente compatibili con lo standard ISDN.
	- R
		È l’interfaccia dei dispositivi non compatibili con lo standard ISDN.
	- U
		È la connessione tra la centrale ISDN e NT1.
## ATM
Le reti ATM (Asynchronous transfer Mode) sono le successive delle ISDN e sono state pensate per trasferire con velocità elevate una grande quantità di dati di tipo diverso (Voce, dati e multimedia) in modalità [connection oriented](Architettura%20a%20livelli.md#SERVIZIO).
L’unico mezzo fisico usato è la fibra ottica; questo, insieme alla commutazione basata sul label swapping, rende possibile il trasferimento a grandi velocità e premette di avere una grande affidabilità; per questo motivo ci possiamo permettere di effettuare il controllo degli errori su base E2E.
Oggi sono usate nelle dorsali per il collegamento ad alta velocità. I servizi si differenziano in vari classi: bit-rate constante, bit-rate variabile in real time e non in real time e servizi best effort (Non si garantisce il trasferimento che avviene solo se c’è disponibilità di banda).
##### Multiplazione
La caratteristica delle reti ATM è la multiplazione asincrona, resa possibile grazie alla dimensione fissa delle celle. Impedire che un pacchetto grande occupi la linea per il tempo necessario al sua trasferimento, permette di rispettare meglio l’ordine di arrivo dei pacchetti sui vari nodi; la tecnica di accesso al mezzo fisico è di tipo TDMA asincrona: si accede solo in istanti di tempo prefissati.
Un nodo ATM non risulta mai inattivo: se non deve inviare nulla invia una cella con il campo dati fittizio.
##### Architettura
L’ATM prevede un’architettura in tre dimensioni: il piano utente è responsabile dei servizi richiesti dall’utente; il piano di controllo ha il compito di gestire il controllo e la segnalazione; il piano di gestione si occupa di far cooperare correttamente i due livelli precedenti.
I livelli utente e di controllo sono ulteriormente sotto struttati nei seguenti livelli:

- Fisico
	Si divide in PM (Physical Medium), che rappresenta l’interfaccia con il mezzo fisico, e TC (Trasmission Convergence), responsabile dal lato del mittente di trasformare le celle in un flusso di dati e lato ricevente dell’operazione opposta.
- ATM
	È indipendente dal livello fisico usato ed è responsabile della consegna delle celle tra mittente e destinatario. Nei commutatori delle rete (Nei nodi di transito e non nei nodi sorgente e destinazione) è il livello più alto e il suo compito è quello di instradare le celle nella rete attraverso l’operazione di commutazione: si leggono i campi VPI e VCI contenuti nell’header della cella; nella routing table si leggono i nuovi valori riferiti a questi due campi; si effettua il label swapping, impostando questi campi sui nuovi valori.
- AAL (ATM Adaptation Layer)
	È un livello implementato solo nei nodi mittente e ricevente; ha il compito di convertire le informazioni ricevute dagli strati superiori nel formato compatibile con le celle ATM.
	Questa conversione è diversa per ogni servizio richiesto dal livello superiore; per questo motivo sono stati previste quattro suddivisioni:
	- 1
		Servizi con bit rate costante e [connection oriented](Architettura%20a%20livelli.md#SERVIZIO); ottima scelta per servizi isocroni (In real time). Un esempio sono la trasmissione di voce e video. 
	- 2
		All’inizio era per servizi isocroni con  bit rate variabile. Adesso è usato per servizi con un basso bit rate come il traffico di reti cellulari.

	- 3/4
		All’inizio erano servizi separati: [connection oriented](Architettura%20a%20livelli.md#SERVIZIO) il primo e [connectionless](Architettura%20a%20livelli.md#SERVIZIO) il secondo. Adesso sono stati aggregati in un unico servizio non isocrono, sia connection oriented che connectionless.
	- 5
		Serve per servizi best-effort (Cioè dove non si garantisce la qualità del servizio); infatti non ci sono controlli sugli errori o sull’ordine delle celle.
##### Condivisione logica del mezzo
La connessione fisica tra due utenti è definita dal canale di connessione (TP, Trasmission Path), che ha una certa banda; la banda di questo canale è suddivisa in più cammini virtuali (VP, Virtual Path); la banda di ogni VP viene ulteriormente suddivisa in modo dinamico in più circuiti virtuali (VC, Virtual Circuit). In questo modo all’interno di un collegamento tra due utenti (Figura 12.7) non viene sprecata la banda a disposizione: a servizi diversi sono destinati VC diversi con bande diverse.

Ogni connessione virtuale tra due utenti deve essere riconosciuta in modo univoco. Questo viene realizzato con un’identificazione gerarchica a due livelli: l’identificatore primario è l’identificatore di cammino virtuale VPI e quello secondario è l’identificatore di circuito virtuale VCI; na connessione virtuale viene definita quindi dalla coppia VPI/VCI.