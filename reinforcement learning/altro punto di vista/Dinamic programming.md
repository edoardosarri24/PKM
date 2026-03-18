Lo scopo della dinamic programming è quello di trasformare l'[equazione di Bellman](Markov%20Decision%20Processes%20(MDPs).md#Equazione%20di%20Bellman) in una regola di update per approssimare iterativamente le [value function](Markov%20Decision%20Processes%20(MDPs).md#Value%20function) $v_*$ e $q_*$. Queste saranno poi utilizzate per derivare la [policy ottima](Markov%20Decision%20Processes%20(MDPs).md#Policy%20ottima).
Sono algoritmi applicabili quando consociamo perfettamente l'ambiente, cioè quando abbiamo piena conoscenza di un [MDP](Markov%20Decision%20Processes%20(MDPs).md) definito dalla probabilità $p(s',r|s,a)$.
# Policy Evaluation
Si vuole valutare in modo iterativo una data policy, cioè capire quanto questa sia buona. Vogliamo quindi, data la policy $\pi$, trovare in modo itreativo la stima $V$ della value function $v_\pi$.
L'algoritmo è il seguente. Possiamo vedere che l'aggiornamento di $V$ avviene per tutti gli stati $s\in S$ si basa sull'[equazione di Bellman](Markov%20Decision%20Processes%20(MDPs).md#Equazione%20di%20Bellman), cioè abbiamoc he il prossimo aggiornamento è $v_k(s)=\displaystyle\sum_a\pi(a|s)\sum_{s'}\sum_rp(s',r|s,a)[r+\gamma v_k(s')],\forall s\in S$.
![iterative policy evaluation](iterative%20policy%20evaluation%20dinamic%20programming.png)
# Policy Improvement
Si vuole sapere in modo iterativo se, data una policy, esiste un'azione $a$ che nello stato $s$ sia migliore di quella definita da $\pi(s)$; questa informazione ci viene fornita dall'action value function $q_\pi(s,a)$.
- C'è un teorema che ci dice che se troviamo un'azione migliore di quella specificata dalla policy attuale allora la nuova policy è almeno buona q la vecchia: se $q_\pi(s,\pi'(s))\ge v_\pi(s)$ allora $v_{\pi'}\ge v_\pi$, dov $\pi'$ è la nuova policy.
##### Greedy Policy Improvement
Per trovare la nuova policy $\pi'$ in un dato stato $s$ si usa una scelta greedy: abbiamo che $\pi'(s)=\displaystyle\arg\max_a q_\pi(s,a)=\displaystyle\arg\max_a\sum_{s',r}p(s',r|s,a)[r+\gamma v_\pi(s')]$; osserviamo che dobbiamo conoscere $v_\pi(s)$.
Se la nuova policy non è milgiore della precedente, allora $v_\pi(s)=v_{\pi'}(s)$ e $v_{\pi'}(s)=a^*=\displaystyle\max_a\sum_{s',r}p(s',r|s,a)[r+\gamma v_\pi(s')]$, cioè la policy $\pi'$ soddisfa la [Bellman Optimality Equation](Markov%20Decision%20Processes%20(MDPs).md#Policy%20ottima).
- Siccome l'ambiente è totalmente definito e noto, il numero di policy è finito. Per questo motivo questa strategia converge a $\pi_*$ in un numero finito di passi.
![policy iteration](policy%20iteration%20dinamic%20programming.png)
# Value Iteration
Tramite la value iteration stiamo facendo contemporaneamente policy evaluation e policy iteration.
Si ottiene un algoritmo migliore della combinazione dei due sopra.
![value iteration](value%20iteration%20dinamic%20programming.png)