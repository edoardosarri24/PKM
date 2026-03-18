È una direttiva [pragma](compilazione.md#Pragma) per [openMP](openMP.md) che permette di definire un punto dove i thread devono sincronizzarsi a livello di esecuzione.
Se tutti i thread non hanno raggiunto quel punto allora non si prosegue; non si fa un join, perché poi si continua in parallelo.
# Direttiva
La direttiva è $\#pragma\ omp\ barrier$.
# Implicita
Ci sono delle operazioni che definiscono delle barriere implicite.
##### Abolizione
Le barriere implicite possono essere eliminate aggiungendo $nowait$ alla pragma.
In questo modo i thread possono continuare anche subito: si deve stare attenti che le istruzioni successive non siano dipendenti dalla terminazione del blocco precedente.
##### Direttive
- Le direttive che la definiscono una barriere implicita sono: [parallel](parallel%20region.md), [for](loop.md), [single](esecuzione%20singola.md#Single) e [sections](sections.md).
- Non definsocno una barriera implicita invece [master](esecuzione%20singola.md#Master), [task](parallel%20computing/openMP/task.md) e le direttive di [sincronizzazione](parallel%20computing/openMP/sincronizzazione.md).