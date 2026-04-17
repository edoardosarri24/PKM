Il routing è il compito principale del livello di rete (i.e. livello 2 di [TCP/IP](TCP%20IP.md)) e il suo scopo è quello di decidere il percorso tra sorgente e destinazione. Deve prendere una decisione logica su dove mandare il pacchetto e poi occuparsi del data forwarding.
# Protocolli
Una volta definito un Autonomous System (AS), ci sono due protocolli pricipali. Esso è un'organizzazione o un dominio i cui router condividono il prefisso dell'indirizzo IP.
##### Interior Router Protocol
L'IRP opera all'interno di un singolo AS. Solitamente gli algoritmi utilizzati sono [distance vector](algoritmi%20con%20tabella.md#Distace%20vector) e [link state](algoritmi%20con%20tabella.md#Link%20state).
##### Exterior Router Protocol
Gestiscono l'instradamento tra diversi AS.
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