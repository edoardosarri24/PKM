Un puntatore in [C](C.md) è una variabile a che contiene l'indirizzo in [memoria](memoria.md) di un'altra; si tratta quindi di un gruppo di celle (i.e., o 2 o 4) che contengono un indirizzo.
# Null pointer
Ci sono due varianti.
- $\texttt{NULL}$
	È la versione tradizionale, una macro implementata come $(void\ *)0$ o semplicemente 0 e definita in $<stdio.h>$. Essendo fondamentalmente un numero può causare ambiguità di tipo e il compilatore potrebbe non distinguere tra un puntatore nullo e l'indirizzo 0.
- $\texttt{nullptr}$
	È la versione introdotta con [C23](C.md). Non si tratta di una macro ma una keyword del linguaggio. Ha un tipo specifico, $nullptr\_t$ e non si rischia ambiguità.
# Operazioni
- Dichiarazione
	Un puntatore si dichiara definendo il suo tipo, cioè il tipo di oggetto a cui punta, come $\texttt{tipo *nome}$.
- Dereferenziazione
	Permette di accedere al valore presente nella cella di memoria a cui il puntatore punta.
	Se $\texttt{int x}$ contiene 1 e $\texttt{int *p}$ punta a $\texttt{x}$ allora con $\texttt{int y=*p}$ si ottiene $\texttt{y=1}$.
- Referenziazione
	Permette di ottenere l'indirizzo di una variabile, cioè è il modo in cui definire un puntatore.
	Se vogliamo che $\texttt{int *p}$ punti a $\texttt{int x}$ allora si usa l'assegnazione $\texttt{int *p=\&x}$.
# Funzioni
Quando chiamiamo una funzione gli argomenti vengono [passate per valore](coding/C/funzioni.md#Chiamata%20per%20valore) e quindi qualunque modifica sugli argomenti agirà solo sulla copia locale dei parametri della funzione. Tramite i puntatori possiamo eseguire una chiamata per riferimento, cioè possiamo passare l'indirizzo di memoria e quindi consentire alla funzione di accedere direttamente ai dati.
Consideriamo come esempio una funzione $\texttt{swap(a,b)}$:
- Il chiamante deve passare l'indirizzo di delle variabili, cioè dovrà usare $\texttt{swap(\&a,\&b)}$.
- La funzione dovrà dichiarare i suoi parametri come puntatori e lavorare all'interno con essi, cioè dovrà dichiarare $\texttt{swap(*a,*b)}$.
##### Ritorno
La sintassi per ritornare un puntatore è $\texttt{tipo *nome\_funzione(param)}$.
# Aritmetica
L'aritmetica dei puntatori, usata spesso nei [vettori](vettori.md), è consistente con il tipo: fare $\texttt{p++}$ se $p$ punta all'indirizzo di un elemento di un vettore, permette di andare all'indirizzo dell'elemento successivo a prescindere dal tipo degli elementi.
# Vettori di puntatori
I vettori di puntatori sono spesso usati al posto delle [matrici](vettori.md#Multidimensionali). In pratica si definisce un vettore di puntatori dichiarando la sua dimensionalità (i.e., il numero di elementi, cioè di puntatori) come $\texttt{tipo *nome[dimensione]}$ (o con $\texttt{tipo **nome}$ se non conosciamo la dimensione) e a questo punto i vari elementi del vettore possono essere allocati in modo separato:
- Si può avere elementi di dimensione diversa. Con le matrici anche la seconda dimensione (i.e., la lunghezza dei vari elementi) sarebbe fissata.
- Gli elementi referiti dai puntatori possono essere allocati dinamicamente con $malloc$.
- Si può eseguire l'ordinamento logico, che richiede il solo scambio dei puntatori e non dell'intero dato. Questo è molto più efficiente se i dati sono ad esempio stringhe o qualocsa che occupa più byte di intero.
##### Stack vs heap
La scelta di una sotto è influenzata dal numero di elementi nel vettore.
- Se usiamo $\texttt{tipo *nome[dimensione]}$ allora la variabile diventa locale e sarà memorizzata nello stack. In questo caso $\texttt{dimensione}$ deve essere nota a priori e non deve essere definita a runtime.
- Se usiamo $\texttt{tipo **nome}$ e poi si [alloca](allocazione.md) la sua memoria con una \texttt{malloc} allora andremo a mettere la variabile nello heap.
# Interi
Gli interi in generale non possono essere associati a un puntatore: scrivere $\texttt{int *p=n}$, con $n$ intero, vorrebbe dire forzare il puntatore a puntare all'indirizzo di memoria $n$; questo è molto pericolo perché potrebbe portare a un segmentation fault, cioè quando il programma prova ad accedere a un'area di memoria a cui non dovrebbe aver accesso.
L'unica eccezione è fatta per il valore 0, che rappresenta il puntatore nullo. Al posto che usare 0 solitamente si usa la variabile $null$ presente nella [standard library](standard%20library.md) in $stdio.h$.