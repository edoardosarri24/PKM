Per far comunicare reti [LAN](IEEE%20802%20-%20LAN.md) distinte tra loro o con la rete globale si usano dei dispositivi di interconnessione, che agiscono su livelli diversi della pila protocollare [ISO/OSI](ISO%20OSI.md).
## HUB PASSIVI
Operano sotto il livello [fisico](ISO%20OSI.md#LIVELLI), cioè direttamente col mezzo fisico.
Sono in pratica dei connettori che permettono la continuità del segnale; aumentano la portata delle rete senza fornire nessuna elaborazione del segnale.
## HUB ATTIVI
Sono detti anche ripetitori e operano sul livello [fisico](ISO%20OSI.md#LIVELLI). Sono in pratica dei ripetitori multi-porta: ricevono un segnale in ingresso e lo trasmettono su ogni uscita possibile.
Questo comporta che se più reti distinte sono collegate tramite un hub attivo queste si comportano come se fossero una singola rete; si perde così tutti i vantaggi di avere reti [LAN](IEEE%20802%20-%20LAN.md) divise. Questo problema è risolto dai bridge.
## BRIDGE
Operano sul livello di [collegamento](ISO%20OSI.md#LIVELLI) e quindi possono prendere decisioni basate sull’indirizzo del mittente/destinatario.
Quando arriva un pacchetto il bridge consulta una tabella e decide su quale porta di uscita inoltrarlo; la tabella viene configurata in modo dinamico, associando inizialmente l'indirizzo del mittente alla porta che esso ha usato per comunicare col bridge.
I bridge operano su base [Store&Forward](Reti%20di%20telecomunicazioni.md#Store&Forward).
Se un bridge si danneggia le [LAN](IEEE%20802%20-%20LAN.md) collegate ad esso si scollegano; per risolvere questo problema si creano dei percorsi ridondanti. Questo porta però alla possibilità che i pacchetti entrino in percorsi ciclici; la soluzione è data dallo spanning tree: i bridge hanno un albero senza cicli sulla base del quale decidono quale percorso far seguire ad un pacchetto che deve arrivare ad una certa destinazione.
# ROUTER
- Operano sul livello di rete dello standard [ISO/OSI](ISO%20OSI.md) e sul livello IP del protocollo [TCP/IP](TCP%20IP.md).
- Il loro computo è il [Routing](Routing.md), ovvero decidere quale percorso prenderà il pacchetto, e il forwarding, cioè l'inoltro del pacchetto lungo il percorso scelto.
# GATEWAY
- Agisce come ponte tra reti che utilizzano protocolli diversi. Opera sia sul livello di rete che sui livelli superiori di [ISO/OSI](ISO%20OSI.md) e IP di [TCP/IP](TCP%20IP.md).
- Le operazioni principali sono quelle di traduzione del protocollo e controllo e filtraggio di pacchetti.
- In generale si parla di gatway quando il dispositivo può modificare i pacchetti con cui ineragisce.
- Esempi sono la traduzione tipica dei router [NAT](IPv4.md#NAT), i firewall (filtraggio di pacchetti che non devono passare) e proxy.