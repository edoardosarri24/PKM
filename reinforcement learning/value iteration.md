La value iteration è un algoritmo model-based (richiede la conoscenza della [markov transition density](reinforcement%20learning/markov%20decision%20process.md#Markov%20transition%20density) e della funzione di [reward](reward.md)) per approssimare la [value function](value%20function.md) ottima (e quindi la policy ottima) quando l'[equazione di ottimalità di Bellman](reinforcement%20learning/policy.md#Equazione%20di%20ottimalità%20di%20Bellman) è troppo complessa da risolvere in forma chiusa
A partire da una value function inziale $V_0$, calcola una sequenza di value function che convergono alla value function ottima $V^o$.
# Algoritmo
L'idea è di risolvere lo stesso problema dell'equazione di ottimalità di Bellman ma usando l'approssimazione precedente al posto della value function ottima.
L'algoritmo è iterativo e ottiene la prossima value fucntion come $V_{k+1}(x)=T(V_k)=\displaystyle\max_u\{r(x,u)+\gamma\sum_{x'\in\mathbb{X}}\phi(x'|x,u)V_k(x')\}$ a partire da una qualche value function iniziale, dove $T(V_k)$ è detto operatore di Bellman (si ha che $V^o$ è un punto fisso dell'operatore di bellman, cioè $T(V^o)=V^o$).
##### Versioni
Ci sono due versioni:
- Sincrona
	In ogni iterazione si aggiorna il valore di ogni stato.
	Richiede la memorizzazione di tutto il vettore della value function.
- Asincrona
	Si campiona dall'insieme degli stati e si aggiorna solo lo stato campionato. Questo ha due vantaggi:
	- L'implementazione richiede di memorizzare un solo vettore: l'occupazione di memoria è la metà di quello sincrono.
	- Si fanno meno aggiornamenti sul singolo elemento, ma più iterazioni complessive. Il motivo è dovuto al fatto che l'aggiornamento $i$-esimo del $k$-esimo stato è calcolato sulla base di più aggiornamenti dei singoli elementi.
# Convergenza
- Dato uno spazio degli stati $\mathbb{X}$ finito, se $\gamma\in(0,1)$ allora l'algoritmo garantisce che $V_k\to_{k\to\infty}V^o$.
- Se siamo in un problema episodico allora basta che $\gamma\in(0,1]$.
- Per la versione asincrona si richiede che gli stati siano campionati in modo uniforme, cioè che per $k\to\infty$ tutti gli stati siano equamente aggiornati.
Vedi dimostrazione slides 7 pagina 54.
# Complessità
Nella realtà questa soluzione non è molto praticabile da punto di vista computazionale perché richiede che il vettore della value function stia in memoria. È quindi fattibile quando l'insieme degli stati è finito e inoltre quando non è troppo grande.
##### Approssimato
Per risolvere questo problema si approssima la value iteration (non la value function) tramite una funzione (i.e., una rete neurale); in questo modo dobbiamo memorizzare solo i parametri (i.e., i pesi) della funzione.
L'algoritmo è il seguente:
- Si campionano $M$ stati $x_1,\cdots,x_M$ da $\mathbb{X}$. Non posso usarli tutto perché sennò il vettore della value function mi starebbe in memoria e non userei la value iteration approssimata.
- Per ogni $x_m$ campionato si usa l'operatore di Bellman (i.e., si fa la value iteration) per calcolare la prossima approssimazione della value function come $\hat{V}_{k+1}(x_m)=T(V_k)$, dove $V_k=V(x',w_k)$, cioè è una rete neurale nei parametri $w$ aggiornati all'interazione $k$-esima. Esplicitamente si calcola $\hat{V}_{k+1}(x_m)=\displaystyle\max_u\{r(x_m,u)+\gamma\sum_{x'\in\mathbb{X}}\phi(x'|x_m,u)V(x',w_k)\}$.
- Si aggiornano i pesi $w_k$ nei pesi $w_{k+1}$ nella direzione che minimizza $\displaystyle\sum_{m=1}^M[V(x_m,w_k)-\hat{V}_{k+1}(x_m)]^2$. I dati per eseguire questo aggiornamento, tramite ad esempio GD, sono dati dal passo precedente e sono $(x_1,\hat{V}_{k+1}(x_1)),\cdots,(x_M,\hat{V}_{k+1}(x_M))$. In pratica stiamo aggiorna i pesi in modo che l'approssima della value function sia più vicina al valore calcolato.