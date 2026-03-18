Le CPU possono essere visti come dispositivi ottimi se abbiamo una decomposizione del problema a [livello di task](decomposizione.md#Task%20decomposition); le GPU invece sono progettate per vere una [data decomposition](decomposizione.md#Data%20decomposition), cioè se devono eseguire istruzioni [SIMD](parallel%20pragramming.md#Flynn's%20Taxonomy).
# CPU
I dati vengono divisi in chunk (i.e., insime di indici in un ciclo for) e ogni chucnk viene eseguito da un thread.
Ogni thread ottiene quindi un vettore di dati ed esegue su quello. Se abbiamo le ALU per la vettorizzazione (i.e., mettono insieme più dati e con un ciclo di clock eseguono più operazioni) a disposizione allora possiamo elaborare più dati contemporanemente, ma sennò il thread esegue in modo sequenziale su ogni dato del suo chunk.
# GPU
Si parla di Single Instruction Multiple Thread (SIMT).
- Ogni thread ha il proprio stato (i.e., program counter, stack e altro) ed è totalmente indipendente dagli altri.
- Solo i thread dello stesso [warp](parallel%20computing/GPU/concorrenza.md#Warp) eseguono la stessa istruzione.
- L'esecuzione di thread diversi è totalmente indipendente anche se appartengono allo stesso warp. Questo comporta che anche se il codice è lo stesso il dato può condizionare l'esecuzione.