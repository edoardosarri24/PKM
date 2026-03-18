Siamo nella situazione in cui abbiamo lo spazio delle azioni è piccolo, ma lo spazio degli stati è enorme.
Quello che vogliamo fare è arrivare a un'azione definita da un'output categorico (cioè determinar la miglior azione $a$) dato un input; volgiamo quindi arrivare a $q_*(s,a)$.
Nel deep Q-Learning usiamo una [CNN](Convolutional%20neural%20network%20(CNN).md) che approssimi $q_*$ con $\hat{q}(s,a|\textbf{w})$. I pesi $\textbf{w}$ sono aggiornati iterativamente tramite la back propagation sugli episodi stato/azione (cioè sulla traiettoria) con $w_{t+1}=w_t+\alpha[R_{t+1}+\gamma\displaystyle\max_a\hat{q}(S_{t+1},a|\textbf{w}_t)-\hat{q}(S_t,A_t|\textbf{w}_t)]\nabla\hat{q}(S_t,A_t|\textbf{w}_t)$.
##### Problemi
I problemi principali sono due:
- Abbiamo un problema di regressione con un target che si aggiorna. Questo provoca instabilità.
- L'aggiornamento è correlato con la traiettoria.
##### Target Networks
La soluzione al primo problema, cioè quello di stabilizzare l'aggiornamento. è data da usare due reti, una che genera esperienze e una che aggiorna i pesi; ogni $C$ episodi viene aggiornata anche la rete che genera episodi. Questo è fatto tramite una rete $\tilde{q}$ con $w_{t+1}=w_t+\alpha[R_{t+1}+\gamma\displaystyle\max_a\hat{q}(S_{t+1},a|\textbf{w}_t)-\hat{q}(S_t,A_t|\textbf{w}_t)]\nabla\tilde{q}(S_t,A_t|\textbf{w}_t)$, che prende il nome di momentum update,
##### Experience Replay
Vogliamo decorelare l'aggiornamento dalla traiettoria.
Invece che aggiornare subito basandoci sulla traiettoria, salviamo la tupla $(S_t,A_t,R_{t+1},S_{t+1})$ in una replay memory; l'aggiornamento vero e proprio viene fatto periodicamente campionando in modo uniforme dalla dalla replay memory.