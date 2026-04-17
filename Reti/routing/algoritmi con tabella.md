Sono algoritmi di [routing](Reti/routing/routing.md).
In generale possono essere sia centralizzati che distribuiti e sia dinamici che statici; noi quelli con tabella, distribuiti e dinamici.
Il principio di ottimalità consiste nel trovare gli shortest path; un percorso può avere il costo minimo in base a più concetti, come la distanza dei collegamenti, l’ampiezza di banda o l’affidabilità.
# Distace vector
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
##### Soluzione
Per ovviare a questo problema ci sono tre soluzioni:
- Infinito finito
	Una volta che il costo di un collegamento raggiunge un certo limite superiore esso viene considerato come non disponibile. In questo modo si risolve il problema, ma ci vuole molto tempo per arrivare a questa situazione.
- Split horizon
	Prevede l’invio del distance vector solo ai nodi non direttamente collegati con il nodo destinazione aggiornato. Questo introduce un problema, derivante dal fatto che nell’algoritmo se un’informazione non viene aggiornata da molto tempo si considera non più attuale e quindi viene eliminata.
- Poisoned reverse
	È una variante dello split horizon: ai nodi direttamente collegati con il nodo destinazione aggiornato si invia comunque un’informazione, ma questa contiene un valore del costo molto alto; in questo modo il valore si aggiorna comunque, ma senza modificare le informazioni all’interno della tabella.
# RIP (Routing Information Protocol)
È una delle prime implementazioni concrete dell’algoritmo Distance Vector ed è ancora oggi utilizzata.
- La metrica di costo usata è il numero di salti eseguiti.
- I nodi si scambiano informazioni ogni 30 secondi o dopo ogni aggiornamento, attraverso messaggi detti RIP response che si possono riferire ad un massimo di 25 destinazioni. Ogni router può ricevere al massimo un RIP response contemporaneamente.
- Se un router non invia aggiornamenti da più di 180 secondi viene considerato irraggiungibile.
- Comprende un ulteriore tipo di messaggi con cui un router richiede l’invio delle informazioni a seguito della propria attivazione inziale.
- Il numero massimo di salti per cui si considera un nodo non raggiungibile è 15 hop. Questo limita l’utilizzo dell’algoritmo a reti non troppo estese.
##### Considerazioni
L’algoritmo ha le seguenti caratteristiche:
- La convergenza è relativamente veloce per via della limitazione superiore del costo di collegamento.
- La robustezza dell’algoritmo è data dalle modalità di aggiornamento delle tabelle dei router.
# Link state
Con questo algoritmo, detto algoritmo di Dijkstra, si vuole trovare lo shortest path definendolo per costi crescenti. Assume che ogni nodo sia a conoscenza dell’intera mappa della rete.
##### Algoritmo
Basa la conoscenza globale della rete a partire dalla conoscenza dei collegamenti verso i nodi vicini.
Una volta venuto a conoscenza di queste singole informazioni, il nodo le condivide con il resto della rete in modalità [flooding selettivo](Reti/Routing.md#Flooding%20selettivo) attraverso gli LSP (Link State Packet).
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
# OSPF (Open Shortest Path First)
È il successore dell’algoritmo RIP. È aperto, cioè il suo codice è disponibile e usabile da tutti.
- La metrica di costo è astratta, cioè non accoppiato all’algoritmo. In questo modo si può quindi considerare sia il numero di salti che la larghezza di barda dei singoli collegamenti.
- I nodi si scambiano informazioni ogni 30 minuti o dopo ogni aggiornamento.
- Comprende un ulteriore tipo di messaggi, detto Hello, con cui un router richiede l’invio delle informazioni a seguito della propria attivazione.
-  Quando sono presenti più percorsi con lo stesso costo si può mantenere entrambi attivi e non sceglierne uno solo. In questo modo si gestisce meglio il carico di lavoro.
- Permette di gestire strutture gerarchiche.