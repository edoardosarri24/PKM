Un thread, noto anche come light-weight processes, è un'unità di elaborazione all'interno di un [processo](processo.md).
# Memoria
I thread sono legati al processo che li genera e per questo lo spazio di indirizzamento in memoria è lo stesso.
##### Stack
Ogni thread dello stesso processo ha un proprio stack, che cresce ogni volta che viene chiamata una funzione. Il thread alloca in questo spazio le variabili locali usate durante l'esecuzione delle funzioni.
Lo stack privato esiste finché il thread è in vivo; quando termina allora la memoria per lo stack viene liberata.
##### Heap
I thread dello stesso processo condividono lo heap e quindi possono comunicare tramite variabili condivise.
Sono condivise ovviamente anche le variabili globale definite dal processo.
##### Program counter
Ogni thread ha il proprio program counter.
# Efficienza
I thread possono essere supportati a livello sw o hw; nel secondo caso (ormai comune) si permette un'efficienza molto maggiore.
- Il context switch tra thread è molto più veloce rispetto al context switch tra processi. Questo avviene soprattutto grazie all'[hyper threading](hyper%20threading.md).
- Creare un thread è molto veloce perché non si deve allocare spazio nuovo, ma andrà nello stesso spazio di indirizzamento degli altri.
- Ogni thread ha i propri registri e viene visto dall'OS come un core logico.
# Problemi
- [race condition](race%20condition.md)
- [false sharing](false%20sharing.md)