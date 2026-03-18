L'Address Resolution Procol (ARP) è un protocollo di servizio appartenente a [IPv4](IPv4.md) che opera a livello MAC (livello più basso di [TCP/IP](TCP%20IP.md)). Si occupa della mappatura tra l'indirizzo IP e l'indirizzo MAC (48 bit) in una link local di una rete che permette la comunicazione broadcast (es: ethernet).
### Scopo
Per inviare un pacchetto a un dispositivo nella stessa link local si usano i pacchetti di livello data link (per capirsi MAC, quello più basso di TCP/IP) che contiene l'indirizzo MAC. Il protocollo ARP è usato per ottenere questo indirizzo dall'indirizzo IP pubblico.
Se invece il pacchetto viene mandato all'interno di un'altra sotto rete allora ARP viene usato per scoprire l'indirizzo del router che collega la link local.
### Cache
Il protocollo ARP ha una cache (ARP cache) che evita di dover chiedere ogni volta l'indirizzo MAC del destinatario, il che provocherebbe un notevole ritardo.
Le informazioni che non sono più usate da (solitamente) 5 minuti vengono eliminate.
### Funzionamento
L'host che vuole l'indirizzo MAC di un altro host di cui conosce l'IP, invia in broadcast un pacchetto ARP request contenente il proprio indirizzo MAC e l'IP dell'host che vuole indagare. Il pacchetto sarà ricevuto da tutti gli host che appartengono alla link local; l'host indagato, che riconoscerà il proprio IP, invierà in unicast una pacchetto ARP reply contente il proprio indirizzo MAC.
Lo scambio di pacchetti ARP avviene a livello 2 di TCP/IP, cioè a livello datalink.