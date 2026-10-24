La distribuzione gaussiana o normale è spesso usata come prima approssimazione quando si vuole rappresentazione una [random variable](random%20variable.md) che si concentra in un singolo valore medio.
# Definizione matematica
La distribuzione normale è definita da due parametri come $P(x|\mu,\sigma^2)=\tfrac{1}{\sqrt{w\pi\sigma^2}}exp(-\tfrac{1}{2}(\tfrac{x-\mu}{\sigma})^2)$, dove:
- $\mu$ è la [media](momento.md#Valore%20atteso), cioè il valore in cui la variabile casuale si distribuisce. È quindi un parametro di posizione.
- $\sigma$ è la [varianza](momento.md#Varianza). Si può vedere come la regione di confidenza in cui con alta probabilità il valore si troverà. È quindi un parametro di scala che stringe o allarga la distribuzione.
# Normale standard
La gaussiano si dice normale stardard (o ridotta) quando ha media $\mu = 0$ e varianza $\sigma^2 = 1$.
Viene solitamente indicata con la variabile $Z$ e la sua PDF diventa
$$f(x) = \frac{1}{\sqrt{2\pi}} \exp\left(-\frac{1}{2}x^2\right).$$
##### Trasformazione
Qualsiasi random variable normale $X \sim \mathcal{N}(\mu, \sigma^2)$ può essere convertita in una normale standard $Z$ tramite la trasformazione di standardizzazione $Z = \frac{X - \mu}{\sigma}$.
# Grafici
- [PDF](random%20variable.md#PDF)
	![PDF gaussiana](PDF%20gaussiana.png)
- [CDF](random%20variable.md#CDF)
	![CDF gaussiana](CDF%20gaussiana.png)