
Sono sintatticamente e semanticamente simili a dei puntatori e permettono di scorrere tra gli elementi del contenitore ignorandone l'implementazione e la struttura interna.
Essendo simili a puntatori possiamo:
- Accedere all'oggetto puntato con $\texttt{*(ptr).membro}$ o con $\texttt{it->membro}$.
	- Usare l'[Aritmetica](puntatori.md#Aritmetica) dei puntatori e avanzare al prossimo elemento con $\texttt{++}$ e tornare indietro (se l'iteratore è bidirezionale) con $\texttt{--}$.
# Tipi
Abbiamo i seguenti tipi:
- $\texttt{const\_iterator}$ che permette di leggere e basta dalla collezione.
- $\texttt{iterator}$ che permette anche di scrivere.