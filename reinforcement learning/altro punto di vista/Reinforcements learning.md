Nell’addestramento supervisionato c’è un teacher che in ogni situazione dice al modello se l'azione che sta facendo è giusta o sbagliata; questo nella pratica avviene grazie a una funzione da ottimizzare in base alle etichette dei samples.
In molte situazioni però etichettare tutto lo spazio di azioni che può svolgere un agente non è possibile. Pensiamo ad robot che deve spostare un bicchiere: non possiamo dire per ogni singolo movimento se quello sta convergendo al movimento ottimo (e nemmeno ci interessa che sia ottimo); possiamo però dare un feedback alla fine di ogni serie di azioni. Questo è il concetto alla base del reinformance learning: l'agente deve essere in grado di stabilire una relazione causa-effetto con l'ambiente in modo da interagire con esso, e in modo empirico arrivare a dei reward del teacher; il reward è qualitativo, indica quanto bene è stata fatta un'azione e non se questa è quell ottima.
# Exploration vs explotation
Uno dei trade-off da valutare nel reinforcement learning è quello dell'esplorazione e dello sfruttamento. Il concetto alla base è che non vogliamo usare il tempo per fare azioni che hanno un reward inferiore rispetto alle altre.
Per ottenere molti reward l'agente deve preferire azioni che nel passato hanno dato alti reward, ma per scoprire se ci sono azioni che danno reward più alti deve eseguire quelle azioni che non ha mai eseguito o che ha eseguito poco.
Un esempio per capire questa relazione tra exploration ed exloitation è il [k-armed bandit problem](k-armed%20bandit%20problem.md).
# Deep reiforcement learning
Alla base del riforcement learning ci sono i [Markov Decision Processes](Markov%20Decision%20Processes%20(MDPs).md), che, tramite le tecniche di [dinamic programming](reinforcement%20learning/altro%20punto%20di%20vista/Dinamic%20programming.md) o di [TD learning](Temporal%20Difference%20Learning.md) permettono di trovare le azioni migliori da eseguire in uno stato.
##### Pesi
Nella realtà per trovare la milgiore azioni dobbiamo lavorare per approssimazioni delle [value function](Markov%20Decision%20Processes%20(MDPs).md#Value%20function) e della [policy](Markov%20Decision%20Processes%20(MDPs).md#Policy).
Lavorare con le approssimazioni vuol dire che in alcuni stati avremo una stima migliore e ne penalizzeremo altri; questo vuol dire che avremo dei pesi associati agli stati che esprimeranno delle preferenze.
Questi pesi sono espressi da $\mu(s)$ e solitamente la norma è usare il tempo speso nello stato $s$; i pesi sono detti on-policy distribution.
##### Tecniche
- [Deep Q-learning](Deep%20Q-learning.md)
- [Policy gradient method](Policy%20gradient%20method.md)
- [Actor-Critic Methods](Actor-Critic%20Methods.md)