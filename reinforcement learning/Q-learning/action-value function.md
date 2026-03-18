L'action-value function, data una [policy](reinforcement%20learning/policy.md) $\pi$ e la sua [value function](value%20function.md), quantifica la bontà di un'azione eseguita in uno stato e si definisce come $Q_{\pi}(x,u)=\displaystyle r(x,u)+\gamma\sum_{x'\in\mathbb{X}}\phi(x'|x,u)V_{\pi}(x')$.
Se la value function è un vettore con un valore di bontà per ogni stato, possiamo vedere l'action-value function come una matrice $Q\in\mathbb{R}^{|\mathbb{X}|\times|\mathbb{U}|}$ sulle coppie <stato,azione> detta Q-table.
# action-value function ottima
L'action-value function ottima si definisce a partire dalla value function ottima come $Q^o(x,u)=\displaystyle r(x,u)+\gamma\sum_{x'\in\mathbb{X}}\phi(x'|x,u)V^o(x')$.
##### Soluzione
Se siamo in grado di trovare l'action-valute function ottima allora abbiamo risolto il problema del reiforcement learning. Possiamo infatti derivare la value function ottima come $V^o(x)=\max_uQ^o(x,u)$ e la policy ottima come $\pi^o(x)=\arg\max_uQ^o(x,u)$.
##### Equazione di ottimalità di Bellman
Sostituendo $V^o(x)=\max_uQ^o(x,u)$ nell'equazione della value function ottima possiamo ottenere l'equazione di ottimalità di Bellman in relazione all'action-value function come $Q^o(x,u)=\displaystyle r(x,u)+\gamma\sum_{x'\in\mathbb{X}}\phi(x'|x,u)\max_{u'}Q^o(x',u')$, dove $x'$ è il prossimo stato e $u'$ è l'azione decisa nel prossimo stato.