La policy iteration è un algoritmo model-based (richiede la conoscenza della [markov transition density](reinforcement%20learning/markov%20decision%20process.md#Markov%20transition%20density) e della funzione di [reward](reward.md)) e on-policy per approssimare la [policy ottima](reinforcement%20learning/policy.md#Policy%20ottima) quando l'[equazione di ottimalità di Bellman](reinforcement%20learning/policy.md#Equazione%20di%20ottimalità%20di%20Bellman) non è risolvibile in forma chiusa.
A partire da una policy iniziale $\pi_0$, si calcola una sequenza di policy che convergono alla policy ottima $\pi^o$.
Si definsce on-policy perché approssima la policy usando la policy stessa per definire l'azione da prendere.
# Algoritmo
L'algortimo è detto actor-critic: il critoc valuta la policy; l'actor fa qualcosa per migliorarla.
L'algoritmo è il seguente:
- Si parte da una policy iniziale $\pi_0$. Queste può essere scelta a caso oppure generata da una qualche euristica. Si itera poi su $k$ alternando due operazioni:
	- Policy evaluation
		Calcolo la [value function](value%20function.md) per tutti gli stati $x\in\mathbb{X}$ secondo la policy $\pi_k$ usando l'[equazione di Bellman](equazione%20di%20Bellman.md).
		In questo abbiamo due problemi: gli stati potrebbero non entrare in memoria; se non conosciamo il modello del mondo, cioè la Markov transition density e la funzione di reward, allora si usa la [model-free policy evaluation](model-free%20policy%20evaluation.md).
	- Policy improvement
		Aggiorno la policy per ogni stato $x\in\mathbb{X}$ tramite $\pi_{k+1}(x)=\displaystyle\arg\max_uQ(x,u)$, dove $Q$ è la [action-value function](action-value%20function.md).
# Convergenza
- Dato uno spazio degli stati $\mathbb{X}$ finito, allora la sequenza di policy generate dalla policy iteration garantiscono un miglioramento monotono, cioè abbiamo che $V_{\pi_{k+1}}(x)\ge V_{\pi_k}(x)$.
- Se anche lo spazio delle azioni $\mathbb{U}$ è finito si converge alla [policy ottima](reinforcement%20learning/policy.md#Policy%20ottima) $\pi^o$ in un numero finito di passi.