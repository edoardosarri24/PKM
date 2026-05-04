L'application plane in [sdn](sdn.md) contiene le applicazioni che definiscono, monitorano e controllano le risorse di rete del [data plane](data%20plane.md) tramite il [control plane](control%20plane.md).
# Network Services Abstraction Layer
La comunicazione tra l'application layer e il control layer è data dall'[interfaccia nord](control%20plane.md#Nord). Tale interfaccia può essere locale o remota: nel primo caso contro e application plane sono sulla stess macchian; nel secondo sono su macchine diverse.
Il Network Services Abstraction Layer serve per unifica l'interfaccia nord ed esporla in modo uniforme all'application plane.
##### Frenetic
Si tratta di un query language che implementa il NSAL. La sua caratteristica è di mostrare la rete non più come un grafo, ma come un database; in questo modo le applicazioni posso fare query tramite questo linguaggio senza ragionare sulla topologia.
Per l'interrogazione delle rate, i flussi possono essere visti da tre prospettive: lato sorgente per capire cosa parte; lato destinazione per capire cosa arriva; direttamente negli switch per capire cosa succede durante il routing.
Per la scrittura sulla rete, cioè la definizione di azioni per determinati tipi di traffico, abbiamo funzioni come l'inoltro verso una data porta, l'inoltro a tutte le porte, l'invio di pacchetti al controllore e la modifica dell'header di un pacchetto.