Le eccezioni sono la pratica con cui si gestiscono possibili errori che possono avvenire durante l'esecuzione del nostro codice: si può cercare di recuperare l'errore oppure di terminare nel modo correttoil programma.
# Flusso
Il flusso è il seguente:
- Un metodo che può lanciare un eccezione prova ($\texttt{try}$) a eseguirlo.
- Se riconosoce la presenza di un errore lancia ($\texttt{throw}$) un'eccezione per segnalare il problema.
- Un blocco apposito cattura ($\texttt{catch}$) e gestisce l'eccezione, eventualmente rilanciandola.
# Gerarchia
Nel caso un blocco $\texttt{try}$ possa sollevare più eccezioni l'ordine della cattura deve essere dall'eccezione più specializzata a quella più generica.
Se così non è allora la più specializzata non sarebbe mai raggiunta per il concetto del polimorfisfmo: è di tipo più generico.