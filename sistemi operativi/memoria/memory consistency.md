Nei sistemi [paralleli](parallel%20pragramming.md) i vari [thread](thread.md) accedono alle variabili globali in momenti diversi portandole nella propria cache e poi nei registri.
Durante l'esecuzione questo accesso alle variabili può avvenire in momenti diversi rispetto a quanto previsto nel codice perché il [compilazione](compilazione.md), e poi anche la CPU, può riordinare il codice in modo da ottimizzare i [cache miss](cache%20miss.md) e le invalidazioni della cache. Questo meccanismo di riordino ha dei vantaggi in performance, ma se i thread vedono l'aggiornamento in momenti diversi rispetto a quando gli serve allora il comportamento può essere non deterministico.
Un concistency model è un contratto tra il sistema e il programmatore che definisce quando le operazioni sulla memoria sanno visibili ai vari thread.
# Sequential consistency
Questo modello non prevede il riordino di nessuna istruzione: l'ordine delle istruzione è eseguito esattamente come il programmatore le scrive; tutte le dipendenze tra istruzioni sono mantenute.
##### Scrittura
La scrittura di variabili globali non deve essere vista subito dai processori per forza. Si garantisce però che tutti i thread la vedranno nello stesso momento.
##### Considerazioni
La consistenza sequenziale non è una buona soluzione, perché non permette l'efficiente compilazione da parte dei moderni compilatori che operano riordinando le istruzioni.
# Relaxed consistency
Il modello relaxed permeate al compilatore di ottimizzare parte del codice variando l'ordine delle istruzioni, ma questo viene fatto solo lontano dalle sincronizzazioni.
In prossimità delle [sincronizzazioni](sistemi%20operativi/thread/sincronizzazione.md) si garantisce che i thraed abbiano la stessa vista della memoria.
È il modello seguito da [openMP](openMP.md).
##### Motivazioni
Dare la stessa vista ai thread delle variabili globali solo in prossimità delle sincronizzazioni ha senso in un sistema parallelo che è implementato correttamente: si suppone infatti che i thread facciano uso soprattutto di variabili locali, sulle quali non c'è bisogno di nessun sincronismo e quindi che permettono al compilatore di eseguire tutte le ottimizzazioni che vuole.
##### Coerenza
Oltre alla consistenza, cioè cosa al compilatore è permesso riordinare, è fondamentale anche la coerenza, cioè quando i vari thread vedono il valore di una variabile.
Il fatto che le operazioni non siano riordinate non vuol dire che il valore aggiornato sia visibile da tutti i thread. Potrebbe capitare infatti che per eseguire delle ottimizzazioni il compilatore tenga una variabile nei registri invece che copiarla nella cache; in questo caso la variabile è visibile solo al core che la sta usando.
Per imporre questo bisogna usare dei costrutti di sincronizzazione; in OpenMP una soluzione è usare la pragma [flush](flush.md).
# Berstein condition
Le condizioni di Berstein sono un semplice test per valutare se più operazioni sono ottimizzabili dal compilatore, cioè se possono essere riordinate senza problemi.
In pratica stiamo guardando se ci sono [flow dependance](dipendenze.md#Flow%20dependance) del codice e se due processi sono eseguibili totalmente in parallelo.
##### Regola
Siano $I_i$ e $O_i$ l'insieme delle locazioni rispettivamente lette e scritte dal processo $i$-esimo.
Allora $P_i$ e $P_j$ possono essere eseguiti in parallelo se:
- $I_i\cap O_j=\emptyset$
- $I_j\cap O_i=\emptyset$
- $O_i\cap O_2=\emptyset$