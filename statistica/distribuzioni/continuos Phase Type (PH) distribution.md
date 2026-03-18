Le Phase Type (PH) sono una classe di distribuzioni utilizzate per modellare tempi di esecuzione in un workflow che non sono [esponenziali](distribuzione%20esponenziale.md), mantenendo i vantaggi matematici della distribuzione esponenziale.
Si definisce come il tempo all'[assorbimento](markov%20chain.md#Stati) in una [CTMC](continuous%20time%20markov%20chain%20(CTMC).md), dove il tale tempo è il tempo di esecuzione citato sopra. Il motivo è che una PH è una qualche composizione di esponenziali, come di fatto lo è una Markov chain; se la Markov chain ha un solo stato assorbente e la probabilità di finirci è maggiore di 0 allora prima o poi ci finiremo; il tempo per risolvere la PH è il tempo in cui si arriva nello stato assorbente.
# Vantaggi
- Variando il numero di fasi $n$ si può approssimare qualunque distribuzione.
- Non si risolvono integrali complessi ma si lavora con le matrici e l'algebra lineare.
# Definizione
Una PH è definita da:
- Un vettore di probabilità iniziali
	Questo determina lo stato della catena in cui si parte.
	Si suppone di non avere una massa di probabilità nello stato assorbente; per questo motivo se abbiamo $n$ stati totali, questo vettore è $\mathbb{R}^{n-1}$ e la Phase type è $PH^{n-1}$.
- Un generatore infinitesimale $Q$
	Siccome l'ultimo stato non ha possibilità di uscita l'ultima riga di questa matrice è nulla (e quindi è inutile anche l'ulitma colonna). per questo motivo si lavora con il sotto generatore infinitesimale $A$.
- Uno stato assorbente unico della markov chain.
##### Tempo all'assorbimento
Il tempo all'assorbimento è una distribuzione la  cui [CDF](random%20variable.md#CDF) è definta da $F(x)=1-\alpha e^{Qx}\mathbb{1}$.
# Forme canoniche
Le PH più usate sono quelle acicliche. Di queste si possono definire tre forme canoniche: ogni PH può essere rappresentata tramite una di queste e si può passare tra loro in modo equivalente.
- CF1
	Ha [questo](PH%20aciclica%20CF1.jpeg) modello.
	Si ha che $\lambda_1\le\lambda_2\le\cdots\le\lambda_n$.
	I parametri sono $2n-1$ perché si hanno $n$ tassi e $n-1$ probabilità.
- CFA
	Ha [questo](PH%20aciclica%20CFA.jpeg) modello.
	L'idea è che si entra sempre nel primo stato e poi si può passare in tutti.
- CFB
	Ha [questo](PH%20aciclica%20CFB.jpeg) modello.
	L'idea è che si entra sempre nel primo stato e poi si può andare nello stato assorbente da tutti.
	È ottima quando si deve modellasistemi che invecchiano: via via che ci avviciniamo allo stato assorbente si deve aumentare il tasso con cui entrarci, cioè la probabilità di fallire.
# Distribuzioni
L'idea è di comporre in modi diversi [distribuzioni esponenziali](distribuzione%20esponenziale.md); ogni combinazione approssima i dati in maniera diversa.
- Le [whitt approximation](whitt%20approximation.md) ci danno approssimano i dati mantenendo solo [media](momento.md#Primo%20ordine) e [varianza](momento.md) dei dati ottenuti.
- Per ottenere i vantaggi computazionali dei polinomi si utilizzano le [Bernstein phase type distributions](bernstein%20phase%20type%20distributions.md).