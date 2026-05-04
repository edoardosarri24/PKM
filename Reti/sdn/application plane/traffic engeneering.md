Il traffic engineering (TE) è un'applicazione dell'[application plane](application%20plane.md). Si occupa dell'analisi, la regolazione e la predizione del comportamento della rete per rispettare il SLA (Service Level Agreement), basandosi sui parametri di [QoS](Reti/misure.md#QoS) che esso definisce.
# PolicyCop
È un framework per la TE che serve a far rispettare in modo automatico i parametri di QoS: monitora la rete, rileva le violazioni delle policy e la riconfigura.
Il ciclo (i.e., non termina mai) di lavoro è il seguente:
- Il policy validator monitora la rete e rileva eventi che fanno deviare i parametri di QoS dai SLA.
- L'event manager decide se l'evento è gestibile automaticamente: in caso negativo lo invia a un manager.
- L'evento che può essere risolto automaticamente viene preso in carica dal policy enforcer. Questo utilizza il topology manager per decidere come riprogrammare la rete.
- Il policy enforcer utilizza il policy adaptation per la riprogrammazione.
# Infrastruttura Google
L'infrastruttura google non è altro che una grande applicazione che utilizza SDN.
##### Layer
I layer principali sono:
- Jupiter
	È la rete che collega i server in un data center, detto anche cluster.
- B4
	È la rete che collega i data center. Ci sono due dorsali: una per il traffico degli utenti; una per il traffico tra data center (200 Tbps).
- Andromeda
	È il framework NFV per la virtualizzazione delle funioni di rete.
##### Motivazioni
Prima di SDN Google usava euristiche che definivano il percorso in base al costo (e.g., distanza in hop). Questo comportava che c'erano pochi cavi dove passava quasi tutto il traffico: erano maggiormente soggetti a usura.
Adesso tramite il TE, l'obiettivo è quello di avere un utilizzo molto più equo dei cavi: in pratica Google definisce i suoi SLA per il TE come il fattore di utilizzo dei cavi e l'obiettivo è averlo uguale su tutti questi. Questo permette inoltre di inviare uno stesso flusso in modo partizionato (i.e., partizionando i pacchetti di un flusso) tramite percorsi diversi.
Inoltre questo è stato un motivo per cambiare fornitore di Switch: prima prendeva i router da Cisco e altri; adesso se li produce in casa e sono solo Switch SDN.
##### Pipeline
La pipeline per l'implementazione è la seuqente:
- Ogni cluster ha circa 100K nodi. questi sono gestiti da switch SDN.
- Gli switch del cluster sono partizionati e ogni partizione è assegnata a un controller. Siamo quindi in un contesto di [control plane distribuito](control%20plane%20distribuito.md), dove si utilizza un [algoritmo del consenso](legge%20del%20consenso.md) (e.g., Paxos) per avere la stessa informazione.
- Per quanto riguarda la comunicazione Google fa all'opposto: l'interfaccia eat/west è wrappata e implementata tramite chiamate [REST](REST.md); l'interfaccia nord è implementata con [BGP](border%20gateway%20protocol.md).
- L'applicazione di TE gira in un server; questa viene raggiunta passando da un gateway, che ha il compito di parlare con un controller per cluster (tanto l'informazione è condivisa) e inviare i dati al server.
- A questo punto il server TE ha tutte le informazioni per prendere le decisioni.