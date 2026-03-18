I metodi Monte Carlo nel [reinforcement learning](Reinforcements%20learning.md) permettono di stimare le [value function](Markov%20Decision%20Processes%20(MDPs).md#Value%20function) $v_*$ e $q_*$ senza conoscere perfettamente l'ambiente definito da $p(s',r|s,a)$, come invece deve essere per i metodi basati sulla [dinamic programming](reinforcement%20learning/altro%20punto%20di%20vista/Dinamic%20programming.md).
Tramite questa conoscenza poi si può derivare la [policy ottima](Markov%20Decision%20Processes%20(MDPs).md#Policy%20ottima).
# Policy evaluation
Si vuole valutare in modo iterativo una data policy, cioè capire quanto questa sia buona. Vogliamo quindi, data la policy $\pi$, trovare in modo itreativo la stima $V$ della value function $v_\pi$.
![first visit MC prediction](first%20visit%20MC%20prediction.png)
# Policy iteration
Vogliamo applicare contemporaneamente la policy valuation e una versione Monte Carlo della [policy Improvement](reinforcement%20learning/altro%20punto%20di%20vista/Dinamic%20programming.md#Policy%20Improvement) (definita per dal dinamic programming).
Rispetto alla [policy iteration](reinforcement%20learning/altro%20punto%20di%20vista/Dinamic%20programming.md#Value%20Iteration) della dinamic programming non consociamo l'ambiente e quindi le possibili coppie $(s,a)$ stato-azione sono potenzialmente infinite e potrebbe non essere fattibile visitarle tutte. Tramite l'algortimo garantiamo l'exploration collezionando esperienze: si parte da una coppia $(s,a)$ che garantisce che la probabilità di raggiungere ogni $(s,a)$ non sia nulla; in questo modo avendo infinite iterazioni ogni coppia sarà visitate infinite volte.
![Monte Carlo Exploring Starts](Monte%20Carlo%20Exploring%20Starts.png)
# Problemi
Una regola di aggiornamento per la sitma di $V$ della state value function nelle tecniche Monte Carlo può essere $V(S_t)=V(S_t)+\alpha[G_t-V(S_t)]$.
Il problema è che dobbiamo aspettare fino alla fine dell'episodio per conoscere $G_t$. Per risolvere questo problema si usano tecniche di tipo [Temporal Difference Learning](Temporal%20Difference%20Learning.md).