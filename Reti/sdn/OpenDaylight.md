OpenDaylight (ODL) è un'implementazione Java open source del [control plane](control%20plane.md), ma possiede anche funzionalità dell'[application plane](application%20plane.md).
Permette sia di implementare una versione a singolo controller, che una versione [distribuita](control%20plane%20distribuito.md).
# Architettura
Offrendo funzionalità di controller e per l'application layer, ha un'interfaccia molto ampia.
##### APis
Sono le API con cui l'[application plane](application%20plane.md) si può collegare alle funzioni del controller.
Permette sia di utilizzare il protocollo [REST](REST.md) se l'application layer gira su un altra macchina, sia OSG se controller e application layer sono sulla stessa macchina.
##### Service Abstraction Layer
Il SAL è l'elemento che offre l'astrazione del [data plane](data%20plane.md), permettendo alle funzionalità di control plane di operare senza conoscere l'implementare dell'[interfaccia sud](control%20plane.md#Sud).
Quello che fa è mappare una richiesta di servizio (e.g., crea un flusso) in messaggio che utilizza il protocollo usato dallo switch verso cui è diretto il messaggio.
# Base Network Functions
ODL include delle funzionalità base con cui gestire la rete:
- Topology Manager
	Si occupa dell'apprendimento e manutenzione del [NIB](control%20plane.md#Grafo%20di%20rete).
- Switch Manager
	Mantiene persistenti le informazioni degli switch: modello, id, capacità.
- Forwarding Rules Manager
	Si occupa di usare il NIB per installare le rotte e i slussi collaborando con il Topology e Switch Manager.
- Statistics Manager
	Raccoglie informazioni sulle performance: stastiche sui flussi e code degli switch.
# East-West interface
Per la comunicazione tra controller nel caso distributivo, ODL utilizza il protocollo [SDNi](border%20gateway%20protocol.md#SDNi) con una particolarità. Nonostante sia unìinterfaccia east/west, si utilizza un SDNi wrapper: la comunicazione che teoricamente dovrebbe avvenire con il protocollo BGP avviene invece tramite chiamate REST, come se fosse un'interfaccia nord.