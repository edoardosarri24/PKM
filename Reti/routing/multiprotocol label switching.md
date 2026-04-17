Il Multiprotocol Label Switching (MPLS) è un algoritmo di [routing](Reti/routing/routing.md) introdotto per migliorare la velocità di instradamento dei pacchetti. Il principio su cui si basa è stato mutuato dalle rete [frame relay](Reti%20geografiche.md#FRAME%20RELAY) e [ATM](Reti%20geografiche.md#ATM), cioè si basa sulla commutazione di etichetta.
# Traffic engineering
MPLS introduce quello che è il traffic engineering: creando una pila di etichette con ordine LIFO si possono accorpare più flussi di dati diversi e trattarli come uno singolo.
Ai flussi che devono seguire lo stesso percorso, LSP (Label Switched Path) viene associata la stessa etichetta; una volta assegnata questa etichetta il pacchetto viene inoltrato su una porta di uscita; all’arrivo nel router di destinazione si rimuove l’etichetta e la si sostituisce con la prossima oppure si segue il percorso definito dall’etichetta successiva della pila.
Questo è ottimo in un contento di VPN (VirtualPrivate Network), con cui si possono nascere i singoli indirizzi IP sotto una pila serie di etichette.
# Etichetta
Il protocollo MPLS non fa altro che aggiungere un’etichetta a 32 bit nell’header IP. I suoi campi sono:
- Label
	Identifica l’etichetta ed ha significato su base L2L. I valori di questo campo vengono decisi dai vari router.
- Exp
	È stato definito come sperimentale. Viene usato ad esempio per definire il tipo di servizio trasmesso.
- S
	È un campo formato da un siglo bit. Se posto a 1 significa che l’etichetta corrente è l’ultima della pila, altrimenti ce ne sono altre.
- TTL
	È il time Time To Live e ha lo stesso significato dell’omonimo campo dell’header IP.