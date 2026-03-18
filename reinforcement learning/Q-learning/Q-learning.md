Il Q-learning è un algoritmo off-policy, [model-free](model-free%20policy%20evaluation.md), asincrono e real-time (aggiornamento subito dopo aver ricevuto un reward) che vuole approssimare l'[action-value function ottima](action-value%20function.md#action-value%20function%20ottima); se la Q-table non sta in memoria o uno tra $\mathbb{X}$ e $\mathbb{U}$ è continua allora si deve [approssimare](Q-table%20approximation.md).
Si definisce off-policy perché sto cercando una policy ottima, ma si utilizza una policy diversa per decidere la prossima azione da eseguire.
# Algoritmo
Per ogni istante di tempo $t$:
- Seguendo una policy random $\pi_r$, si stabilisce l'azione $u(t)$ e si ottengono i dati $(x(t), u(t), r(t), x(t+1))$.
- Si aggiorna il valore dell'approssimazione dell'action-value function ottima per la coppia $(x(t), u(t))$ come $Q_{t+1}(x(t),u(t))=Q_t(x(t),u(t))+\alpha(t)[r(t)+\gamma\displaystyle\max_{u'}Q_t(x(t+1),u')-Q_t(x(t),u(t))]$, basandoci quindi sulle [temporal difference](model-free%20policy%20evaluation.md#Temporal%20difference%20learning).
- Si lasciano invariati gli altri valori, cioè $Q_{t+1}(x,u)=Q_{t}(x,u),\ \forall(x,u)\ne(x(t),u(t))$.
##### Derivazione
Supponiamo di essere all'istante $t$ e di avere $x(t)$, $u(t)$, $r(t)$ e $x(t+1)$ e l'approssimazione $t$-esima $Q_t$ di $Q^o$.
Posso calcolare una stima campionaria di $Q_t$ su $x(t)$ e $u(t)$ come $\hat{Q}_{t+1}(x(t),u(t))=r(t)+\gamma\max_{u'}Q_t(x(t+1),u')$, cioè prendiamo il reward attuale (che lo osserviamo) e la stima dell'action-value function del prossimo stato (che abbiamo perché il prossimo stato lo vediamo e l'approssimazione ce l'abbiamo).
Usare questo valore come prossima approssimazione sarebbe sbagliato, perché si tratta del valore ottenuto da un singolo esperimento. Quello che si fa è quindi una media pesata del valore che avevamo prima e di tale valore, cioè $Q_{t+1}(x(t),u(t))=[1-\alpha(t)]Q_t(x(t),u(t))+\alpha(t)\hat{Q}_{t+1}(x(t),u(t))$.
Il learning rate $\alpha(t)\in[0,1]$ quantifica il peso che diamo al nuovo valore calcolato. È in funzione del tempo: all'inizio è alto e poi si abbassa; all'inizio il valore attuale è molto importante perché non abbiamo fatto esperimenti.
# Convergenza
L'algortimo converge, cioè $Q_t(x,u)\to_{t\to\infty}Q^o(x,u)$ con probabilità 1 supponendo che:
- Tutte le coppie <stato,azione> siano visitate infinite volte.
	Questo è garantito se abbiamo una policy random $\pi_r$ con cui scegliamo la prossima azione che garantisce una buona [exploration](#Exploration).
- $\alpha(t)\to0$ non troppo veloce.
	Formalmente $\alpha$ deve soddisfare le condizioni di Robbins-Monrò: il learning rate deve tendere a 0, ma non troppo velocemente: rispettivamente $\displaystyle\sum_{t=0}^\infty\alpha(t)^2=0$ e $\displaystyle\sum_{t=0}^\infty\alpha(t)=\infty$.
	Una scelta che soddisfa queste condizione è $\alpha(t)=\tfrac{c_1}{c_2+t}$.
# Exploration
Per garantire che ogni coppia <stato,azione> sia visita infinite volte si definisce la policy random $\pi_r$ in modo stocastico: $\pi_r(u|x(t))$, con $\pi_r(u|x(t))>0,\ \forall u\in\mathbb{U}$.
Ci sono due soluzioni opposte per scegliere questa policy:
- Una soluzione che funziona in teoria è avere una policy randomica, cioè una policy la cui distribuzione sulla prossima azione è uniforme; nella pratica questo non funziona perché non abbiamo tempo infinito per ogni esperimento.
- La soluzione opposta è invece avere una policy greedy, cioè dove $u(t)=\displaystyle\arg\max_uQ_t(x(t),u)$: dato lo stato si sceglie l'azione che massimizza il valore dell'action-value function.
##### $\epsilon$-greedy policy
La policy random $\pi_r$ è tale per cui con probabilità $1-\epsilon$ si sceglie l'azione greedy, con probabilità $\epsilon$ si sceglie l'azione random.
Il parametro $\epsilon$ definisce la randomicità della policy: se è 1 allora la policy è totalmente randomica; se è 0 allora abbiamo la policy deterministica (greedy). Solitamente $\epsilon$ varia in funzione del tempo; un esempio è $\epsilon(t)=\tfrac{1}{t}$, che garantisce all'inizio una grande esplorazione e poi più explotation.
##### Boltzmann exploration
La policy random $\pi_r$ è tale per cui si usa una softmax con una temperatura $K$: $\pi_r(u|x(t))=\tfrac{e^{Q(x(t),u)/K}}{\sum_ue^{Q(x(t),u)/K}}$.
Il parametro $K$ definisce la randomicità della policy: se $K=0$ allora abbiamo la policy greedy; se $K\to\infty$ abbiamo la policy random.
# Vantaggi
Imparare questa action-value function ottima, rispetto alla [value function ottima](value%20iteration.md) ha due vantaggi:
- Stiamo calcolando il valore atteso del massimo e non il massimo del valore atteso.
- Se ho la $Q$ ottima allora ho anche la policy ottima in un contesto [model-free](model-free%20policy%20evaluation.md). Con la $V$ ottima invece devo conoscere comunque il modello del mondo.