Il routing coinvolge tutti i router presenti tra destinato e mittente: si deve scegliere il percorso ottimo tra tutti quelli che tali router mettono a disposizione; il forwarding coinvolge il singolo router: è il semplice trasferimento dell’informazione dalla porta di ingresso ad una di uscita.
Un algoritmo di routing viene valutato in base a:
- Semplicità
	Deve essere semplice da implementare e non richiedere un alto costo computazionale.
- Robustezza
	Deve funzionare per qualsiasi tipo di rete ed essere in grado di adeguarsi a cambiamenti di topologia o a guasti.
- Ottimalità
	Deve generare il percorso ottimo dal mittente al destinatario.
## CLASSIFICAZIONE
- Statici
	Si basano su decisioni predefinite. Non sono algoritmi reattivi a guasti della rete o al cambiamento delle condizioni di traffico.
- Dinamici
	Prevedono delle procedure di aggiornamento periodico e attuano le loro decisioni in base al contesto attuale.
- Con tabella
	Associano uscite ad entrate.
- Gerarchici
	Suddivide una rete in sottoreti. È adatto per situazioni in cui una rete è molto grande e avere una tabella memorizzata in ogni router sarebbe troppo costoso. Le sottoreti possono implementare il routing in modo indipendente tra loro.
- Senza tabella
	Non prevedono abbinamenti tra entrate ed uscite.

- Centralizzato
	Il percorso da seguire viene deciso, di solito tramite l'uso di tabelle, da un singolo router centrale. Un esempio sono le reti SDN.
- Distribuito
	Si sceglie il percorso da seguire in modo cooperativo, cioè basandosi sui dati posseduti da più router. Ad oggi è la soluzione più diffusa.
- Isolato
	Si usano in contesti senza tabelle e prevede l’esecuzione dell’algoritmo in locale.
