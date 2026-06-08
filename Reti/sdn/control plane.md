Il control plane nell'architettura [sdn](sdn.md) è il controllore che si occupa di fornire un'astrazione del [data plane](data%20plane.md) all'[application plane](application%20plane.md), come farebbe un OS (si parla infatti di Network Operating System): astrae il tipo di dispositivo di rete e la topologiae; permette di instradare i flussi delle applicazioni senza conoscere i dettagli della rete sottostante; fornisce un'interfaccia centralizzata della rete anche se ci sono [più controller](control%20plane%20distribuito.md) distribuiti.
A volte si può anche dividere in control plane e management plane: il primo impone la policy sui dispositivi di rete; il secondo, composto da umani, definisce la policy.
# Interfacce
Il control plane dialoga con il [data plane](data%20plane.md) e l'[application plane](sdn.md#Application%20plane) tramite diverse interfacce.
##### Nord
L'interfaccia nord è il punto di contatto tra control plane e [application plane](application%20plane.md). Non esistono protocolli standard come per la sud, ma solitamente il control plane espone delle API [REST](REST.md).
Ci sono diversi livelli di granularità che tali API pososno esporre: il livello più basso tocca le primitive del controller con cui gli sviluppatori possono costruire i servizi di rete di base; il livello applicativo può usare servizi di rete già pronti (e.g., routing o [QoS](Reti/misure.md#QoS)); usare non solo servizi di rete ma altre applicazioni.
##### Sud
Permette la comunicazione e la riprogrammazione dei dispositivi di rete. Utilizza il protocollo TLS su TCP.
Come prima cosa per creare l'interfaccia si deve usare [NETCONF](network%20configuration%20protocol.md); dopo che il dispositivo conosce l'IP del controllore allora si deve [creare il NIB](#Link%20discovery). Dopo tutto si usa [OpenFlow](OpenFlow.md) per creare le regole.
##### East/West
Si tratta di un canale di comunicazione tra [più controller](control%20plane%20distribuito.md). Per far collaborare due domini diversi si utilizza il [SDNi](border%20gateway%20protocol.md#SDNi); per parlare la prima volta devono conoscere l'indirizzo IP l'uno dell'altro (non si può fare automaticamente questo).
Una delle due interfacce (e.g., east) è delegata alla comunicazione tra controller di [pari livello](control%20plane%20distribuito.md#Un%20livello) mentre l'altra (e.g., west) è per la comunicazione tra [livelli adiacenti](control%20plane%20distribuito.md#Due%20livelli).
# Implementazioni
Le implementazioni del control plane possono essere sia proprietarie che open.
- [OpenDaylight](OpenDaylight.md)
- [ONOS](ONOS.md)
- POX
	È open source è ha una documentazione e GUI molto bene fatta.
- Floodlight
	Controller open source.
- Onix
	Controller proprietario di Google.
# Grafo di rete
Per implementare il [routing](routing.md) in modo centralizzato il controller ha una visione del grafo di rete completa, detto NIB (Network Information Base).
##### Link discovery
Per scoprire i nodi della rete e creare il NIB, il controller usa il LLDP (Link Layer Discovery Protocol), un protocollo di livello 2 dove richiede pacchetti LLDP (0x88cc EtherType) agli switch per fornirgli informazioni su nome e le capacità.
Dopo che uno switch è stato configurato con [NETCONF](network%20configuration%20protocol.md) ha una configurazione iniziale definita da: l'indirizzo del controller; la regola OpenFlow per cui se lo switch ricevo un pacchetto LLDP lo deve mandare al controller tramite un $Packet-In$.
Quando lo switch si connette cerca di avviare una comunicazione TLS con il controller: con un FEATURE REQUEST MESSAGE il controller chiede identità e capacità del dispositivo; con un FEATURE RESPONSE MESSAGE lo switch risponde con il suo ID e le sue porte attive. Dopo l'handshake il controller sa esattamente quali dispositivi ci sono, le loro porte e quali porte connettono quali switch.
A questo punto invia un pacchetto LLDP tramite un messaggio $Packet{-Out}$ allo switch $A$ che lo invia alla rete tramite un multicast; lo switch $B$ che riceve il pacchetto lo deve inviare al controller con un messaggio $Packet-In$ (per via della regola iniziale) arricchendolo con i metadati della sua provenienza; a questo punto i controller ha tutte le informazioni per creare il NIB.
Per funzionare il numero di pacchetti necessari è $\sum_{i=i}^SP_{i}$, dove: $P_{i}$ è numero il numero di porte dello switch $i$-esimo e $S$ è il numero di switch. In pratica ogni router deve inviare un messaggio da ogni sua porta.
##### Topology manager
Una volta che abbiamo il NIB e lo manteniamo in un database, possiamo usare un algoritmo come Dijkstra per calcolare il percorso minimo tra due nodi per decidere come instradare i pacchetti.
In questo modo abbiamo due conseguenze:
- Il controllo è definito da una funzione $control=f(graph)$ e non da un algoritmo di routing.
- Se la topologia cambia, si aggiorna il $graph$ e la $f$ ricalcola tutto.