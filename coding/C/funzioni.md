Le funzioni servono per scomporre un programma in moduli più semplici, con l'idea che chi li usa non deve conoscere la loro implementazione. Il [C](C.md) è ideato per avere più funzioni semplici e piccole, visto che sono efficienti e facilmente utilizzabili.
# Convenzioni
- Una funzione non specifica il valore di ritorno questo è un [undefined behaviours](undefined%20behaviours.md).
- Se la funzione restituisce 0 allora ha avuto un corretto comportamento; altrimenti restituisce un valore non nullo.
# Dichiarazione
Prima di essere usata una funzione deve essere dichiarata, cioè deve essere specificata la sua firma (i.e., nome, tipo di ritorno e parametri). Solitamente questo si fa negli header file.
La dichiarazione della funzione deve contenere il tipo di ritorno, il nome e tra parantesi il tipo dei parametri (non serve il nome).
##### No parameters
Se non ci sono parametri allora si deve indicare $\texttt{void}$ tra parantesi. Senza questa convenzione stiamo dicendo al compilatore di non fare nessun controllo sugli argomenti.
##### Ottimizzazione
La dichiarazione di una funzione avviene nell'header. Se una funzione è molto breve e piccola (i.e., massimo 5 righe) allora ha senso definirla (i.e., implementarla) nell'header stesso. In questo modo si sta chiedendo più esplicitamente al compilatore di usare l'inlining e di ottimizzare il codice.
# Chiamata per valore
Gli argomenti sono passati alle funzioni per valore.
Questo implica che il valore sarà memorizzato (con tutto il processo di copia, costoso se la variabile è grande) in una variabile locale alla funzione chiamata e un suo qualche cambiamento non avrà influenza sulla variabile della funzione chiamante.
Per passare il rifermento si possono passare gli indirizzi delle variabili.
##### Vettori
Quando un vettore è passato come argomento stiamo in realtà passando l'indirizzo del suo primo elemento. Questo comporta che le modifiche al vettore saranno visibili anche sulla variabile della funzione chiamante.
# Statica
Una funzione $static$, come una [variabile](coding/C/variabili.md#Statiche), è visibile solo nel file dove è dichiarata.
- Permette di evitare conflitti di nomi in file diversi.
- Permette ottimizzazioni più spinte come l'inlining (scrittura della funzione direttamente nel codice e non eseguire chiamate).