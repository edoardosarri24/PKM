Un false sharing non è un problema che invalida una proprietà [safety](proprietà.md#Safety) (come succede con le [race condition](race%20condition.md)), ma è relativo alle performance in un sistema [parallelo](parallel%20pragramming.md) ma anche sequenziale.
# Descrizione
Si verifica quando due o più [thread](thread.md) vogliono accedere a dati diversi che hanno una qualche località spaziale per cui sono entrambi caricati nelle linee di cache dei core dove eseguono i thread.
Il primo thread che riscrive il proprio dato causa un'invalidazione della riga di cache dell'altro thraed: le due viste della RAM sono diverse e l'hw non permette questa situazione.
Il tutto è risolto dall'hw caricando nuovamente la linea di cache invalidata, ma questo porta a cali di performance.
# Soluzione
Per risolvere questo problema ci sono diverse soluzioni.
##### Allineamento
Il compromesso per risolvere questo problema è caricare in memoria solamente quanto necessario, allineando il dato alla dimensione della riga di cache. Questo fa si che venga caricato solo il dato necessario e non anche i dati vicini.
Facciamo un esempio: se una linea di cache è di 64Byte e il dato occupa solo 4 Byte, si può caricare 4 Byte e poi introdurre del padding, cioè dei Byte fittizi finché non si riempie la linea di cache.
Per fare questo si può usare in [C++](C++.md) la funzione $\texttt{std::bit\_ceil(N)}$ della [STL](STL.md) dove $\texttt{N}$ è il il numero di cui si vuole calcolare la potenza di due più vicina. È una fuzione introdda da C++20.
##### Scritture
Ridurre il numero di scritture degli oggetti. In questo modo la linea di cache non sarà invalidata e non dovrà essere caricata nuovamente in memoria.
Si può ad esempio scrivere in una variabile locale al thread e alla fine scrivere nella variabile condivisa.
# Identificazione
Trovare un problema di questo tipo non è banale. Quello che si può guardare è: dati vicini che sono acceduti da thread diversi: oggetti vicini nello stesso array; campi vicini nello stesso oggetto; dati statici che il linker decide di mettere vicini in memoria.