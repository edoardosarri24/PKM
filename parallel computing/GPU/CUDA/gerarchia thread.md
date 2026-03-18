Quando si lancia un kernel, cioè il device code che esegue nella [GPU](GPU.md), l'idea è che a ogni thread sia associato a un dato da elaborare: si lancia un numero di thread tale (di solito maggiore) da coprire tutti dati, e poi è l'hw che si occupa della parallelizzazione.
Il lavoro principale è definire la forma del [grid](#Grid) e dei [blocchi](#Block) in modo da assegnare a ogni thread un id con l'idea che l'id rappresenti anche il dato della struttura in input che il thread deve elaborare.
##### Grid
È l'insieme dei thread che sono generati alla chiamata di un kernel. C'è una dimensionalità massima teorica che è $65K^3$ (65K per ogni dimensione): l'idea è di allocare quanti thread vogliamo senza proccuparci di quanti sono.
Tutti i thread del grid condividono la stessa [memoria globale](architettura.md#Globale).
##### Block
All'interno di un grid i thread sono suddivisi in blocchi.
In questo caso abbiamo un massimo di 1024 thread per blocco: quando modelliamo la shape del blocco il prodotto delle dimensioni deve essere minore o uguale di 1024.
È bene inoltre che i thread per blocco siano un multiplo (possibilmente almeno 4 volte circa, i.e. non troppo pochi thread) di 32: lo scheduler alloca warp di 32 thread; se il numero di thread in un blocco non è multiplo di 32 allora in un'allocazione ci saranno dei core che non sono usati.
In un blocco i thread possono cooperare utilizzando la sincronizzazione (i.e., le [barriere](barrier.md)) e la [shared memory](architettura.md#Local%20memory); secondo l'[architettura logica](gestione%20della%20memoria.md#Architettura%20logica) infatti la shared memory è condivisa solo dai thread che appartengono allo stesso blocco, mentre thread di blocchi diversi non possono cooperare in nessun modo.
##### Warp
L'unità più piccola che possiamo mandare in esecuzione non è un singolo thread, ma un warp, cioè un blocco di 32 thread. I thread dello stesso warp eseguono quindi per forza la stessa istruzione, ma ognuno di essi è indipende dagli altri.
Uno warp è assegnato a uno e un solo [streaming multiprocessor](architettura.md#Streaming%20multiprocessor) e non può essere cambiato fino al completamento del warp stesso. All'interno di uno SM possono però essere assegnati più warp.
# Kernel
La funzione kernel in [CUDA](CUDA.md) viene chiamata con $\texttt{kernel<<<dimGrid,dimBlock>>>()}$, dove $dimGrid$ e $dimBlock$ sono le grandezze in tre dimensioni rispettivamente del grid e dei blocchi: grid e blocchi possono essere organizzati in strutture 3D.
CUDA definisce una struct nativa $dim3$ che rappresenta una grandezza in tre dimensioni; se viene impostato solo uno o due parametri allora gli altri vengono messi direttamente a 1. Questo permette anche di specificare uno di questi tre valori (o tutti) tramite una funzione.
# Indici
Per definire l'indice del thread e quindi capire su quale dato lavora, un thread può leggere due variabili real-only e native di CUDA che sono di tipo $uint3$: si tratta di una struct con tre campi dove ognuno rappresentano la dimensionalità del blocco e quindi come possiamo identificare un thread in una struttura in più (massimo 3) dimensioni.
- BlockId
	Rappresenta il numero del blocco all'interno del grid a cui il thread appartiene.
- ThreadId
	Rappresenta il numero del thread all'interno del blocco.
##### Associazione
L'associazione tra un thread e il proprio id è deterministica. Viene assegnato prima considerando l'indice del blocco e poi in ordine le coordinate $x$, $y$ e $z$.
Usando una formula simile alla linearizzazione delle matrici in memoria, ogni thread ottiene l'indice in memoria di un dato da elaborare.