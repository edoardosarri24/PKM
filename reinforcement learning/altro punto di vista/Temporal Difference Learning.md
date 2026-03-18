Il Temporal Difference Learning (TD) nel [reinforcement learning](Reinforcements%20learning.md) ha lo stesso scopo dei metodi [Monte Carlo](Metodi%20Monte%20Carlo.md): si vuole arrivare alla policy ottima a partire da una stima delle [value function](Markov%20Decision%20Processes%20(MDPs).md#Value%20function) basandoci sull'esperienza. In questi approcci infatti non si conosce l'ambiente definito da $p(s',r|s,a)$ in modo perfetto, come invece deve essere per le tecniche basate sulla [dinamic programming](reinforcement%20learning/altro%20punto%20di%20vista/Dinamic%20programming.md).
# Policy evaluation
Si vuole valutare in modo iterativo una data policy, cioè capire quanto questa sia buona. Vogliamo quindi, data la policy $\pi$, trovare in modo iterativo la stima $V$ della value function $v_\pi$.
Come possiamo notare dell'algoritmo il passo di aggiornamento di $V$ non si basa su $G_t$ come succede per l'aggiornamento nelle tecniche basate sul metodo di [Monte Carlo](Metodi%20Monte%20Carlo.md#Problemi); questo permette di non aspettare fino alla fine dell'episodio, ma di poter eseguire subito l'aggiornamento. L'algoritmo per questo motivo è detto one-step lookaheadad.
![one-look ahead](TD%20one-look%20ahead.png).
# Policy iteration
Vogliamo applicare contemporaneamente la policy valuation e una versione TD della [policy Improvement](reinforcement%20learning/altro%20punto%20di%20vista/Dinamic%20programming.md#Policy%20Improvement) (definita per dal dinamic programming) per cercare di imparare una policy.
Ci serve un compromesso tra exploration ed explotation tramite algoritmi greedy. Con questi algoritmi vogliamo trovare in modo iterativo una stima $Q$ di $q_*$.
##### Tipi
Un algoritmo può essere di due tipi:
- On policy
	Si parla di on policy quando la policy viene migliorata tramite episodi generati dalla stessa policy.
- Off policy
	Si parla di algoritmi off policy quando la policy viene migliorata tramite episodi generati da un'altra policy, che viene poi aggiornata in differita.
##### SARSA
L'algoritmo SARSA deriva dalla regola di aggiornamento $Q(S_t,A_t)=Q(S_t,A_t)+\alpha[R_{t+1}+\gamma Q(S_{t+1},A_{t+1})-Q(S_t,A_t)]$.
L'[algoritmo](SARSA.png) è detto on policy perché contemporaneamente stima $Q$ e la utilizza per campionare episodi.
##### Q-Learning
L'algoritmo Q-learning deriva dalla regola di aggiornamento $Q(S_t,A_t)=Q(S_t,A_t)+\alpha[R_{t+1}+\gamma\max_aQ(S_{t+1},a)-Q(S_t,A_t)]$; in pratica stiamo prendendo l'azione che massimizza la reward pr ogni stato.
L'[algoritmo](Q-learning%20algorithm.png) utilizza $Q$ per stimare direttamente $\pi_*$ indipendentemente dalla policy $\pi$ seguita.