Risolve il problema del [Q-learning](Q-learning.md) nelle situazioni in cui la Q-table non sta in memoria o uno tra $\mathbb{X}$ e $\mathbb{U}$ è continua.
In questi casi l'[action-value function](action-value%20function.md) è approssimata con una funzione parametrica (i.e., neural network), cioè $Q_t(x,u)=Q(x,u,w(t))$ e l'obiettivo è modificare i pesi $w(t)$ in modo iterativo per ottenere approssimazioni che convergono a $Q^o$.
# Algortimo
L'idea è di modificare i pesi in direzione dell'[approssimazione](Q-learning.md#Derivazione) $\hat{Q}_{t+1}(x(t),u(t))$:
- Si calcola una stima campionaria di $Q_t$ come $\hat{Q}_{t+1}(x(t),u(t))=r(t)+\gamma\max_{u'}Q(x(t+1),u',w(t))$.
- Si cambiano i pesi $w(t)$ in $w(t+1)$ in modo da minimizzare la distanza $[Q(x(t),u(t),w(t))-\hat{Q}_{t+1}(x(t),u(t))]^2$; si sta quindi cercando di minimizzare una loss del tipo $L(w)=\tfrac{1}{2}[\hat{Q}_{t+1}-Q(x(t),u(t),w)]^2$. Se osserviamo la versione [tabellare del Q-learning](Q-learning.md) allora è come se stessimo modificando il valore di ogni coppia <stato,azione>.
# Problemi
Ci sono due problemi principali che sono risolti con i relativi modi.
##### Correlazione
Quando siamo in uno stato il prossimo stato non può essere uno qualunque, ma comunque dipende da quello attuale; questo porta a una correlazione tra sequenze di stati.
Si risolve con l'experience replay: si raccoglie un dataset $D_t=\{(x(\tau),u(\tau),r(\tau),x(\tau+1)), \tau=0,\cdots,t\}$; ogni istante di tempo $t$ si campiona $M$ elementi dall'insieme; si calcola $\hat{Q}_{t+1,m}=r_m+\gamma\max_{u'}Q(x_m',u',\hat{w}(t))$; si aggiornano i pesi $w(t)$ in $w(t+1)$ muovendoci nella direzione che minimizza $\displaystyle\sum_{m=1}^M[Q(x(t),u(t),w(t))-\hat{Q}_{t+1,m}]^2$.
##### Dipendenza
Calcoliamo il nuovo vettori di pesi $w(t+1)$ muovendoci in direzione del target $\hat{Q}_{t+1}$, che però è calcolata usando la stessa rete che stiamo aggiornando. Questa ricorsione porta a instabilità: se la rete porta un errore allora questo si propaga durante le varie iterazioni.
Si risolve con la fixed target: l'approssimazione $\hat{Q}_{t+1}$ è calcolata con una rete i cui pesi sono $\hat{w}(t)$; questi pesi sono aggiornati al termine di un qualche periodo.