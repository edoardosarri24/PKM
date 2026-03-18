Mentre le CPU sono ottimizzate per la computazione parallela, le GPU nascono per un'esecuzione parallela di operazioni in floating point.
Questo è possibile grazie ad un cambio di paradigma nell'[architettura](architettura.md) delle GPU.
# Heterogeneus architecture
Stiamo andando verso sistemi eterogenei, che hanno al loro interno sia CPU che GPU.
Supponendo che la CPU esegua anche dei calcoli, possiamo vederle però come dei gestori general-purpose di acceleratori: la CPU coordina il sistema, mentre un acceleratore è un device specializzato per un qualche compito; le GPU sono device specializzati per eseguire operazioni in floating point in modo imbarazzamente parallelo.
##### Host vs device
Le CPU sono parte del sistema host, cioè appunto del coordinatore; il codice che esegue sulle CPU è detto host code.
Le GPU, e in generale gli acceleratori, sono detti device; il codice che gira sui device è detto device code o kernel.
##### CPI bus
In un sistema eterogeneo l'host deve spostare i dati dalla CPU alla GPU e recuperare i risultati. Questi spostamenti di dati avvengono tramite il Peripheral Component Interconnect (PCI) bus; spesso questo bus è il [bottleneck](bandwidth.md#Bottleneck) del sistema.
# Compute capabilities
Le prestazioni di una GPU NVIDIA sono racchiuse dentro le compute cababilities, dove un valore più alto indica prestazioni migliori.
Si basano su aspetti [architetturali](architettura.md) come il numero e tipo dei core, se ci sono e che performance hanno le SFU e le dimensioni delle memorie.
# Compilazione
Il parallelismo nelle CPU è definito nel momento della compilazione: il compilatore genera [istruzioni vettoriali](vettorializzazione.md) che poi sono eseguite da [thread](thread.md) diversi su core diversi. Abbiamo quindi che il numero di thread e quindi il grado di parallelismo (la dimensione del chunk associata a ogni thread) è definito in fase di compilazione; se cambia l'hw allora dobbiamo ricompilare il codice e il compilatore deve definire chunk diversi rispetto a prima.
Nelle GPU invece la parallelizzazione è definita a livello hw: una volta che abbiamo compilato il codice è l'hw che si occupa di assegnarlo ai thread ed eseguirlo sui core; se cambiamo e usiamo un hw più potente (con più [streaming multiprocessor](architettura.md#Streaming%20multiprocessor) e/o più core per SM) allora il codice rimane lo stesso ma avremo prestazioni migliori.