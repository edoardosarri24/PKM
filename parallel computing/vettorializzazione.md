La vettorizzazione è una tecnica di [parallelillazione](parallel%20pragramming.md) che lavora a livello di [dati](tipi%20di%20parallelismo.md#Instruction%20level) e si occupa dell'ottimizzazione all'interno del singolo core.
L'obiettivo è massimizzare le performance nell'esecuzione di cicli fruttando le più ALU che sono messe a disposizione dalle CPU moderne.
# Idea
Se abbiamo due vettori $a$ e $b$ e vogliamo fare $c=a+b$ ci serve un ciclo sui elementi $i$ che faccia $c[i]=a[i]+b[i]$.
Con l'esecuzione sequenziale si parte dall'indice $i=0$ e si arriva all'indice $n-1$. Usando invece il parallelismo [SIMD](parallel%20pragramming.md#Flynn's%20Taxonomy), si può eseguire la stessa istruzione su più ALU delle stesso core lavorando su più indici contemporaneamente.
# Hardware
Le CPU oggi permettono la vettorizzazione unendo più ALU e registri adatti per la vettorizzazione (i.e., lunghi 256 o 512 bit). In questo modo ogni ALU può lavorare in parallelo su una porzione del registro.
# Tecniche
L'istruzione di vettorizzazione può essere generata in più modi. Sicuramente ci serve un hw che permetta di eseguirla.
##### Assembly
Possiamo scrivere l'istruzione a mano nel linguaggio di livello macchina.
##### Libraries
Si possono usare semplicemente delle librerie low level. Queste permettono di usare l'hw in modo abbastanza semplice. Esempi sono:
- BLAS per l'algebra lineare.
- LAPACK per l'algebra lineare.
- SIMD JSON per fare parsing dei JSON.
- Intel MKL è la migliore per i processori Intel.
##### Auto-vectorization
È il compilatore che riconosce pattern e decide cosa vettorizzare. È la soluzione più effort-less dal punto di vista del programmatore, che si deve occupare solo di scrivere il codice come si deve; ad esempio si deve controllare i fenomeno del [pointer aliasing](pointer%20aliasing.md).
Per forzare il compilatore a vettorizzare il codice ci sono dei flag che possiamo usare durante la compilare; questi dipendono dal compilatore e quindi si deve guadare al momento.
##### Hint al compilatore
Possiamo dare dei suggerimenti al compilatore su come compilare il codice in modo parallelo, usando le istruzioni [pragma](compilazione.md#Pragma).
Ad esempio per vettorizzare un loop possiamo usare $\#pragma\ omp\ simd$: $omp$ è per generare codice portabile (usando openMP) e $simd$ sta per single instruction multiple data.
# Stile di programmazione
Per scrivere codice vettorializzabile ci sono delle accortezze che dobbiamo seguire:
- Usare $restrict$ nelle dichiarazioni di funzioni che hanno come parametri puntatori.
- Usare le istruzioni pragma.
- Ottimizzare il loop più interno di un ciclo annidato.
- Usare il tipo più piccolo necessario ($short$ meglio di $int$).
- Usare blocchi di memoria contigui.
- Usare bene le strutture [AoS e SoA](data-oriented%20design.md).
- Gestire i [false sharing](false%20sharing.md) con il memory allignment.
- Usare gli indici del ciclo per l'indicizzazione quando possibile.
- Non usare $break$ in un ciclo.
- Non usare, se non necessari, condizioni all'interno dei loop.o
- Usare il più possibile codice inline al posto delle chiamate di funzioni.
- Non usare variabili per più scopi; definisci variabili nuove.