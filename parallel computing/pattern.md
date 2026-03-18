In programmazione [parallela](parallel%20pragramming.md) ci sono pattern architetturali (i.e., di alto livello) che possiamo usare in sfide che sono frequenti.
# Superscalar sequence
Usando un data flow dependence si può capire come la dipendenza dei task rispetto ai dati, cioè quando un dato necessario a un task per eseguire sarà pronto.
In questo modo possiamo sapere quali task sono indipendenti dai dati ed eseguirli in parallelo.
# Loop level parallelism
È un pattern che esplicità la [data decomposition](decomposizione.md#Data%20decomposition): dato un ciclo, possiamo assegnare un chunk di indici del ciclo a un thread ed eseguire più thread in parallelo.
È fondamentale che le iterazioni di un loop siano indipendenti.
# Task parallelism
È un pattern che esplicita la [task decomposition](decomposizione.md#Task%20decomposition): si divide un lavoro più grande in task, che vengono assegnati a core della stessa CPU o di CPU diverse; può quindi essere valido sia in sistemi [shared memory](tipi%20di%20parallelismo.md#Shared%20memory) che [distributed memory](tipi%20di%20parallelismo.md#Distributed%20memory).
Gli algoritmi che girano sui diversi core possono essere lo stesso oppure diversi.
# Fork-Join parallelism
Il mail [thread](thread.md) fa una fork e il flusso continua con $n$ thread che eseguono in parallelo; il mail thread riparte (join) quando tutti i fork hanno completato il loro lavoro (in un qualunque ordine).
La sincronizzazione viene implementata con un meccanismo che si chiama barrier: i thread devono aspettare che tutti i thread raggiungano quel punto.
È la tecnica base quando si creano compiti o task in un OS.
# SPMD
È il pattern che esplicita la tassonomie di [Flynn's Taxonomy](parallel%20pragramming.md#Flynn's%20Taxonomy)Singole Process Multiple Data: si esegue lo stesso codice su chunk di dati diversi; quando tutti i thread hanno finito eventualmente si può raccogliere i dati insieme.
Tutti i thread sono sulle stesso livello, non c'è solitamente un master.
# MPMD
È il pattern, noto anche come clinet-server, che esplicita la tassonomie di [Flynn's Taxonomy](parallel%20pragramming.md#Flynn's%20Taxonomy)Multiple Process Multiple Data: si eseguono codici diversi su chunk di dati diversi; quando tutti i thread hanno finito eventualmente si può raccogliere i dati insieme.
Sono i clinet in questo caso che richiedono lavoro al server: c'è una comunicazione tra client e server. Questa logica si contrappone al [master-worker](parallel%20computing/pattern.md#Master-worker).
# Master-worker
È ottimo quando abbiamo più compiti indipendenti da svolgere: il master inserisce in una coda i vari compiti; i worker li prelevano e li eseguono in parallelo senza comunicare.
# Pipeline
Il concetto è lo stesso della pipeline con cui le CPU eseguono le varie istruzioni (i.e., featch, decode, ...): quando il thread $i$ deve lavorare con i dati restituiti dal thrad $i-1$, e questi sono indipendenti, allora $i$ può elaborare il batch di dati $j$-esimo mentre $i-1$ elabora il batch $j-1$.
# Map
È usato per trasformare un dato complesso per la prossima esecuzione; si può vedere come la stessa cosa del pattern [loop level parallelism](parallel%20computing/pattern.md#Loop%20level%20parallelism) ma a un livello di astrazione maggiore.
Solitamente un dato complesso viene decomposto nei sui elementi e su questi viene eseguito il compito.
Un esempio è l'operazione SAXPY (i.e., scaled vector addiction).
# Reduction
Si combina ogni elemento di una collezione con un'operazione associativa; siccome l'associazione è indipendente dall'ordine con cui si esegue, allora non avere un ordine di esecuzione definito (vista la parallelizzazione) ci va bene.
Esempi sono il calcolo del massimo, minimo, somme, moltiplicazioni, operazioni booleane.
# Scan
Viene eseguito quando si ha una collezione di dati e vogliamo restituire una collezione di dati: l'indice $i$-esimo in output è il risultato delle operazioni fino all'indice $i$-esimo sull'input.
# Stencil
Si eseguono delle funzioni elementari sui vicini dell'elemento considerato.
È importante suddividere bene i dati; possibilmente in regioni non sovrapposte.
Un esempio, se i vicini sono tutti i dati in una quadrato, è la [convoluzione](machine%20laerning/Deep%20learning/CNN/Convoluzione.md).
# Recurrence
Si occupa di lavorare con cicli annidati. Se guardiamo i loop come una matrice dove gli input sono in alto e a sinistra e gli output viceversa, allora sembra non parallelizzabile; se però giriamo la matrice di 45° allora vediamo che le dipendenze non sono così grandi e che possiamo ridurlo.