Le direttive [pragma](compilazione.md#Pragma)  di [openMP](openMP.md) di questo tipo definiscono costrutti per la [sincronizzazione](sistemi%20operativi/thread/sincronizzazione.md) tra thread, cioè permettono di stabilire punti in cui i thread devono essere allineati sulla loro esecuzione.
Ricordiamo che al fine delle performance la sincronizzazione fa [perdere prestazioni](performace.md#Downgrade). Inoltre più la sincronizzazione è lontano dall'hw più è complessa da gestire: si deve scegliere atomic quando possibile.
##### Coerenza
Quando gestiamo una variabile come una critical section, cioè vogliamo che un solo thread alla volta agisca su di essa, dobbiamo essere coerenti con la scelta del costrutto: si deve scegliere quello che più fa al caso nostro e usarlo in tutte le situazioni.
Questo perché l'uso di atomic può bloccare un critical: se siamo dentro critical e un thread arriva ad un atomic a guardia della stessa variabile allora quest'ultimo thread può acquisire l'uso della variabile.
# Atomic
Con $\#pragma\ omp\ atomic [read|write|update|capture]$ si richiede che un'istruzione (o un blocco) venga eseguita in modo atomico.
È implementata via hw e non via mutex come [critical](parallel%20computing/openMP/sincronizzazione.md#Critical).
##### Parametri
I parametri nelle parantesi quadre sono opzionali; senza una specifica allora viene scelto $update$.
##### Operazioni
Le operazioni che possono essere dichiarate come atomiche sono quelle che l'hw può gestire in modo [atomico](sistemi%20operativi/thread/sincronizzazione.md#Atomicità):
- Lettura
	La sola operazione di lettura di $x$ in $var=x$.
- Scrittura
	La scrittura in $x$ come $x=expr$.
- Update
	Rappresenta una modifica di una variabile. Sono esempi $x++$, $++x$, $x\ op= espr$, dove $op$ sono le operazioni classiche e semplici.
- Capture
	Rappresenta una lettura e un aggiornamento di una variabile.
	Tali operazioni devono essere scritte in un blocco (parantesi graffe).
# Critical
Con $\#pragma\ omp\ critical\ [name]$ si richiede che un'istruzione (o un blocco) venga eseguita da un solo thread per volta, stiamo quindi definendo una critical section.
È implementata con un mutex e non via hw come [atomic](parallel%20computing/openMP/sincronizzazione.md#Atomic); se è possibile è meglio scegliere atomic, che introduce meno overhead.
##### Critical section nominale
Il nome della direttiva permette di eseguire più critical section in parallelo; senza un nome abbiamo una sola critical section all'interno del programma.
##### Nested
Ci possono essere situazioni in cui usiamo critical section nested; ad esempio una funzione chiamata dentro una critical section nel cui corpo (della funzione) viene chiamata la stessa critical section.
Questo porta a un deadlock. Per evitarlo si deve garantire un ordine di esecuzione delle critical section; questo si fa definendo dei nomi e avendo critical section diverse.
# Lock
A volte è utile acquisire un lock in una porzione di codice (e.g., in una funzine) e rilasciarlo in un altra. In queste situazioni pragma atomic e critical di omp non ci aiutano, ma dobbiamo definire un lock esplicito.