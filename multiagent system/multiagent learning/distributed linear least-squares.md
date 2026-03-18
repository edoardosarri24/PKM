Il problema di [regressione lineare ai minimi quadrati](regressione%20lineare.md#Linear%20least-square) è noto e facile nel caso centralizzato; nel caso [distributito](multiagent%20learning.md) lo formalizziamo adesso.
# Idea
Nel caso centralizzato i pesi ottimi possono essere trovati in forma chiusa come $w^o=\Omega^{-1}q$, dove $\Omega=\displaystyle\sum_{k=1}^K\phi(x_k)\phi(x_k)'$ e $q=\displaystyle\sum_{k=1}^K\phi(x_k)y_k$, se la loss considerata è $L(w)=\displaystyle\sum_{k=1}^K(y_k-w^T\phi(x_k))^2$.
Tramite i propri dati l'agente $i$-esimo può trovare $\Omega_i=\displaystyle\sum_{k\in K_i}\phi(x_k)\phi(x_k)'$ e $q_i=\displaystyle\sum_{k\in K_i}\phi(x_k)y_k$. L'idea è di usare l'algoritmo del [consenso (TD)](consenso%20a%20tempo%20discreto.md) per ottenere $\Omega=\displaystyle\sum_{i=1}^N\Omega_i$ e $q=\displaystyle\sum_{i=1}^Nq_i$.
# Algoritmo del consenso
L'algoritmo per ottenere quanto sopra è iterativo ed è il seguente:
- Si inizializza ogni agente $i$ con $\Omega_i(0)=\displaystyle\sum_{k\in K_i}\phi(x_k)\phi(x_k)'$ e $q_i(0)=\displaystyle\sum_{k\in K_i}\phi(x_k)y_k$.
- In ogni istante di tempo $t$:
	- Si pone $\Omega_i(t+1)=\pi_{ii}\Omega_i(t)+\sum_{j\in N_i}\pi_{ij}\Omega_j(t)$.
	- Si pone $q_i(t+1)=\pi_{ii}q_i(t)+\sum_{j\in N_i}\pi_{ij}q_j(t)$.
	- Si pone $w_i(t+1)=\Omega_i(t+1)^{-1}q_i(t+1)$.
##### Osservazioni
Siccome siamo nella legge del consenso, devono valere [queste restrizioni](consenso%20a%20tempo%20discreto.md#Definizione) per i pesi $\pi_{ii}$.
##### Convergenza
Supponendo un grafo del sistema multi agente connesso e indiretto e che la [matrice del consenso](consenso%20a%20tempo%20discreto.md#Matrice%20del%20consenso) sia simmetrica (i.e., doppiamente stocastica), allora questo algoritmo converge alla media dei valori iniziali, cioè si ha:
- $\displaystyle\lim_{t\to\infty}\Omega_i(t)=\tfrac{1}{N}\sum_{i=1}^N\Omega_i(0)=\tfrac{1}{N}\sum_{i=1}^N\Omega_i=\tfrac{1}{N}\Omega$.
- $\displaystyle\lim_{t\to\infty}q_i(t)=\tfrac{1}{N}\sum_{i=1}^Nq_i(0)=\tfrac{1}{N}\sum_{i=1}^Nq_i=\tfrac{1}{N}q$.
- $\displaystyle\lim_{t\to\infty}w_i(t)=\lim_{t\to\infty}\Omega_i(t)^{-1}q_i(t)=(\tfrac{1}{N}\Omega)^{-1}\tfrac{1}{N}q=\Omega^{-1}q=w^o$. Questo vuol dire che per la convergenza ai pesi ottimi il sistema non deve conoscere il numero di agenti $N$.
# Regolarizzatore di Tikhonov
Se al problema aggiungiamo un regolarizzatore di tipo di Tikhonov (i.e., [Ridge](Regolarizzazione.md#Ridge) regularization).
La funzione di loss, che oltre al rischio empirico ha anche il regolarizzatore, diventa $J_{EMP}(w)=\displaystyle\sum_{k=1}^K(y_k-w'\phi(x_k))^2+\rho\left\|w\right\|^2$. In forma chiusa i pesi possono essere definiti come $w^o=(\Omega+\rho I)^{-1}q$: la matrice da invertire $\Omega+\rho I$ diventa meno mal condizionata e otteniamo un calcolo più stabile.
##### Algortimo
Rispetto all'algoritmo nel caso precedente dobbiamo solo modificare l'aggiornamento dei pesi: $w_i(t+1)=[\Omega_i(t+1)+\tfrac{\rho}{N}I]^{-1}q_i(t+1)$.
Lo svantaggio è che in questo caso ogni agente deve sapere il numero di agenti $N$ coinvolti.