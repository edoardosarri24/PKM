Le Phase Type (PH) sono una famiglia di metodi [parametrici](distribuzioni%20empiriche.md#Parametriche) per approssimare la distribuzioni di dati empirici.
# Definizione
Si definisce come il tempo all'[assorbimento](markov%20chain.md#Stati) in una [CTMC](continuous%20time%20markov%20chain%20(CTMC).md) con un unico stato assorbente, dove il tale tempo è il tempo di esecuzione citato sopra.
##### Elementi
Nel contesto delle phase type, quando si dice che i metodi parametrici definiscono i parametri della distribuzione tramite i dati raccolti, si intende trovare quegli elementi che definiscono la phase type:
- Il numero di stati transienti, cioè di stati che compongono la catena di Markov. Queste sono dette phasi.
- Un vettore di probabilità iniziali
	Questo determina lo stato della catena in cui si parte.
	Si suppone di non avere una massa di probabilità nello stato assorbente; per questo motivo se abbiamo $n$ stati totali, questo vettore è $\mathbb{R}^{n-1}$ e la Phase type è $PH^{n-1}$.
- Il rate delle transizioni.
##### Tempo all'assorbimento
Il tempo all'assorbimento è una distribuzione la  cui [CDF](random%20variable.md#CDF) è definta da $F(x)=1-\alpha e^{Qx}\mathbb{1}$.
# Vantaggi
Sono molto apprezzate per la potenza e flessibilità:
- Possiamo definire il trade off tra accuracy dell'approssimazione e complessità computazionale variando il numero di fasi.
- Si può approssimare qualunque distribuzione a valori positivi. A valori positivi si intende qualunque variabile non assuma valori negativi: tempi di esecuzione, latenza di rete o tempi di guasto.
- Non si risolvono integrali complessi ma si lavora con le matrici e l'algebra lineare.
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
# Famiglie
Una volta scelta la famiglia si deve utilizzare i dati per scegliere i parametri della famiglia stessa. Le famiglie più usate sono:
- [Whitt approximation](whitt%20approximation.md)
	Mantengono primo e secondo [momento](momento.md) dei dati.
- [Bernstein PH distributions](bernstein%20phase%20type%20distributions.md)