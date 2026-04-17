La convoluzione è l'operatore matematico ($\star$) che permette di moltiplicare tra loro due polinomi lavoransolo eslusivamente sui coefficienti che li definiscono.
Un uso più particolare è quello della somma di due [random variable](random%20variable.md) indipendenti; in questo caso si lavora con le [PDF](random%20variable.md#PDF) delle random variabile e non è possibile coivolvere le CDF.
# Polinomi
Se abbiamo due polinomi $A(x)$ e $B(x)$ allora possiamo moltiplicarli algebricamente e ottenere $C(x)=A(x)\cdot B(x)$ oppure fare la convoluzione discreta dei loro coefficienti.
Il polinomio risultante $C(x)$ avrà grado $n+m$, dove $n$ è il grado di $A(x)$ e $m$ è il grado di $B(x)$.
##### Operazione
In questo caso, dati $A(x)=\sum_{i=0}^na_{i}x^i$ e $B(x)=\sum_{j=0}^nb_{i}x^i$, avremmo che il $k$-esimo coefficiente di $C(x)$ è definito come $c_{k}=\sum_{i+j=k}a_{i}b_{j}$.
Il costo di questo algoritmo è $\mathcal{O}(n^2)$. Può essere ottimizzato con la FFT (Fast Fourier Transform) che ha un costo di $\mathcal{O}(k\log k)$, con $k$ grado del polinomio risultante.
# Distribuzioni
La convoluzione può essere utilizzata anche per calcolare la somma di due [random variable](random%20variable.md) indipendenti; in questo caso si lavora con le [PDF](random%20variable.md#PDF) delle random variabile e non è possibile coivolvere le CDF.
##### Utilizzo
Questo è comodo per valutare la distribuzione risultante di due eventi sequenziali che hanno tempi di esecuzione indipendenti.
##### Indipendenza
Se utiliziamo la classica convoluzione per sommare variabili casuali che non sono indipendenti quello che otteniamo è un'approssimazione ottimistica del risultato.
##### Concentualmente
Si tratta della media di $f$ pesata per i valori di $g$. Ad esempio se $g$ ha una forma a [campana](distribuzione%20gaussiana.md) allora la convoluzione $(g\star g)$ darà più peso ai valori centrali.