In [C](C.md) le variabili sono passate alle [funzioni](coding/C/funzioni.md) per valore. Possiamo comunque passare l'indirizzo di queste variabili.
Le variabili devono essere dichiarate e definite:
- Dichiarazione
	Permette al compilatore di aggiungere il nome nella tabelle dei simboli. Non è ancora allocata nessuna memoria.
- Definizione
	Riserva byte per quella variabile in memoria; non è necessario che poi nella memoria ci venga scritto qualcosa.
# Scope
Le variabili possono avere due scope diversi.
##### Interne
Le variabili all'interno di una funzione sono dette interne e/o automatiche.
Sono visibili sono all'interno della funzione.
La loro inizializzare deve essere esplicita, altrimenti è un [undefined behaviours](undefined%20behaviours.md) e si parla di dirty memory.
##### Esterne
Le variabili esterno sono quelle dichiarate all'inizio del file e fuori da ogni funzione.
Il loro scope è quello di programma e possono essere usate da tutte le funzioni, sia locale al file che in altri file: nel file dove è definita può essere usata così come è; in altri file deve prima essere dichiara con la keyword $\texttt{extern}$ per dire al compilatore "guarda io la sua la sua definizione è da un'altra parte".
Tramite la [statiticità](#Statiche) possiamo dare uno scope del file alle variabili esterne.
# Constanti
Una variabile preceduta da $\texttt{const}$ è una variabile il cui valore non dovrebbe cambiare.
Modificare il valore di una constante è un [undefined behaviours](undefined%20behaviours.md).
# Statiche
Il modificatore $\texttt{static}$ serve per ridefinire lo scope di una [variabile estrena](#Esterne) o di una funzione al solo file che la contiene.
Se una [variabile interna](#Interne) a una funzione è dichiarata come statica il suo stato è mantenuto anche dopo la terminazione della funzione.
# Register
Il modificatore $\texttt{register}$ serve per chiedere al compilatore di mantenere quella variabile in un registro perché sarà utilizzata di frequente. Può essere applicata solo a variabili interne e ai parametri di una funzione.
Il compilatore può comunque decidere di ignorare questa istruzione.