## ALGORITMI SENZA TABELLA
### Random
Un pacchetto, una volta che ha raggiunto un router, viene inoltrato su una porta di uscita scelta su base statistica (Di solito con distribuzione uniforme).
Sono algoritmi robusti perché si adattano bene ai cambiamenti della rete non previsti; non garantiscono però la scelta ottimale del percorso da seguire.
### Flooding
Un pacchetto, una volta che raggiunge un router, viene inoltrato su tutte le porte di uscita del router.
Riduce al minimo l’elaborazione all’intento del router e rende molto sicura la consegna del pacchetto al destinatario, tanto che viene usata in situazioni in cui l’affidabilità è necessaria (Applicazioni militari e Safety-Critical).
Lo svantaggio principale è il [broadcast storm](Reti%20di%20telecomunicazioni.md#Broadcast%20storm); per limitare questo problema ci sono due variazioni:
- Utilizzo del campo [TTL](IPv4.md#HEADER) nell’header IPv4 o [Hop limit](IPv6.md#HEADER) nell'heaader IPv6. Questo permette di non far circolare troppe copie del pacchetto all’interno della rete.
- Un router memorizza una copia di ogni pacchetto ricevuto; se lo stesso paccheto ritorna ad un router già attraversato può essere scartato.
##### Flooding selettivo
È una variante del flooding e si basa sull'idea che il pacchetto sia destinato ad un roter che si trova in direzione opposta a quella del mittente. Quando un router riceve un pacchetto lo invia quindi nelle porte di uscita che hanno direzione opposta a quella di ricezione.
### Direct diffusion
I dati presenti nella rete sono contrassegnati da un’etichetta costituita dalla coppia valore-attributo. È utilizzata nella rete di [sensori](WSN%20-%20Wireless%20Sensor%20Networ.md).
Quando un nodo vuole ricevere un’informazione, invia nella rete, attraverso la modalità flooding, il tipo di informazione che richiede (La coppia sopra specificata). I nodi che possiedono l’informazione gli comunicano le condizioni (Qualità dei dati, affidabilità, bit rate) con cui possono inviare i dati richiesti. Il richiedente sceglie quella che più lo soddisfa inviando sulla rete le caratteristiche scelte; il nodo che ha un riscontro procede con l’invio.
Il richiedente più eseguire anche un reinforcement, ovvero può diventare più specifico sulle condizioni che il percorso deve rispettare.
### Spin
Quando un nodo riceve o genera un pacchetto invia su base flooding i suoi metadati, cioè le informazioni su cosa il pacchetto contiene. I dati veri e propri vengono inviati solamente ai nodi che sono effettivamente interessati. È utilizzato nelle reti di [sensori](WSN%20-%20Wireless%20Sensor%20Networ.md).
In questo modo si riduce notevolmente il [broadcast storm](Reti%20di%20telecomunicazioni.md#Broadcast%20storm).
La nota negativa è che non tutti i nodi hanno la possibilità di ricevere il pacchetto, come nel caso in cui un nodo interessato ha tutti vicini che non ricevono il pacchetto perché non interessati.
### Source routing
Il nodo sorgente specifica all’interno dell’header il percorso che il pacchetto inviato deve seguire. Questa informazione viene acquisita dal mittente in due modi diversi:
- Path server
	Viene contattato un server che ha memorizzati tutti i percorsi sorgente/destinazione possibili. È una soluzione di tipo centralizzato e statica: se il server ha dei problemi l'interà rete si blocca; se la rete ha un problema e il server non viene aggionrato allora alcuni percorsi non saranno più dispobili.
- Path discovery
	Il nodo sorgente invia in flooding un pacchetto esploratore nel quale viene specificato solo l’ID del mittente e quello del destinatario; ogni volta che il pacchetto attraverso un nodo della rete si memorizza il suo ID; il primo pacchetto che raggiunge il nodo destinazione è quello che ha attraverso il percorso migliore; viene quindi rinviato al mittente seguendo il percorso inverso.
## ALGORITMI CON TABELLA
In generale possono essere sia centralizzati che distribuiti e sia dinamici che statici; noi quelli con tabella, distribuiti e dinamici.
Il principio di ottimalità consiste nel trovare gli shortest path; un percorso può avere il costo minimo in base a più concetti, come la distanza dei collegamenti, l’ampiezza di banda o l’affidabilità.
### Distace vector
Ogni router ha una tabella di instradamento che contiene tutte le destinazioni, il costo per raggiungerle e il prossimo router a cui inviare il pacchetto.
##### Algoritmo
La tabella viene costruita attraverso l’algoritmo di Bellman-Ford: si vuole trovare lo shortest path in modo progressivo a partire dal nodo sorgente.
Il funzionamento dell'algoritmo si basa sul distance vector: dato un router è una forma semplificata della sua tabella d'instradamento, dove sono reperibili le informazioni riguardanti i nodi destinazione e il costo per raggiungerli.
La tabella è costruita in modo incrementale: all’inizio le informazioni significative di un dato router sono solo quelle relative ai collegamenti con suoi vicini; ogni volta che un router aggiorna la propria tabella deve inviare ai suoi vicini il proprio distance vector; quando un router riceve un distance vector deve confrontare per ogni destinatario i costi presenti nella sua tabella di routing con quelli ottenuti sommando i costi presenti nel distance vector e quelli per raggiungere il mittenete del distance vector; si crea quindi la propria nuova tabella d'instradamento mettendo come costo per ogni destinazione il costo inferiore tra i due.
##### Considerazioni
- L’algoritmo permette di ottenere la soluzione ottima ed è molto semplice da implementare.
- Gli svantaggi sono la complessità computazionale, la lenta convergenza verso un’instradamento ottimale e la sua difficile applicazione in reti complesse. I primi due aspetti rendono questo algoritmo non indicato in una rete molto dinamica.
##### Conteggio all’infinito
La lenta convergenza dell’algoritmo porta al problema del conteggio all’infinito. Spieghiamolo con una rete composta da tre nodi A->B->C in serie: supponiamo che il collegamento B->C si interrompa; B aggiornerà la propria tabella e invierà il distance vector ad A; se però in questo lassso di tempo A aggiorna la propria e invia a B il proprio distance vector, quest’ultimo vedrà il collegamento con C come ancora attivo e aggiornerà la propria tabella in relazione a questo collegamento; B invia a questo punto il proprio distance vector ad A, che a sua volta aumenterà il costo del proprio collegamento con C; si crea così un ciclo infinito dove il costo del collegamento con C cresce sempre, sembra disponibile, ma in realtà non lo è.

Per ovviare a questo problema ci sono tre soluzioni:
- Infinito finito
	Una volta che il costo di un collegamento raggiunge un certo limite superiore esso viene considerato come non disponibile. In questo modo si risolve il problema, ma ci vuole molto tempo per arrivare a questa situazione.
- Split horizon
	Prevede l’invio del distance vector solo ai nodi non direttamente collegati con il nodo destinazione aggiornato. Questo introduce un problema, derivante dal fatto che nell’algoritmo se un’informazione non viene aggiornata da molto tempo si considera non più attuale e quindi viene eliminata.
- Poisoned reverse
	È una variante dello split horizon: ai nodi direttamente collegati con il nodo destinazione aggiornato si invia comunque un’informazione, ma questa contiene un valore del costo molto alto; in questo modo il valore si aggiorna comunque, ma senza modificare le informazioni all’interno della tabella.
##### RIP (Routing Information Protocol)
È una delle prime implementazioni concrete dell’algoritmo Distance Vector ed è ancora oggi utilizzata.
- La metrica di costo usata è il numero di salti eseguiti.
- I nodi si scambiano informazioni ogni 30 secondi o dopo ogni aggiornamento, attraverso messaggi detti RIP response che si possono riferire ad un massimo di 25 destinazioni. Ogni router può ricevere al massimo un RIP response contemporaneamente.
- Se un router non invia aggiornamenti da più di 180 secondi viene considerato irraggiungibile.
- Comprende un ulteriore tipo di messaggi con cui un router richiede l’invio delle informazioni a seguito della propria attivazione inziale.
- Il numero massimo di salti per cui si considera un nodo non raggiungibile è 15 hop. Questo limita l’utilizzo dell’algoritmo a reti non troppo estese.

L’algoritmo ha le seguenti caratteristiche:
- La convergenza è relativamente veloce per via della limitazione superiore del costo di collegamento.
- La robustezza dell’algoritmo è data dalle modalità di aggiornamento delle tabelle dei router.
### Link state
Con questo algoritmo, detto algoritmo di Dijkstra, si vuole trovare lo shortest path definendolo per costi crescenti. Assume che ogni nodo sia a conoscenza dell’intera mappa della rete.
##### Algoritmo
Basa la conoscenza globale della rete a partire dalla conoscenza dei collegamenti verso i nodi vicini.
Una volta venuto a conoscenza di queste singole informazioni, il nodo le condivide con il resto della rete in modalità [flooding selettivo](Routing.md#Flooding%20selettivo) attraverso gli LSP (Link State Packet).
Ogni nodo ha un proprio database LSP dove memorizza tutte le informazioni degli altri nodi della rete; questo viene poi usato per definire localmente (In modo indipendente dagli altri nodi) tutti i percorsi disponibili e il loro costo.
##### Considerazioni
- Consente di gestire reti di grandi dimensioni con una convergenza rapida.
- Lo svantaggio è legato alla complessità dell’implementazione e ai costi di elaborazione all’interno di ogni nodo.
##### Riceive processor
Quando un router riceve un pacchetto si attiva il receive processor che ne deve verificarne il tipo:
- Destinato al router
	Viene inviato ai livelli superiori.
- Di transito
	Viene trasferito al forwarding processor. Questo interroga il database locale usando come chiave l’indirizzo di destinazione del pacchetto e determina la porta di uscita sul quale inoltrarlo.
- Di Hello
	Se il router ricevente conosce il vicino non fa nulla. Altrimenti lo inserisce nella lista dei suoi vicini e notifica questo aggiornamento a tutti nodi della rete attraverso un messaggio LSP.
- LSP
	Il router sorgente trasmette il messaggio LSP in modalità flooding. Una volta che un router riceve un pacchetto LSP: se il numero di sequenza è maggiore del più grande numero di sequenza presente nel database o se non ha mai ricevuto pacchetti LSP da quella sorgente allora aggiorna il proprio database e lo ripete in modalità flooding selettivo; se il pacchetto LSP ricevuto ha lo stesso numero di sequenza del numero di sequenza più grande all’interno del database non fa nulla; se il pacchetto LSP ricevuto ha lo un numero di sequenza inferiore rispetto al numero di sequenza più grande all’interno del database trasmette al mittente il proprio pacchetto LSP, in modo che questo possa aggiornarsi.
##### OSPF (Open Shortest Path First)
È il successore dell’algoritmo RIP. È aperto, cioè il suo codice è disponibile e usabile da tutti.
- La metrica di costo è astratta, cioè non accoppiato all’algoritmo. In questo modo si può quindi considerare sia il numero di salti che la larghezza di barda dei singoli collegamenti.
- I nodi si scambiano informazioni ogni 30 minuti o dopo ogni aggiornamento.
- Comprende un ulteriore tipo di messaggi, detto Hello, con cui un router richiede l’invio delle informazioni a seguito della propria attivazione.
-  Quando sono presenti più percorsi con lo stesso costo si può mantenere entrambi attivi e non sceglierne uno solo. In questo modo si gestisce meglio il carico di lavoro.
- Permette di gestire strutture gerarchiche.
## MPLS (Multiprotocol Label Switching)
È un algoritmo introdotto per migliorare la velocità di instradamento dei pacchetti. Il principio su cui si basa è stato mutuato dalle rete [frame relay](Reti%20geografiche.md#FRAME%20RELAY) e [ATM](Reti%20geografiche.md#ATM), cioè si basa sulla commutazione di etichetta.
### Traffic engineering
MPLS introduce quello che è il traffic engineering: creando una pila di etichette con ordine LIFO si possono accorpare più flussi di dati diversi e trattarli come uno singolo.
Ai flussi che devono seguire lo stesso percorso, LSP (Label Switched Path) viene associata la stessa etichetta; una volta assegnata questa etichetta il pacchetto viene inoltrato su una porta di uscita; all’arrivo nel router di destinazione si rimuove l’etichetta e la si sostituisce con la prossima oppure si segue il percorso definito dall’etichetta successiva della pila.
Questo è ottimo in un contento di VPN (VirtualPrivate Network), con cui si possono nascere i singoli indirizzi IP sotto una pila serie di etichette.
### Etichetta
Il protocollo MPLS non fa altro che aggiungere un’etichetta a 32 bit nell’header IP. I suoi campi sono:
- Label
	Identifica l’etichetta ed ha significato su base L2L. I valori di questo campo vengono decisi dai vari router.
- Exp
	È stato definito come sperimentale. Viene usato ad esempio per definire il tipo di servizio trasmesso.
- S
	È un campo formato da un siglo bit. Se posto a 1 significa che l’etichetta corrente è l’ultima della pila, altrimenti ce ne sono altre.
- TTL
	È il time Time To Live e ha lo stesso significato dell’omonimo campo dell’header IP.
## ROUTING MULTIPLO
Fino ad ora abbiamo visto gli algoritmi di routing unicast, cioè con sorgente e destinazione unica.
Le applicazioni dell’instradamento multiplo possono essere l’accesso a database distribuiti, la diffusione delle informazioni e le teleconferenze.
### Broadcast
È la situazione in cui il pacchetto generato da una sorgente deve essere inviato a tutti i partecipanti della rete. In ordine crescente di efficienza si può:
- Si realizza effettuano tanti trasferimenti unicast quanti sono i nodi della rete. Si ha lo svantaggio di avere tante copie del pacchetto quanti sono i nodi.
- Il multidestination routing prevede l’invio di un solo pacchetto, il cui campo di indirizzo è una lista di indirizzi. Quando il pacchetto arriva primo router viene duplicato tante volte quante sono le porte di uscita; nel campo indirizzo di ogni pacchetto duplicato vengono inseriti solo gli indirizzi raggiungibili da quel percorso. In questo modo il campo di indirizzo si riduce sempre di più, fino ad arrivare ad essere composto solo dall’indirizzo del destinatario. Gli svantaggi sono: il nodo sorgente deve conoscere gli indirizzi di tutti i partecipanti alla rete; i tempi di elaborazione all’interno di ogni router sono elevati.
- Flooding.
- Nel RPF (Reverse Path Forwarding) il pacchetto viene inoltrato su tutte le uscite del router solo se non sono arrivate prima delle sue copie (Il router deve mantenere una lista di pacchetti arrivati).
- Con lo spanning tree, con radice il nodo sorgente, si riesce ad avere tutti percorsi ottimi. Il problema sta nella conoscenza dello spanning tree di tutti i nodi della rete; è infatti una soluzione percorribile solo se la rete implementa l’algoritmo Link State. Quando un nodo vuole essere aggiunto allo spanning tree lo si fa cercando il cammino più breve che vada dalla radice al nodo da aggiungere; l’operazione di rimozione di un nodo dalla rete è detta potatura.
### Multicast
È la situazione in cui il pacchetto generato da una sorgente deve essere inviato a più nodi della rete. Vediamo dire modi in cui si possono attuare collegamenti multicast:
- Multicast
	Il gruppo dei destinatari è identificato da un singolo indirizzo; quando arriva in un router questo si occuperà di duplicarlo e trasmetterlo sulle uscite corrette. In questo modo un collegamento non si può trovare più copie dello stesso pacchetto una dopo l’altra.
- Unicast multiplo
	La sorgente invia nella rete lo stesso pacchetto tante volte quante sono le destinazioni da raggiungere. In questo modo un collegamento si può trovare più copie dello stesso pacchetto una dopo l’altra.