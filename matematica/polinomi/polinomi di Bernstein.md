In matematica i polinomi di Bernstein sono utilizzati per approssimare funzioni continue definite in $[0,1]$.
# Definizione
Nel definire Bernstein dobbiamo dividere il concetto di operatore di Bernstein e forma di Bernstein.
Rispetto alla base dei polinomi, dove usiamo una somma pesata di potenze di $x$ (i.e., $\{1,x,x^2,\cdots\}$) in questo modo riusciamo a rappresentazione fenomeni che hanno un decadimento o un incremento non lineare (e.g., all'inizio alto e poi basso, come la classica forma esponenziale).
##### Operatore
Sia $f:[0,1]\to\mathbb{R}$. L'operatore o polinomio di Bernstein di ordine $n$ è definito come $B_{n}(f,x)=\displaystyle \sum_{i=0}^n f_{i,n}b_{i,n}(x)$, dove:
- Coefficienti
	$f_{i,n}$ sono i coefficienti e rappresentano il peso associato al relativo elemento della base. L'$i$-esimo coefficiente è il valore della funzione in $\frac{i}{n}$, cioè $f\left( \frac{i}{n} \right)$: in questo modo stiamo prendendo $n$ valori della funzione $f$ in punti equi spaziati.
	C'è una relazione tra il numero di coefficienti e il grado $n$ del polinomio: $\text{numero coefficienti}=n+1$.
- Base
	$b_{i,n}$ sono i polinomi di [base](#Base) di Bernstein. Sono definiti dalla [distribuzione binomiale](distribuzione%20binomiale.md): $b_{i,n}(x)=\binom{n}{i}x^i(1−x)^{n−i}$, $i=0,\cdots,n$.
##### Forma
L'operatore di Bernstein tira fuori un polinomio in forma di Bernstein basandosi sulla funzione da approssimare.
La forma di Bernstein è invece la forma generica di ogni polinomio di Bernstein, definita come $p(t)=\displaystyle \sum_{{i=0}}^nc_{i}b_{i,n}(t)$, dove $b_{i,n}(t)$ è la base di Bernstein. I coefficienti non sono ottenuti nel modo definito dall'operatore, ma possono essere definiti liberamente.
# Convergenza
I polinomi di Bernstein verificano il [teorema di Weierstrass](teorema%20di%20Weierstrass.md) e quindi l'errore tra funzione $f$ e polinomio $B_{n}(f,x)$ tende a 0 se $n\to\infty$ in modo uniforme, cioè $\forall t\in[0,1]$.
Questo è dovuto a due proprietà della base: i valori sono non negativi per $t\in[0,1]$ e sono una somma dell'unità (i.e., $\displaystyle \sum_{i=0}^nb_{i,n}(t)=1$).
# Approssimazione
L'operatore di Bernstein interpola la funzione solo in $f(0)$ e $f(1)$, mentre negli altri valori (i.e., coefficienti) non è interpolante. Questo implica che l'operatore di Bernstein non sia la migliore approssimazione della funzione $f$; un esempio è un polinomio di grado $n>1$.
Questo non vuole dire che non esista una [forma di Bernstein](#Forma) che non possa rappresentare fedelmente una funzione, ma solo che dobbiamo scegliere dei nuovi coefficienti un altro modo.
# Proprietà
Gli utilizzi e algoritmi più utili derivano da alcune proprietà della forma di Bernstein e delle sue basi.
![bernstein basis](bernstein%20basis.png)
##### Simmetria
Le funzioni di base $b_{i,n}(t)$ e $b_{n-i,n}$ sono simmetriche rispetto a $t=\frac{1}{2}$.
##### Ricorsione
La base di grado $n+1$ può essere generata a partire da quella di grado $n$ in modo ricorsivo come $b_{i,n+1}(t)=tb_{i-1,n}(t)+(1-t)b_{i,n}(t)$ per $k=0,\cdots,n+1$, dove $b_{i,n}(t)=0$ se $k<0$ o $k>n$; la ricorsione è inizializzata con $b_{0,0}(t)=1$.
##### Non negatività
I valori delle basi sono non negative, i.e., $b_{i,n}(t)\ge{0}$ per $t\in[0,1]$.
##### Partizione dell'unità
La base di Bernstein è una partizione dell'unità, i.e. $\displaystyle \sum_{i=0}^nb_{i,n}(t)=1$.
##### Lower and upper bound
La forma di Bernstein soddisfa la proprietà per cui agli estremi il valore è esattamente quello della funzione, i.e. $p(0)=c_{0}$ e $p(1)=c_{n}$.
##### Relazione con la base monomiale
Le basi Bernstein e monomiale sono correlate dalla relazione $t^{i}=\sum_{j=i}^{n}\frac{\binom{j}{i}}{\binom{n}{i}}b_{j}^{n}(t)$ e $b_{i,n}(t)=\sum_{j=i}^{n}(-1)^{j-i}\binom{n}{j}\binom{j}{i}t^{j}$.
##### Scalare il supporto in $[0,r]$
Cambiando variabile da $t\to rt$ possiamo mappare il supporto da $[0,1]$ in $[0,r]$.
# Supporto $[a,b]$
Se abbiamo una funzione $f$ in $[a,b]$ continua allora il polinomio di Bernstein è definito come $B_{n}(f,t)=\displaystyle \sum_{i=0}^n f_{i,n}b_{i,n}(t)$, dove $t=\frac{x-a}{b-a}$.
# Operazioni
Le operazioni che possono essere fatte su due polinomi $f(t)$ e $g(t)$ sono diverse.
##### Addizione
Se il grado è lo stesso allora si sommano o sottragono i coefficienti.
Se i due gradi sono diversi allora dobbiamo prima matchare i gradi aumentando il grado del polinomio con grado minore.
##### Moltiplicazione
Se $f(t)$ ha grado $m$ e coefficienti $a_{0},\cdots,a_{m}$ e $g(t)$ ha grado $n$ e coefficienti $b_{0},\cdots,b_{n}$ allora il polinomio risultante dal loro prodotto ha coefficienti $c_{k}=\sum_{j=max(0,k-n)}^{min(m,k)}\frac{\binom{m}{j}\binom{n}{k-j}}{\binom{m+n}{k}}a_{j}b_{k-j}$, con $k=0,\dots,m+n$.
Questa operazione può essere utilizzata per fare una [convoluzione](matematica/polinomi/convoluzione.md) discreta dei due polinomi.
##### Divisione
Vedi [articolo](the_Bernstein_polynomial_basis_a_centennial_retrospective.pdf) alla proprietà 14.
##### Convoluzione
Se i due polinomi di Bernstein rappresentano due distribuzioni di probabilità (i.e., [PDF](random%20variable.md#PDF)) allora la [moltiplicazione](#Moltiplicazione) può essere utilizzata.
Il supporto del polinomio risultante sarà in $[a+c,b+d]$, anche nel caso di due supporti in $[0,1]$.