Mentre con il [Q-learning](Deep%20Q-learning.md) vogliamo approssimare le action value fucntion per poi trovare una policy ottima, nelle tecniche policy gradient si ottimizzano direttamente le policy per massimizzare l'[expected return](Markov%20Decision%20Processes%20(MDPs).md#Expected%20return).
##### Idea
Stabiliamo una policy parametrizzata in $\theta$ oltre che nello stato, cioè abbiamo la distribuzione $\pi(a|s,\theta)$.
Quello che vogliamo è aggiornare $\theta$ per ottenere la miglior policy possibile; questo aggiornamento diventa $\theta_{t+1}=\theta_t+\alpha\nabla J(\theta$), dove $J$ è una funzione che esprime la bontà di $\theta$.
##### Stocasticità
Per aggiornare $\theta$ si userà [SGD](Gradient%20descent.md#Stocastic%20Gradient%20Descent%20(SGD)) e quindi la policy sarà stocastica.
È importante che la policy resti stocastica, cioè che non diventi mai deterministica producendo o 0 o 1 per ogni coppia $(s,\theta)$ perché altrimenti non si continua la fase di exploration. Inoltre in alcuni ambienti la policy ottimale potrebbe essere proprio una policy stocastica e non deterministica.
##### Output
Come per una normale classificazione, se lo spazio delle azioni è piccolo è possibile costruire $h(s,a,\theta)$ in modo che restituisca un'array di preferenze di tutte le azioni. Se poi usiamo una Softmax, allora abbiamo i logits di queste azioni.
# Policy gradient theorem
Per misurare la bontà di $\theta$ si usa una la value function nello stato iniziale, cioè $J(\theta)=v_{\pi_\theta}(s_0)$.
A questo punto per trovare $\theta_*$ massimo si fa un gradient ascent e si arriva all'[equazione di Bellman](Markov%20Decision%20Processes%20(MDPs).md#Equazione%20di%20Bellman) $\theta_*=\displaystyle\max_\theta\sum_a\pi_\theta(a|s_0)\sum_{s'}\sum_rp(s',r|s,a)[r+\gamma v_{\pi_\theta}(s')],\forall s\in S$.
Il problema è che fare il gradiente di tutto questo è troppo complesso.
##### Teorema
Il teorema del policy gradient ci dice che $\nabla J(\theta)\propto\displaystyle\sum_s\mu(s)\sum_aq_\pi(s,a)\nabla\pi(a|s,\theta)$, dove $\mu$ è la [on policy distribution](Reinforcements%20learning.md#Deep%20reiforcement%20learning).
Questo ci permette di spostare il gradiente solo alla policy e di ottenere $\theta_{t+1}=\theta_t+\alpha\sum_a\hat{q}(S_t,a|\text{w})\nabla\pi(a|S_t,\theta)$.
La costante $\alpha$ ci permette di pareggiare la proporzionalità.
##### REINFORCE: Monte Carlo Policy Gradient
Questo algoritmo è molto suscettibile alla costante $\alpha$.
![Reinforce algorithm](Reinforce%20algorithm.png)
