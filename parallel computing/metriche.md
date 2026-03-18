Quando passiamo da un algoritmo sequenziale a uno [parallelo](parallel%20pragramming.md) si devono prendere delle misure prima e dopo il cambiamento per valutare i miglioramenti.
Se facciamo esperimenti su un algoritmo, non dobbiamo variare solo il numero di processori che possiamo usare per eseguire il calcolo, ma anche i parametri dell'algoritmo. Ad esempio se paralleliziamo su pochi dati allora le migliorie non si vedranno, ma se il numero di dati aumenta allora si ha un a performance migliore.
##### Timing
Per valutare tutte le metriche successive ci serve il tempo che ci mette un programma a eseguire.
Per ottenerlo abbiamo varie opzioni, ognuna con le proprie conseguenze. Consideriamo quelle della libreria $\texttt{time.h}$:
- $clock()$
	Restituisce il numero di clock (della cpu) in cui il processo (tutti i thread) ha occupato la cpu: non include attese; serve per calcolare il tempo in cui la cpu è stata occupata.
	Ha senso in programmi sequenziali. In programmi paralleli se abbiamo $n$ thread che eseguono contemporaneamente per 1 clock, allora $clock()$ restituirà 4, ma il tempo che a noi serve è 1.
	La precisione massima che si può avere è in millisecondo facendo $\texttt{double seconds = (double)(end - start) / CLOCKS\_PER\_SEC}$.
- wall-clock time
	Un wall-clock time è un orologio che è indipendente dai thread o dal processo; è quello che ci serve per capire il tempo che ci ha messo una funzione a eseguire.
	Una sua possibile implementazione in C è $clock\_gettime$, che ha una precisione del nanosecondo. Se siamo in C++ allora abbiamo $std::chrono$ with $::high\_resolution\_clock$. C'è anche una versione [openMP](openMP.md) (i.e., $omp\_get\_wtime()$) che permette indipendenza del codice.
# Service time
Si può definire in due modi:
- Il throughput medio sullo stream.
- L'inverso della larghezza di banda.
##### Sequenziale
Se un modulo sequenziale $\Sigma$ implementa $k$ operazioni, ognuna delle quali ha un service time $t_i$ e probabilità $p_i$, allora il service time medio di $\Sigma$ sarà $t=\sum_{i=1}^kp_it_i$.
Nel caso sequenziale il service time è uguale alla latenza.
##### Parallelo
Se un sistema parallelo $\Sigma^n$, dove $n$ è il grado di parallelismo, cioè il numero di processori (moduli) disponibili nel sistema, il service time è l'intervallo di tempo medio tra l'inizio di due esecuzioni su elementi consecutivi dello stream; si può nche dire che è il tempo medio con cui il sistema accetta un nuovo input dello stream.
# Completion time
È il tempo per eseguire il calcolo su tutti gli elementi dello stream.
Per elaborazioni su singoli dati è la metrica migliore; per stream è meglio la latency.
In sistemi paralleli il competion time divnta più importante della latenza.
##### Sequenziale
Se un modulo parallelo $\Sigma$ implementa $k$ operazioni e il service time è $t$ allora il tempo di completamento è $t_c=mt$, dove $m$ è la lunghezza dello stream.
##### Parallelo
Se un sistema parallelo $\Sigma^n$, dove $n$ è il grado di parallelismo, cioè il numero di processori (moduli) disponibili nel sistema, il completion time $t_c^n$ è il tempo in cui tutti gli elementi dello stream vengono processati; per $m>>n$ (e solitamente è così) abbiamo $t_c^n=mt^n$.
# Latency
È il tempo medio per eseguire i calcolo su un elemento dello stream.
Per stream di dati è la metrica più usata; per singola dati è meglio il competion time.
In sistema paralleli la latenza è meno importante del completion time.
##### Sequenziale
Nel caso sequenziale la latenza è uguale al service time.
##### Parallelo
Se un sistema parallelo $\Sigma^n$, dove $n$ è il grado di parallelismo, cioè il numero di processori (moduli) disponibili nel sistema, la latenza è il tempo medio necessario a $\Sigma^n$ per processare un singolo elemento dello stream.
# Scalability
La scalabilità è una misura che esprime la velocità di un sistema parallelo con $n$ moduli (processori) rispetto alla stessa operazione svolta con un singolo processori (i.e., $\Sigma^1$).
Si definisce come $S_{\Sigma^n}=\tfrac{B^n}{B^1}=\tfrac{t^1}{t^n}$, cioè il rapporto tra il tempo del codice parallelo eseguito su un processore sul tempo dello stesso codice eseguito con $n$ processori,
##### Conseguenze
- È una buona metrica per valutare se abbiamo fatto un buon lavoro nella parallelizzazione.
- Non va bene per paragonare sistemi paralleli.
- Non è facile da calcolare perché spesso è difficile dire al sistema esegui il codice parallelo usando un solo core. Ad esempio in CUDA non si può fare. Solitamente si usa lo [Speedup](parallel%20computing/metriche.md#Speedup).
# Speedup
È un concetto più debole della [scalabilità](parallel%20computing/metriche.md#Scalability), ma sempre calcolabile.
Si definisce come $S_P=\tfrac{t_s}{t_P}$, dove $P$ è il numero di processori: il rapporto tra il tempo di esecuzione del codice sequenziale e quello parallelo, entrambi nei rispettivi sistemi.
##### Bontà
I motivi per cui si può arrivare solitamente a uno speedup lineare sono che in alcuni momenti il processore può essere idle (magari ha finito prima degli altri) e solitamente la comunicazione e sincronizzzazione tra processi.
- Lineare
	È quello che solitamente si raggiunge. Abbiamo che $S_P\approx P$, cioè poco più basso di $P$.
- Ideale
	È difficile da raggiungere ed è $S_P=P$.
- Superlineare
	Per raggiungerlo i dati devono essere piccoli abbastanza da entrare nella cache e dobbiamo inoltre usarla in modo ottimo.
	È quando $S_P>P$.
# Span
Lo span di un programma è definito come la parte più lunga del programma che non può essere parallelizzata.
Ovviamente l'obiettivo è ridurre lo span. In questo modo minimiziamo l'effetto della legge di [Amdahl](performace.md#Amdahl).
##### Metodi
Per ridurre lo span spesso dobbiamo ripensare alla struttura del programma.
- Un ciclo $for$ che a ogni iterazione legge da un vettore e poi scrive, è molto poco parallelizzabile. Per parallelizzarlo possiamo usare invece che il vettore un albero, in modo da riprodurre una tencica di divide et impera.