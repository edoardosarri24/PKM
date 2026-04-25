Il routing è il compito principale del livello di rete (i.e. livello 2 di [TCP/IP](TCP%20IP.md)) e il suo scopo è quello di decidere il percorso tra sorgente e destinazione.
# Distribuito
Il routing classico è un routing di tipo distribuito, dove l'informazione di ogni dispositivo di rete è solo locale.
- Packet switching
	I router decidono il prossimo nodo a cui trasmettere il pacchetto tramite un algoritmo. Tale algoritmo è definito all'interno del dispositivo di rete e non può essere modificato senza la sostituzione del dispositivo; al massimo può essere leggermente configurato tramite parametri.
- Distributed control
	La conoscenza della rete è distribuita e non centralizzata e quindi ogni router ha bisogno di tempo perché la propria tabella di conoscenza della rete converga.
# Protocolli
Una volta definito un [Autonomous System](Internet.md#Concetti%20base) (AS), ci sono due protocolli principali.
##### Interior Router Protocol
L'IRP è un protocollo  opera all'interno di un singolo AS e il suo obiettivo è quello di conoscere in dettaglio la topoloigia della rete per trovare il percorso ottimo.
Solitamente gli algoritmi di routing usati sono [distance vector](algoritmi%20con%20tabella.md#Distace%20vector), [link state](algoritmi%20con%20tabella.md#Link%20state) e [OSPF](algoritmi%20con%20tabella.md#OSPF%20(Open%20Shortest%20Path%20First)).
##### Exterior Router Protocol
Sono protocolli (e.g., [BGP](border%20gateway%20protocol.md)) che gestiscono l'instradamento tra diversi AS. Non devono sapere la topologia esatta, ma solo se una rete esterna è raggiungibile e da dove.
# Classificazione
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
# Limiti
Le normali [reti](Reti%20di%20telecomunicazioni.md) dove la gestione del routing è all'interno dei router stressi e si basa sulla sola destinazione ha dei grandi limiti.
##### Scenari
Se prima le reti trasmettevano solo il classico traffico tra pagine web, adesso ci sono molti più scenari:
- IoT
	Moltissimi dispositivi connessi in rete.
- Big data
	Gestione di data estremamente grandi che richiedono una bassa latenza e grandi capacità di storage.
- Cloud
	Si tratta di un modello di calcolo on-demand che garatisce l'accesso a un pool di risorse (i.e., rete, computing, storage) teoricamente infinito.
- Mobile
##### Problemi
Ci sono numerosi problemi a cui questi scenari diversi portano:
- Varietà di informazioni
	Non abbiamo dolo informazione testuali trasmesse, ma anche voce e video, dati piccoli per i dispositivi mobili.
- Complessità
	I pattern di traffico sono sempre più complessi: server diversi generano sia traffico orizzontale parlando tra loro, sia traffico verticale parlando con il clinet.
- Dinamicità
	Le reti oggi devono evolvere a una grande velocità: sono definiti nuovi protocolli; le sotto reti possono essere definite per un intervallo di tempo anche breve; ci possono essere reti che all'inizio hanno bisogno di poche risorse e poi devono scalare molto velocemente.