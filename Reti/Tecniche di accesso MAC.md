# POLLING
È una tecnica di accesso ordinato: l’accesso alla rete è definito da regole prefissate a cui tutti gli utenti devono sottostare ed è controllato da un gestore della rete, detto master.
Il master invia un pacchetto di autorizzazione ad un client, che ha il permesso di usare la rete finché l’autorizzazione non viene rilasciata.
È usata per implementare il livello [MAC delle rete LAN](IEEE%20802%20-%20LAN.md#MAC%20(Medium%20Access%20Control)); un esempio può essere lo standard [Bluetooth](IEEE%20802.15.1%20-%20Bluetooth.md).
##### Autorizzazione
Le modalità con cui l’autorizzazione viene concessa sono:
- Roll-call
	È una tecnica centralizzata: il master invia l'autorizzazione ad un client alla volta seguendo una lista; quando il client ha finito di usare la rete restituisce l'autorizzazione al master.
	Lo svantaggio di questa soluzione è il tempo utilizzato per gestire l’accesso, cioè per inviare e ricevere l’autorizzazione da parte del nodo master; questo si nota particolarmente nel caso pessimo, in cui nessun client vuole usare la rete.
- Hub-Polling
	È una tecnica di tipo distribuito usata per reti con topologia a [bus](Topologia.md#BUS) o a [ring](Topologia.md#RING). Il master invia l’autorizzazione al client a lui più lontano; questo, una volta terminato l’uso della rete, la rilascia e la concede al nodo più vicino a lui in direzione del master; questo processo continua finché non si raggiunge il master e tutti i client hanno avuto la possibilità di usare la rete.
	In questo modo si riesce a risolvere il problema introdotto dalla tecnica roll-call.
##### Tempo di accesso
Le tecniche usate per gestire il tempo di accesso alla rete sono:
- Gated
	Il client può inviare tutti e soli i pacchetti che sono arrivati tra il rilascio dell'autorizzazione precedente e il suo ritorno.
	Il tempo di utilizzo della rete così può essere più o meno grande, ma sarà sicuramente fiinito e non provocherà una monopolizzazione del canale.
- Esaustiva
	Il client non ha alcuna limitazione di tempo per l’utilizzo della rete e può inviare anche i pacchetti che arrivano durante la fase di accesso.
	Chi ha l’autorizzazione preferisce questa modalità, ma può portare ad una monopolizzazione del canale.
# ALOHA
È una tecnica di accesso casuale: un nodo che vuole trasmettere dei dati prova ad accedere alla rete senza dover sottostare a delle regole prefissate o senza dover aspettare una qualche autorizzazione; se due nodi accedono alla rete contemporaneamente si ha una collisione, con la conseguente perdita dell’informazione trasmessa.
È usata per implementare il livello [MAC delle rete LAN](IEEE%20802%20-%20LAN.md#MAC%20(Medium%20Access%20Control)).
##### Accesso
In base alle modalità di accesso alla rete si hanno due varianti:
- Puro
	Gli utenti inviano dati all’access point quando vogliono attraverso un canale. Si mettono poi in ascolto su un canale diverso da quello usato per la trasmissione dati solamente per un determinato lasso di tempo (TimeOut) definito in fase di progettazione della rete; in questo modo si evita di ricevere messaggi indirizzati ad altri utenti. A questo punto abbiamo due scenari:
	- Successo
		L’access point invia sul canale di conferma un messaggio in modalità broadcast, in modo da confermare l'avvenuta ricezione dell'informazione.
	- Collisione
		L'utente non ricevere nessun messaggio di conferma. Questo può avvenire per due motivi: un utente ha provato l’accesso alla rete mentre altri non avevano ancora terminato la loro trasmissione; un utente non ha terminato la sua trasmissione, ma altri hanno tentato un accesso alla rete; si dice quindi che si ha un tempo di pericolo doppio rispetto al tempo di pacchetto.
- Slotted
	Rispetto all'Aloha Puro gli utenti inviano dati all'access point solamente in determinati istanti di tempo, conosciuti e uguali per tutti. 
	In questo modo si rimuove la possibilità di generare un conflitto perché un utente non ha terminato la fase di trasmissione ma altri hanno tentato un accesso alla rete; il tempo di pericolo si dimezza rispetto a quello dell’aloha puro e quindi si raddoppia l’efficienza dell’uso della rete.
##### Collisioni
La risoluzione delle collisioni è simile per entrambi i metodi: terminato il tempo di TimeOut gli utenti possono riprogrammare l'accesso alla rete entro un determinato lasso di tempo detto backOff.
La scelta di quando ripèrovare l'accesso alla rete è indipendente da utente ad utente e viene stabilità su base statistica con probabilità uniforme; in questo modo la possibilità di nuove collisioni è minima.
# CSMA
È una tecnica di accesso casuale: un nodo che vuole trasmettere dei dati prova ad accedere alla rete senza dover sottostare a delle regole prefissate o senza dover aspettare una qualche autorizzazione; se due nodi accedono alla rete contemporaneamente si ha una collisione, con la conseguente perdita dell’informazione trasmessa.
Le tecniche CSMA (Carrier Sense Multiple Access) sono usate per implementare il livello [MAC delle rete LAN](IEEE%20802%20-%20LAN.md#MAC%20(Medium%20Access%20Control)).
##### Accesso
Un nodo prima di accedere alla rete verifica se è libera attraverso il segnale portante, ovvero la frequenza che trasporta effettivamente i dati: se questo viene rilevato allora è avuta una collisione virtuale e si deve riprogrammare l’accesso al canale.
Il problema delle tecniche CSMA sta nei ritardi di propagazione del mezzo fisico, che introduce un ritardo tra il momento in cui un nodo accede al canale e quello in cui gli altri se ne accorgono; per quanto piccoli possano essere questi ritardi non saranno mai nulli. L’efficacia di queste tecniche è tanto maggiore quanto minore è il tempo di vulnerabilità, ovvero il lasso di tempo con cui un segnale raggiunge i due nodi più distanti nella rete.
##### Collisioni
La risoluzione delle collisioni può essere fatta con varie tecniche:
- 1-Persistant
	Si ascolta il canale in cerca del segnale portante: se è libero si trasmette l'informazione, altrimenti si rimane in ascolto finché non lo diventa e poi vi si accede.
	È una tecnica efficace solo se la rete viene usata in modo sporadico; se ci fosse molti accessi ci sarebbero infatti anche molte collisioni.
- Non-Persistant
	Se il canale è libero si procede con la trasmissione dell'informazione; se viene rilevato come occupato si riprogramma l’accesso sulla base del tempo di BackOff (Stessa tecnica usata per l'[aloha](Tecniche%20di%20accesso%20MAC.md#ALOHA)).
	Questa tecnica è efficace in reti dense; non è adatta in reti con accessi sporadici a causa dei tempi di riprogrammazione.
- p-Persistant
	Se la rete viene rilevata come libera allora il nodo vi accede con probabilità $p$; se si ha una collissione si riprogramma l’accesso sulla base del tempo di BackOff (Stessa tecnica usata per l'[aloha](Tecniche%20di%20accesso%20MAC.md#ALOHA)).
	Individuando il valore ottimo di $p$ in relazione al contesto si può ottimizzare la rete.
- CSMA/CD (Collission detection)
	Se il canale è libero si procede con la trasmissione dell'informazione; se si ha una collissione si riprogramma l’accesso sulla base del tempo di BackOff (Stessa tecnica usata per l'[aloha](Tecniche%20di%20accesso%20MAC.md#ALOHA)).
	Si ascolta la rete anche durante la fase di accesso (Cioè di trasmissione); in questo modo in caso di collisione non si deve aspettare il completamento della trasmissione e si aumenta quindi l'efficenza.
	È la tecnica usata nelle reti Ethernet.
# CSMA/CA
La tecnica CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance) è una variante della [CSMA](Tecniche%20di%20accesso%20MAC.md#CSMA).
È usata nelle situazioni in cui il mezzo fisico è il canale radio per risolvere il problema delle interferenze e quindi della possibilità di rilevare delle false collisioni; alcuni esempi possono essere lo standard [IEEE802.11](IEEE%20802.11%20-%20WiFi.md) o il [IEEE 802.15.4](IEEE%20802.15.4.md).
### Tempi di attesa
La gestione dell'accesso si basa sull'intervallo di tempo IFS (Inter Frame Space); esso è suddiviso in sottointervalli, di cui i due più importanti sono:
- SIFS
	Rappresenta il tempo di attesa dei messaggi più urgenti. È usato in due contesti:
	- Messaggi di riscontro ACK (Acknowledgment).
	- Trasmissione di pacchetti successivi al primo: dopo che un nodo ha ottenuto l’accesso al canale ha priorità maggiore sugli altri nodi. Per evitare la monopolizzazione di un canale si assegna un tempo massimo di accesso consecutivo, chiamato TXoP.
- DIFS
	Rappresenta il tempo che un nodo deve attendere prima di entrare nel canale per la prima volta.
### Modalità casuale (DCF)
Nella modalità DCF (Distributed Coordination Function) della tecnica CSMA/CA, come nella classica [CSMA](Tecniche%20di%20accesso%20MAC.md#CSMA), si controlla all'inizio se il canale è libero tramite il segnale portante; nel caso non lo fosse il nodo entra in modalità di contesa.
Se invece il canale è libero si aspetta un tempo di attesa pari a [DIFS](Tecniche%20di%20accesso%20MAC.md#CSMA/CA#Tempi%20di%20attesa); se il canale è ancora libero si invia il pacchetto e si attende il messaggio di ACK per un tempo pari a [CIFS](Tecniche%20di%20accesso%20MAC.md#CSMA/CA#Tempi%20di%20attesa); se il riscontro arriva l’invio è terminato con successo, altrimenti il nodo si mette nello stato di contesa.
##### Contesa
Nello stato di contesa il nodo imposta un contatore a decremento, detto backOff timer, con un valore scelto su base statistica con probabilità uniforme in un intervallo di tempo CW, definito come $CW(n) = min\{2CW(n − 1), CW_{max}\}$; si noti che raddoppiando la sua ampiezza ad ogni tentativo di accesso fallito la probabilità di avere una nuovo collisione al tentativo $n+1$ è molto inferire rispetto al tentativo $n$; quando il valore di CW raggiunge il suo valore massimo il pacchetto viene scartato, perché si pensa che sarebbe più attuale.
##### Terminale nascosto
Supponiamo che ([Figura 7.7a](Figura%207.7.png)) A stia inviando un pacchetto a B e che anche C decida di comunicare con B; il nodo C, non riscontrando il canale come occupato perché A è fuori dalla sua area di copertura, dopo aver atteso un tempo pari a DIFS invia il suo pacchetto a B provocando una collisione.
Questo problema si risolve con una procedura di handshake: dopo che A ha atteso il tempo DIFS, se il canale è ancora libero A invia un messaggio breve, detto RTS, a B per indicargli la volontà di aprire una comunicazione; questo messaggio è ricevuto da tutti i nodi nel raggio di copertura di A, ma non da C; B, dopo aver ricevuto l’RTS, attende il tempo SIFS e invia un messaggio di conferma, detto CTS, ad A; questo messaggio sarà ricevuto da tutti i nodi nel raggio di copertura di B, e quindi anche da C, che lo interpreteranno come una richiesta di attesa alla comunicazione; una volta che A riceve il messaggio CTS può iniziare a comunicare con B.
##### Terminale esposto
È un problema introdotto dalla procedura di handshake. Supponiamo che ci sia un altro nodo D nell’area di copertina di B e C ([Figura 7.7b](Figura%207.7.png)); il messaggio CTS inviato da B sarà ricevuto da C e da D, che lo interpreteranno nello stesso modo; se D volesse comunicare con C teoricamente lo potrebbe fare perché il canale tra i due è libero, ma praticamente non lo farà, perchè D non accede alla rete a causa del messaggio CTS inviato da B.
Da questo si nota che la procedura di handshake riduce notevolmente la probabilità di collisioni, ma diminuisce anche l’efficienza della rete sia per il numero di comunicazioni contemporanee tra terminali sia per il tempo di set-up. Purtroppo questo problema non è risolvibile.
### Modalità senza contesa (PCF)
La modalità PCF (Point Coordination Function) della tecnica CSMA/CA si può usare solo nelle reti [WLAN infrastrutturate](IEEE%20802.11%20-%20WiFi.md).
A turno l’AP interroga i nodi abilitati a questo tipo di comunicazione, permetto loro la trasmissione senza contesa dei pacchetti.
Questo tipo di comunicazione è ha priorità maggiore rispetto alla modalità DCF; per impedire un monopolio della rete dai nodi autorizzati a questo tipo di comunicazione si è previsto un tempo massimo per la fase di PCF.