attiIn matematica i polinomi di Bernstein sono utilizzati per approssimare funzioni continue definite in $[0,1]$.
# Definizione
Sia $f:[0,1]\to\mathbb{R}$. L'operatore di Bernstein di ordine $n$ è definito come $B_{n}(f,x)=\displaystyle \sum_{i=0}^n f\left( \frac{i}{n} \right)b_{i,n}(x)$, dove:
- Coefficienti
	$f\left( \frac{i}{n} \right)$ sono i coefficienti e rappresentano il peso associato al relavo elemento della base. C'è una relazione tra il numero di coefficienti e il grado $n$ del polinomio: $\text{numero coefficienti}=n+1$.
- Base
	$b_{i,n}$ sono i polinomi di base di Bernstein.
# Vantaggi
I vantaggi nell'utilizzo dei polinomi di Bernstein sono diversi.
- Se $f$ è continua, l'errore tra $f$ e $B_{n}(f,x)$ tende a 0 se $n\to\infty$. La scelta del grado dipende dall'accuratezza che vogliamo ottenere e dalla fattibilità computazionale.
- Preserva la forma della funzione che stiamo approssimando
##### Operatore
- L'operatore di Bernstein è lineare.
##### Base
- I polinomi di base di Bernstein sono una partizione dell'unità, i.e. $\displaystyle \sum_{i=0}^nb_{i,n}(x)=1$.
- I polinomi di base di Bernstein sono sempre non negativi, i.e. $b_{i,n}(x)\ge0,\forall x\in[0,1]$.
- Non sono interpolanti, tranne che negli estremi. Questo garantisce che.
	- Sono molto più stabili all'aumentare del grado $n$.
	- Non soffrono del fenomeno di Runge (i.e., aumento dell'errore in prossima degli estremi), come i polinomi di Lagrange.
##### Distribuzioni
- Se $f$ è una CDF (o una complementary CDF, i.e. $1-CDF$) con supporto in $[0,1]$, allora anche $B_{n}(f,x)$ è una CDF (o una complementary CDF, i.e. $1-CDF$) $\forall n\in \mathbb{N}$.
	- $\forall x\in[0,1]$ se $m\le f(x)\le M$ allora $m\le B_{n}(f,x)\le M$.
	- Se $f$ è monotona decrescente/crescente in $[0,1]$ allora anche $B_{n}(f,x)$ è monotona decrescente/crescente. 
	- Il polinomio approssimante consegna il lower e upper bound, i.e. $B_{n}(f,0)=f(0)$ e $B_{n}(f,1)=f(1)$.
# Limiti
I classici polinomi di Bernstein hanno due problemi:
- Sono definiti sull'intervallo limitato (i.e., $[0,1]$) e quindi non possono approssimare direttamente variabili casuali temporali, visto che queste si estendono su $[0,∞)$.
# Basi
In generale ci sono due basi che possiamo utilizzare.
##### Classica
La base classica è definita dalla [distribuzione binomiale](distribuzione%20binomiale.md) e eprmette di rappresentare $b_{i,n}(x)$ come $b_{i,n}(x)=\binom{n}{i}x^i(1−x)^{n−i}$, $i=0,\cdots,n$ e $\forall x\in[0,1]$.
- In questo modo stiamo approssimando la funzione $f$ facendo una somma pesata di potenze di $x$ (i.e., $\{1,x,x^2,\cdots\}$): non riescono a rappresentazione fenomeni che hanno un decadimento o un incremento non lineare (e.g., all'inizio alto e poi basso, come la classica forma esponenziale); dovremmo avere una grado $n$ molto alto per avere una buona precisione e questo non è computazionalmente fattibile.
- Possiamo in realtà approssimare anche funzioni definite in un qualuque intervallo $[a,b]$, con $a,b$ valori finiti. Questo viene fatto mappando $x\to \frac{x-a}{b-a}$.
- Esiste una relazione ricorsiva (i.e., algoritmo di de Casteljau) che permette di calcolare i polinomi di grado superiore partendo da quelli di grado inferiore in modo numericamente molto stabile. La soluzione più efficiente è precalcolare i coefficienti binomiali e memorizzarli in una tabella e poi usare l'approccio diretto.
##### Esponenziale
L'obiettivo è riuscire a modellare una curva con un andamento esponenziale per rappresentare fenomeni che hanno un decadimento o un incremento non lineare. L'implementazione deriva da un cambio di variabile strettamente monotono: si pone $y=e^{-\lambda x}$ (i.e., $x=\frac{−log(y)}{\lambda}$).
- Questo permette di poter rappresentare funzioni definite in $[0,∞)$, come le [CDF](random%20variable.md#CDF).