L'hyper threading è l'implementazione di Intel della tecnica Siimultaneous Multi Threading (SMT), con cui un singolo core fisico può eseguire più thread in [parallelo](parallel%20pragramming.md).
# Architettura
Ogni core ha il suo sistema architetturale: una (o più) ALU, i registri, program counter, cache, bus, ecc.
Grazie al SMT il [sistema operativo](Operating%20System%20(OS).md) vede ogni core fisico come due core logici: la logigità dei core deriva dal fatto che la parte costosa non viene duplicata; si aggiunge solo un po' di memoria per gestire lo stato dei thread.
# Efficienza
L'hyper threading è fondamentale perché i processori sono più veloci della [memoria](memoria.md).
Siccome la [bandwidth](performance.md) delle memorie solitamente è il collo di bottiglia dei sistemi, un thread può aspettare i dati per un lasso di tempo non trascurabile dal punto di vista della rapidità della CPU. Avere la possibilità di eseguire un context switch molto rapido (molto più di quello dei [processi](processo.md)) permette di non attendere la latenza con cui arriva un dato senza far nulla.
# Limiti
L'hyper threading ha anche dei limiti:
- L'efficienza diminuisce tanto più quanto il thread in esecuzione non ha [cache miss](cache%20miss.md).