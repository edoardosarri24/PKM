Sia $f:[0,1]\to\mathbb{R}$. L'operatore di Bernstein di ordine $n$ è definito come $B_{n}(f,x)=\displaystyle \sum f(\tfrac{i}{n})b_{i,n}(x)$, dove $b_{i,n}$ sono i polinomi di base di Bernstein e sono definiti dalla [distribuzione binomiale](distribuzione%20binomiale.md) $b_{i,n}(x)=\binom{n}{i}x^i(1−x)^{n−i}$, $i=0,\cdots,n$ e $x\in[0,1]$.
In matematica i polinomi di Bernstein sono utilizzati per approssimare funzioni continue definite in $[0,1]$.
##### Proprietà
Le proprietà fondamentali sono:
- I polinomi di base di Bernstein sono una partizione dell'unità, i.e. $\displaystyle \sum_{i=0}^nb_i,n(x)=1$.
- I polinomi di Bernstein sono sempre non negativi, i.e. $b_{i,n}(x)\ge0,\forall x\in[0,1]$.
- Esiste una relazione di ricorrenza (e.g., algoritmo di de Casteljau) che permette di calcolare i polinomi di grado superiore partendo da quelli di grado inferiore.
##### Vantaggi
- Godono della proprietà di non essere interpolanti e quindi sono molto più stabili quando si aumenta il grado; in pratica non soffrono del fenomeno di Runge, come i polinomi di Lagrange.
- L'operatore di Bernstein è lineare.
- Preservano la forma della funzione che stiamo approssimando.
- Se $f$ è una CDF (o una complementary CDF, i.e. $1-CDF$) con supporto in $[0,1]$, allora $B_{n}(f,x)$ lo è $\forall n\in \mathbb{N}$.
	- $\forall x\in[0,1]$ se $m\le f(x)\le M$ allora $m\le B_{n}(f,x)\le M$.
	- Se $f$ è monotona decrescente/crescente in $[0,1]$ allora anche $B_{n}(f,x)$ è monotona decrescente/crescente. 
	- Il polinomio approssimante consegna il lower e upper bound, i.e. $B_{n}(f,0)=f(0)$ e $B_{n}(f,1)=f(1).$
- Per ogni funzione $f$ continua, l'errore tra $f$ e $B_{n}(f,x)$ tende a 0 se $n\to\infty$. La scelta del grado dipende dall'accuratezza che possiamo avere e dalla fabbilità computazionale.
##### Limiti
I classici polinomi di Bernstein hanno due problemi:
- Trattano potenze di $x$ (i.e., $\{1,x,x^2,\cdots\}$). Questo comporta che le funzioni approssimanti sono definite combinazioni lineari (i.e., somma) di polinomi (i.e., termini con grado diverso). Questo ha dei limiti nella rappresentazione di fenomeno che hanno un decadimento o un incremento non lineare (e.g., all'inizio alto e poi basso, come la classica forma esponenziale) perché dovremmo combinare linearmente molti polinomi e quindi il grado del polinomio di Bernstein sarà elevato.
- Sono definiti sull'intervallo limitato (i.e., $[0,1]$) e quindi non possono approssimare direttamente variabili casuali temporali, visto che si estendono su $[0,∞)$.
# Bernstein Exponential
Con gli exponential Bernstein polynomial si riesce a modellare una curva con un andamento esponenziale. In questo modo possiamo rappresentare fenomeni che hanno un decadimento o un incremento non lineare.
Questi polinomi trovano applicazione con i [BPH](bernstein%20phase%20type%20distributions.md) epr approssimare distribuzioni.
##### Cambio di variabile 1
Un Bernstein phase type è definito a partire da un polinomio di Bernstein classico con un cambio di variabile strettamente monotono: si pone $y=e^{-x}$, o anche $x=−log(y)$. In questo modo si possono approssimare funzioni definite in $[0,∞)$, come le distribuzioni espresse dalla loro [CDF](random%20variable.md#CDF) o [PDF](random%20variable.md#PDF).
##### Cambio di variabile 2
Possiamo aggiungere al cambio di variabile anche un fattore $\lambda$, ovvero porre $y=e^{-\lambda x}$, o anche $x=\frac{−log(y)}{\lambda}$.