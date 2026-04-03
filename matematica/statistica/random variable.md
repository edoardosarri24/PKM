Una random variable $X$ su uno [spazio di probabilità](spazio%20di%20probabilità.md) $(\Omega, F,P)$ è una funzione $X:\Omega\to\mathbb{R}$ tale che $\{\omega\in\Omega|X(\omega)\le x\}\in F, \forall x\in\mathbb{R}$, cioè è una funzione che assegna una probabilità a un esito di un esperimento.
# CDF
![CDF gaussiana](CDF%20gaussiana.png)
La Cumulative Distribution Function (CDF) è la funzione $F_{X}(x)=Prob\{X\le x\}=P(\omega\in\Omega|X(\omega)\le x)$, cioè rappresenta la probabilità che la variabile casuale $X$ (i.e., l'esperimento) assuma un valore $\le x$. Gode di alcune propeirtà:
- Se $X$ è discreta allora $F_X(x)$ è una funzione a gradini. Se $X$ è continua allora $F_X(x)$ è continua.
- È una funzione monotona non decrescente tale che:
	- $lim_{x\to+\infty}F_X(x)=1$, cioè la probabilità che X sia più piccola di un valore molto grande è 1.
	- $lim_{x\to-\infty}F_X(x)=0$, cioè la probabilità che X sia più piccola di un valore molto piccolo è 0.
	- In pratica parte da 0 fino a 1 e non può decrescere.
# PDF
![PDF gaussiana](PDF%20gaussiana.png)
Se la CDF è assolutamente continua (deviabile quasi ovunque con derivata integrabile) allora $X$ ha anche una Probability Density Function (PDF) $f_X(x)=\tfrac{d}{dx}F_X(x)$, cioè è definita come la derivata della CDF.
Indica la probabilità di estratte un certo valore dalla distribuzione, cioè quanto è concentrata la probabilità attorno a un determinato valore.
Gode di alcune proprietà:
- $\int_{-\infty}^{+\infty} f_X(x) dx = 1$, cioè l'area sotto la PDF è sempre 1.
- Non è mai negativa, perché la CDF è monotona non decrescente.
- Informalmente si può scrivere $f_X(x)dx=Prob\{X\in[x,x+dx]\}$. Questo si può vedere anche come una misura ($dx$) che è deformata come la PDF
# Variabili casuali multivariate
Una variabile casuale multivariata è un vettore di variabili casuali $X=(X_1,\cdots,X_n)$ definite sullo stesso spazio di probabilità. Può essere quindi definita come una funzione $\mathbf{X}: \Omega \to \mathbb{R}^n$ tale che $\mathbf{X}(\omega) = (X_1(\omega),\cdots,X_n(\omega))$.
##### Joint CDF
È la stessa cosa della CDF solo nel caso multivariato, ed è definita come
$F_{(X_1,\cdots,X_n)}(x_1,\cdots,x_n)=Prob\{X_1\le x_1,\cdots,X_n\le x_n\}$.
##### Joint PDF
È la stessa cosa della PDF solo nel caso multivariato, ed è definita come
$f_{(X_1,\cdots,X_n)}(x_1,\cdots,x_n)=\tfrac{\partial}{\partial x_1\cdots\partial x_n} F_{(X_1\cdots,X_n)}(x_1,\cdots, x_n)$.
##### Variabili indipendenti
Due [random variable](random%20variable.md) $X_1$ e $X_2$ definite nello stesso spazio di probabilità $(\Omega, F,P)$ sono indipendenti se il valore assunto da una di esse non è condizionato dalla distribuzione dell'altra, cioè se $,\forall x_1,x_2$, $Prob\{X_1\le x_1\}=Prob\{X_1\le x_1|X_2\le x_2\}$.
- Se $X_1$ e $X_2$ sono indipendenti allora $Prob\{X_1\le x_1,X_2\le x_2\}=Prob\{X_1\le x_1\}\cdot Prob\{X_2\le x_2\}$ e vale $f_{(X_1,X_2)}(x_1,x_2)=f_{X_1}(x_1)\cdot f_{X_2}(x_2)$.
##### Ordine stocastico usuale
Due variabili casuali $X$ e $Y$ sono ordinate stocasticamente $X\succeq Y$ se e solo se $F_{X}(x)\le F_{Y}(x)$, $\forall x$. La $X$ è definita maggiore della $Y$ anche se i valori delle probabilità sono più bassi perché se $X$ è maggiore di $Y$, allora $X$ sta a destra $Y$ e quindi la probabilità di $X$ è minore di quella di $Y$ per lo stesso valore di $x$.
Per misurare questo si usa la [dominance](dominance.md) e in questo caso si dice che $X$ domina $Y$.