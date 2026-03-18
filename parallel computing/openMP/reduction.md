È una direttiva [pragma](compilazione.md#Pragma) per [openMP](openMP.md) che serve per garantire una [sincronizzazione](sistemi%20operativi/thread/sincronizzazione.md) tra thread con operazioni associative.
Implement il pattern [reduction](parallel%20computing/pattern.md#Reduction): quando un ciclo lavora su un'operazione associativa si dovrebbe sincronizzarci sulla variabile dove l'operazione viene accumulata; in questo modo ogni thread lavora su una porzione dei dati e poi si mette inseme le varie parti.
# Direttiva
La direttiva è $\#pragma\ omp\ reduction(op:listOfVar)$, dove $op$ può essere uan qualunque delle operazioni che godono della proprietà associativa (i.e., $+$, $*$, $-$, $\$$, ^, $|$, $\$\$$, $||$).
Può essere unita nella direttiva della [parallelizzazione](parallel%20region.md).
# Vantaggi
Permette di non pagare tutto il costo di una sincronizzazione su una sola variabile globale.
# Implementazione
A livello implementativo omp crea una variabile locale dove il thread lavora in modo che la sua parte possa essere eseguita in parallelo.
Si occupa poi di sincronizzare i thread nella variabile globale dove si finalizza l'operazione.