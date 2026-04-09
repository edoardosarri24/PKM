I Bernstein phase type (BPH) distributions sono [phase type distribution](phase%20type%20distribution.md) che utilizzano i [polinomi di Bernstein](polinomi%20di%20Bernstein.md) per approssimare in maniera analita una ECDF.
# Conseguenze
Oltre ai [vantaggi dei classici polinomi](polinomi%20di%20Bernstein.md#Conseguenze) abbiamo:
- Se la funzione da approssimare $f$ è una CDF (o una complementary CDF, i.e. $1-CDF$) con supporto in $[0,1]$, allora anche $B_{n}(f,x)$ è una CDF (o una complementary CDF, i.e. $1-CDF$) $\forall n\in \mathbb{N}$.
- Se $m\le f(x)\le M$ allora $m\le B_{n}(f,x)\le M$.
- Il polinomio approssimante consegna il lower e upper bound, i.e. $B_{n}(f,0)=f(0)$ e $B_{n}(f,1)=f(1)$.
# Fasi
Quando si approssima una ECDF la scelta del numero di fasi $n$ (i.e., dell'ordine del polinomio) determina una relazione tra due tipi di errori:
- Approssimazione (bias)
	Misura l'errore del modello matematico, cioè quando la pahse type che sta sotto il polinomio di Bernstein è distante dalla vera distribuzione se avessimo dati infiniti.
	Aumentare il numero di fasi $n$ riduce questo errore, visto che il modello diventa più complesso e capaci di fittare curve più complesse.
- Stima (varianza)
	Misura l'errore proveniente dal fatto che nella realtà abbiamo un numero di dati finito e se il grado del polinomio è troppo alto facciamo overfitting sui pochi dati che in realtà abbiamo.
	In questo caso aumentare il grado $n$ del polinomio aumenta l'errore.
##### Soluzione
Soluzioni empiriche ci mostrano come il grado ottimale per bilanciare l'errore bias-varianza sia $n=\frac{M}{\log(M)}$, dove $M$ è il numero di campioni della ECDF che vogliamo approssimare.
# Approssimazione CDF
Tramite un [polinomio di Bernstein](polinomi%20di%20Bernstein.md) di grado $N$ possiamo approssimare una data ECDF (Empirical [CDF](random%20variable.md#CDF)) $F$.
Definiamo $F_{M}(x)=\frac{1}{M}\sum_{i=1}^MI\{X_{i}\le x\}$, cioè la probabilità che un valore all'interno della distribuzione campionaria sia minore di $x$.
##### Intervallo $[0,1]$
L'approssimazione è data dal semplice polinomio di Bernstein $B_{N}(F_{M},x)=\displaystyle \sum_{n=0}^N F_{M}(\tfrac{n}{N})\binom{N}{n}x^n(1-x)^{N-n}$, con $x\in[0,1]$.
##### Intervallo $[a,b]$
L'approssimazione è data dalla valutazione del polinomio in $\tfrac{x-a}{b-a}$, cioè usando la base [lineare](polinomi%20di%20Bernstein.md#Lineare) e allora abbiamo  $B_{N}(F_{M},\tfrac{x-a}{b-a})=\displaystyle \sum_{n=0}^N F_{M}(a+\tfrac{n}{N}(b-a))\binom{N}{n}\tfrac{(x-a)^n(b-x)^{N-n}}{(b-a)^N}$, con $x\in[a,b]$.
##### Intervallo $[0,+\infty)$
Si deve valutare il polinomio di Bernstein con la [base sponenziale](polinomi%20di%20Bernstein.md#Esponenziale). Si ottiene così la Bernstein phase type $BPH_{N}(F_{M},x)=\displaystyle \sum_{n=0}^N F_{M}\left(\log(\tfrac{N}{n}) \right)\binom{N}{n}e^{-n x}(1-e^{-x})^{N-n}$, con $x\in[0,+\infty)$.
# Approssimazione PDF
Possiamo approssimare la PDF di una data [CDF](random%20variable.md#CDF) (o ECDF) tramite un [polinomio di Bernstein](polinomi%20di%20Bernstein.md) derivando l'approssimazione della CDF ottenuta con il polinomio di Bernstein.
##### Intervallo $[0,1]$
L'approssimazione è data dalla derivata dell'approssimazione della CDF, cioè da $B_{N}'(F_{M},x)=\displaystyle \sum_{n=1}^N\left( F_{M}(\tfrac{n}{N})-F_{M}(\tfrac{n-1}{N})\right)n\binom{N}{n}x^{n-1}(1-x)^{N-n}$, con $x\in[0,1]$.
##### Intervallo $[a,b]$
L'approssimazione è data dalla derivata del polinomio con un fattore definito dalla derivata della mappatura, cioè abbiamo $\displaystyle \tfrac{1}{b-a}B_{N}'(F_{M},\tfrac{x-a}{b-a})$, con $x\in[a,b]$.
##### Intervallo $[0,+\infty]$
L'approssimazione è data dalla derivata del polinomio con un fattore definito dalla derivata della mappatura, cioè abbiamo $BPH_{N}'(F_{M},x)=\displaystyle \sum_{n=1}^N \left( F_{M}(\log(\tfrac{N}{n-1}))-F_{M}(\log(\tfrac{N}{n})\right)n\binom{N}{n}e^{-n x}(1-e^{- x})^{N-n}$, con $x\in[0,+\infty)$.







# CCDF
C'è un teorema in [questo articolo](approximation%20of%20cumulative%20distribution%20functions%20by%20bernstein%20phase-type%20distributions.pdf) (proposizione 1) che afferma che approssimare $\bar{F}(x)=1-F(x)$ con BE o fare il complemento di $\hat{F}(x)$, il BE che approssima $F(x)$ è la stessa cosa.
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