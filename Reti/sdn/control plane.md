Il control plane nell'architettura [sdn](sdn.md) è il controllore che si occupa di definire lo stato del [data plane](data%20plane.md), cioè di definire il protocollo usato per popolare le forwarding tables. A volte si può anche dividere in control plane e management plane: il primo impone la policy sui dispositivi di rete; il secondo, composto da umani, definisce la policy.
# Grafo di rete
Il NOS ([Network Operating System](#Network%20Operating%20System)) raccoglie tutte le informazioni provenienti dai dispositivi di rete e le rappresenta in un grafo di rete, che a questo punto rappresenta una centralizzazione delle informazioni di rete.
A questo punto il control plane può usare queste informazioni e usare un algoritmo (e.g., Dijkstra) per definire i percorsi con cui i pacchetti devono attraversare la rete per una data funzionalità (i.e., network virtualization).
##### Conseguenze
- Il controllo della rete passa dal definire un nuovo protocollo a definire una nuova funzione $control=f(graph)$ che da un grafo restituisca delle regole con cui i pacchetti devono essere trasferiti.
- Se la topologia cambia si aggiorna il $graph$ e la $f$ ricalcola tutto.
# Network Operating System
Il NOS (Network Operating System), chiamato anche controller, è un software che si occupa di fornire una vista centralizzata della rete alle applicazioni, le quali usano questa vista per instradare nel modo più efficiente possibile i pacchetti.
# Interfacce
Il control plane dialoga con il [data plane](data%20plane.md) e l'[application plane](sdn.md#Application%20plane) tramite diverse interfacce.
##### Sud
Permette la comunicazione e la riprogrammazione dei dispositivi di rete. Il più utilizzato è [openFlow](openFlow.md) insieme a [NETCONF](network%20configuration%20protocol.md).
##### Nord
Il control plane espone della API, dette northbound APIs, con cui i programmatori possono rilasciare applicazioni sulla rete.
Queste interfacce non sono standard, ma una delle più usate sono interfaccie REST.
##### Est/Ovest
Ci sono delle API, dette Horizontal APIs, che permettono la comunicazione (e.g., la sincronizzazione dello stato) tra gruppi di controller 


# CONTROLLER
Il compito del controllore è quello di fornire un'interfaccia uniforme e centralizzata dell'intera rete, in modo da permettere di controllare in modo efficiente e flessibile l'instradamento degli Switch.
### Interfacce
- Sud
	È l'interfaccia verso la rete.
	- Permette al controllore di ottenere le informazioni della rete fisica, cioè le Network Information Base (NIB); le informazioni comprendono i nodi, la topologia, le porte, gli host e i collegamenti e le statistiche.
	  Lo stato del sistema, cioè queste informazioni, è poi rappresentato in un grafo.
	- Permette al controller di installare le regole sui vari Switch.
- Nord
	È un'interfaccia di tipo [REST](REST.md) che permette ai livelli superiori di interfacciarsi con la rete.
	- Le applicazioni utilizzano il grafo, il NIB, per prendere decisioni e gestire la rete.
	- Essendoci un solo grafo questo sarà standard per tutte le applicazioni.
### Componenti
I [componenti](SDN%20controller%20component.png) del controllor possono essere separati in:
- Interni
	- Gestore dei protocolli, che si occupa dei classici protocolli internet e permette l'interazione con rete legacy.
	- Applicazioni, che usano il NIB e le librerie per programmare la rete.
	- Librerie, forniscono astrazioni per usare l'interfacce sud.
- Sopra il controller ci sono i tool per l'orchestratore, l'Operational Support System (OSS) e il Business Support System (BSS).
- Sotto il controller c'è l'interfaccia per parlare con gli Switch, come OpenFlow. Questa deve essere installata anche sugli Switch, per permettere di poter comunicare.
### Deployment
Logicamente il controllore è uno solo (la vista sulla rete è unica); fisicamente questo può essere composto da più processi che girano sullo stesso server o in server diversi. Le modalità con cui si può fare il deployment del controllore sono:
- Hot-standby
	C'è un controllore master e uno slave, dove il secondo entra in gioco solo se il primo ha deti problemi. Possono girare sia su server distribuiti che sullo stesso server.
	- È una soluzione semplice.
	- Il controllore è il collo di bottiglia rispetto al numero degli Switch, perchè da solo deve gestire tutto il carico di lavoro.
- Dstributed NIB
	Avere un solo controllore porta a problemi di scalabilità e fault tollerance. Per miglioare questi problemi si aggiunge un cluster di controllori, ognuno dei quali gestisce parti diverse ma sovrapposte della rete.
	- Siccome le informazioni di rete sono ridondate, se uno si rompe nessuna parte della rete rimane scoperta.
	- Usato quando ci sono molti Switch. Permette di aumentare infatti la disponibilità del sistema.
	- C'è un overhead dovuto alla comunicazione tra controller. Questa avviene tramite interfacce dette est e ovest.
- Hybrid mode
	È una combinazione delle due sopra: ci sono più master e più slave che controllano parti diverse.