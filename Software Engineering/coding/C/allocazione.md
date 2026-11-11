In [C](C.md) per allocare la memoria esistono sostanzialmente due metodi: l'allocazione automatica e dinamica.
# Stack
È il compilatore che si occupa di inserire nello [stack](processo.md#Stack) le variabili quando una funzione viene chiamata e di liberarla quando essa ritorna. Finiscono in questa memoria tutte le allocazioni a tipi primitivi e i [vettori](vettori.md) di cui abbiamo specificato tipo e dimensione.
Si usa quando la variabile non è troppo grande o vogliamo che venga deallocata quando il suo scope finisce.
# Heap
È il programmatore che si deve occupare di allocarla e liberarla. Questo viene fatto con delle funzioni della $\texttt{stdlib.h}$.
Si usa quando vogliamo che la variabile sopravviva allo scope o quando è molto grande e quindi si rischia di riempire lo stack.
##### Overhead
Oltre alla memoria che richiediamo il kernel alloca anche altri elementi: spazio per dei metadati: dimensione del blocco allocato (informazione usata poi durante la $\texttt{free}$); stato del blocco, cioè se è libero od occupato (immagino per la liberazione logica). Solitamente questo stato occupa circa 8 o 16 Byte e comporta che richiediamo uno spazio piccola ci saranno più meta dati rispetto ai dati.
##### Padding
La CPU preferisce lavorare con multipli di 8 o 16 Byte, a seconda delle architetture. Questo vuol dire che i Byte allocati saranno solitamente multipli di 8 o 16 e questo comporta che se il numero di byte totali, quelli richiesti più quelli dei metadati, non sono un loro multiplo verranno inseriti dei byte di padding.
##### Funzioni
Tali funzioni restituiscono sempre un [puntatore](puntatori.md) a void (i.e., $\texttt{(void *) ptr}$):
- $\texttt{malloc}$
	Inizializza la memoria in modo sporco, cioè a un valore [undefined](undefined%20behaviours.md). È la funzione da chiamare quando non ci interessa il contenuto della memoria perché poi siamo noi a inizializzarla con valori di interesse.
- $\texttt{calloc}$
	Inizializza la memoria a 0. Utile quando ci serve una memoria nulla.
- $\texttt{realloc}$
	Serve per ingrandire o diminuire la memoria destinata a un puntatore.