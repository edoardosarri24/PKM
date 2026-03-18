La memoria virtuale serve per risovere il problema della limitazione della [memoria](memoria.md).
- Ogni processo crede di aver a disposizione tutta la RAM esistente.
- Il programmatore non fa fatica a lavorare con indirizzi che sono sparsi in memoria fisica, in quanto lavora con indirizzi continui.
- Un processo può usare più memoria di quella che effettivamente è presente grazie allo swapping, il processo che leva una pagina dalla memoria fisica e ne carica un'altra.
- Quando facciamo una $fork$ il kernel non copia tutta la memoria del processo padre in modo che il figlio abbia un'identica copia, ma mappa le stesse pagine in frame. Se poi il figlio scrive qualcosa di nuovo allora si cambia effettivamente il contenuto del frame.
# Spazio logico e fisico
Quando un processo lavoro con gli indirizzi della memoria la CPU genera indirizzi logici (virtuali) che vanno da 0 a $2^n - 1$, dove $n$ è l'architettura (i.e., la dimensione dei registri). Un indirizzo logico non è un valore atomico, ma è composto da una coppia $(p,d)$: $p$ è l'indice della pagina; $d$ è l'offset all'interno della pagina $p$-esima.
Quando la CPU dice di voler accedere a un indirizzo logico, la MMU (Memory Management Unit) si occupa della traduzione in tempo reale: la MMU svolge quindi una funzione $f: \text{indirizzo virtuale} \rightarrow \text{indirizzo fisico}$.
# Paging
La memoria (fisica e virtuale) è divisa in blocchi elementari: nel caso della memoria virtuale si parla di pages; nel caso della memoria fisica di frame. L'idea è che una pagina possa essere mappata in un frame oppure possa esistere ma senza essere presente in RAM.
##### Tabella delle pagine
L'associazione tra pagine e frame è memorizzata in una struttura dati gestita dal kernel, detta page table, dove ogni entry è chiamata PTE (Page Table Entry). Oltre all'indirizzo del frame, contiene anche metadati: presenza o assena della pagina in memoria; permessi; dirty bit, cioè se la pagina è stata modificata.
In un sistema a 64bit la memorizzazione della page table occuperebbe decide di GB. Per risolvere questo problema si usa una multi-level page table.
##### TLB
Il Translation Lookaside Buffer (TLB) risolve il problema per cui accedere ogni volta alla RAM dove è memorizzata la page table e fare la mappatura da indirizzo logico a fisico sarebbe troppo lento. Si tratta di una piccola cache che memorizza le mappature con un'alta località temporale (i.e., le mappature più frequenti). Se abbiamo un TLB hit allora l'associazione esiste è la mappatura è alla velocità massima; se abbiamo un TLB miss allora si deve leggere la page table in RAM ed è molto più lento.
# Pinned memory
In [C++](C++.md) quando allochiamo della memoria, sia sullo stack che sullo heap, questa memoria è di default paginabile: l'OS può in ogni momento, se ha bisogno di ram fisica, spostare i dati sul disco e poi riportarli in memoria quando sono nuovamente richiesti.
Per evitare questo, comodo quando si trasferisce dati su un altra memoria come nella memoria dell [GPU](GPU.md), si deve indicare all'OS che quella memoria allocata è pinned, cioè non deve essere mai portata su disco ma deve essere sempre disponibile.
# Tool
In [C](C.md) la funzione principale che permette di mappare un file nello spazio virtuale del processo è $mmap$. 