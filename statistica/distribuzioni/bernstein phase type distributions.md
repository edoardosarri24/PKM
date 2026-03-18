I Bernstein phase type (BPH) distributions sono [Phase Type distribution](continuos%20Phase%20Type%20(PH)%20distribution.md) con cui si possono approssimare distribuzioni (i.e., CDF) utilizzando i [Bernstein Exponential](polinomi%20di%20Bernstein.md#Bernstein%20Exponential).
# Algoritmo
Data la CDF $F(x)$ con supporto $[0,\infty)$, la sua approssimazione $\hat{F}(x)$ definita tramite i BE è definita da $\hat{F}(x)=\displaystyle \sum_{i=0}^nF(\log\tfrac{n}{i})T_{n,i}(x)$, dove $T_{n,i}(x)=\binom{n}{i}e^{-ix}x(1−e^{-x})^{n−i}$. Per $i=0$ dovremmo calcolare $F(\log\tfrac{n}{0})$; questo è risolto considerando invece $F(x)$ con $x\to\infty$ e quindi si usa 1.
##### Scelta di $\lambda$
Possiamo usare i BE con l'induzione del parametro $\lambda\ne0$. Questo cambia il modo in cui l'approssimazione viene generata:
- Alto
	Se prendiamo un $\lambda>1$ stiamo schiacciando i punti campionati in cui l'algoritmo guarda la funzione originale più vicino a 0.
- Basso
	Se prendiamo un $\lambda <1$ stiamo spalmando maggiormento i punti nell'intervallo e li stiamo spostando verso destra.
# CCDF
C'è un teorema in [questo articolo](approximation%20of%20cumulative%20distribution%20functions%20by%20bernstein%20phase-type%20distributions.pdf) (proposizione 1) che afferma che approssimare $\bar{F(x)}=1-F(x)$ con BE o fare il complemento di $\hat{F}(x)$, il BE che approssima $F(x)$ è la stessa cosa.
# Stocasticamente ordinate
In alcune situazioni come il [real time](real%20time.md) vogliamo lavorare con approssimazioni [stocasticamente ordinate](random%20variable.md#Ordine%20stocastico%20usuale) delle distribuzioni reali: questo vuol dire che vogliamo trovare un'approssimazione che sia sempre più grande o più piccola della vera distribuzione.
##### Definizioni
Possiamo definire $F_{+\epsilon}(x)=\min[F(x)+\epsilon,1]$ e $F_{-\epsilon}(x)=\max[F(x)-\epsilon,0]$; queste rappresentano la curve $F(x)$ traslata di una quantità $\epsilon$.
##### Problemi
Il problema di queste nuove curve è che $F_{+\epsilon}(x)$ ha una massa di probabilità di 0 e $F_{-\epsilon}(x)$ non ha tutta la massa di probabilità per $x\to\infty$.
##### Approssimazione
Se vogliamo un'approssimazione stocasticamente più grande (le assunzioni sono simili per avere un'approssimazione stocasticamente più piccola) della CDF $F(x)$ allora dobbiamo approssimare $F_{+\epsilon}(x)$ ($F_{-\epsilon}(x)$ per quella stocasticamente più piccola) usando i BPH con un grado $n$ abbastanza grande, come mostra [questo articolo](approximation%20of%20cumulative%20distribution%20functions%20by%20bernstein%20phase-type%20distributions.pdf) (proposizione 3).
Se le seguenti condizioni sono rispettate allora i problemi di cui sopra non si presentano ed è garantito esistere un valore $n$ abbastanza grande:
- Si sceglie $\epsilon\in[0,0.5]$.
- Decadimento della coda
	La coda delle distribuzioni ($x\to\infty$) deve andare a 0 più velocemente di un'esponenziale $ce^{-x}$ per qualche $c\in\mathbb{R}$.
- Comportamento all'origine
	La derivata in $x=0$ non deve essere completamente piatta per un numero infinito di ordini.
##### Grado $n$
Il grado $n$ deve essere abbastanza grande, ma dipende anche dal livello di approssimazione che volgiamo avere. In generale possiamo dire che ([sezione 5](approximation%20of%20cumulative%20distribution%20functions%20by%20bernstein%20phase-type%20distributions.pdf)) $n$ cresce linearmente con $\frac{1}{\epsilon}$: se volgiamo passare da una precisione di $0.01$ a una di $0.001$ allora la BPH dovrà avere un numero di fasi 10 volte maggiore.