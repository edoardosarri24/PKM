Nel [reiforcement learning](reiforcement%20learning.md) la policy definisce l'azione che l'agente deve eseguire dato uno stato. Si tratta quindi di un vettore della dimensionalità $\left\|\mathbb{X}\right\|$.
# Deterministica
Se è definita solo sulla base dello stato allora si parla di policy deterministica: formalmente è una funzione $\pi:\mathbb{X}\to\mathbb{U}$.
Anche in questo caso il modello è stocastico per via dell [Markov transition density](reinforcement%20learning/markov%20decision%20process.md#Markov%20transition%20density).
# Policy ottima
Tramite l'[equazione di Bellman](equazione%20di%20Bellman.md) possiamo valutare una policy, ma vogliamo capire quale è la policy ottima, cioè quella che massimizza l'[expeted return](reward.md#Expected%20return); questo deve essere indipendente dal singolo esperimento, visto che ci potranno essere policy che nel singolo esperimento ottengono valori migliori.
Vogliamo trovare quindi $\pi^o$ tale che $V_{\pi^o}(x)=V^o(x)\ge V_\pi(x),\ \forall \pi,x\in\mathbb{X}$: la [value function](value%20function.md) della policy ottima associa a ogni stato il migliore valore possibile.
##### Equazione di ottimalità di Bellman
Possiamo definire la policy ottima come $\pi^o=\displaystyle\arg\max_u\{r(x,u)+\gamma\sum_{x'\in\mathbb{X}}\phi(x'|x,u)V^o(x')\}$. L'equazione di ottimalità di Bellman mi definisce la [value function](value%20function.md) ottima (con cui posso trovare la policy ottima) come $V^o(x)=\displaystyle\max_u\{r(x,u)+\gamma\sum_{x'\in\mathbb{X}}\phi(x'|x,u)V^o(x')\}$.
Si tratta di un'equazione non lineare (a causa il massimo) nelle incognite $V^o(x)$ molto complessa da risolvere in modo esatto. Per questo motivo si usano soluzioni approssimate: la [value iteration](value%20iteration.md) per trovare la value function ottima e la [policy iteration](policy%20iteration.md) per trovare direttamente la policy ottima senza passare dalla value function ottima.
##### Derivazione
Vedi slides 7 pagina bianca dopo la slide 35.