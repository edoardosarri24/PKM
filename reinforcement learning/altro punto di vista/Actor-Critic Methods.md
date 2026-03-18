Con i policy gradient method avevamo detto che tramite il [policy gradient theorem](Policy%20gradient%20method.md#Policy%20gradient%20theorem) potevamo ottenere $\nabla J(\theta)\propto\displaystyle\sum_s\mu(s)\sum_aq_\pi(s,a)\nabla\pi(a|s,\theta)$.
##### Baseline
Nei metodi Actor-Critic (AC) possimao introdurre una baseline e paragonarla con l'action value function, cioè $\nabla J(\theta)\propto\displaystyle\sum_s\mu(s)\sum_a(q_\pi(s,a)-b(s))\nabla\pi(a|s,\theta)$.
La basleine function può essere qualunqe cosa, anche una costante, ma è importante che non sia in funzione dell'azione. Solitamente si usa come baseline la state value function $\hat{v}(S_t|\textbf{w})$.
##### Conseguenze
Introducendo questa baseline si permette migliora la varianza dell'algoritmo [REINFORCE](Policy%20gradient%20method.md#REINFORCE%20Monte%20Carlo%20Policy%20Gradient), si converge più velocemente e si ha un learning rate $\alpha$ più grande.
Lo svantaggio però è che servono due reti, una che ottimizza $\theta$ e una che approssima la state value function $v$.
##### AC method
Cioè che caratterizza i metodi AC è l'utilizzo di tecniche di [TD Learning](Temporal%20Difference%20Learning.md) per non dover aspettare tutti i passi in avanti (G nell'[algoritmo reinforce con la baseline](reinforce%20con%20baseline.png)) per calcolare l'errore, ma si deve solo guardare un passo avanti.
![one step actor critic](one%20step%20actor%20critic.png)