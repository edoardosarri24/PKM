È una direttiva [pragma](compilazione.md#Pragma) per [openMP](openMP.md).
Permette di applicare la [task decomposition](decomposizione.md#Task%20decomposition) eseguendo un work-assignment, cioè assegnare una seqeunza di sitruzioni a un solo thread.
# Direttiva
Si deve usare una direttiva esterna $\#pragma\ omp\ sections$ e poi per ogni compito $\#pragma\ omp\ section$.
# Sincronizzazione
La direttiva loop definisce una [barriera](parallel%20computing/openMP/sincronizzazione.md#Barrier) di sincronizzazione implicita alla sua fine: quando il blocco termina tutti i thread devono sincronizzarsi secondo il [relaxed consistency model](memory%20consistency.md#Relaxed%20consistency).
Possiamo eliminare questa barriera implicita aggiungendo $nowait$ nella pragma; questo ha senso se le istruzioni successiva a $sections$ non dipendono dalla sua esecuzioni.