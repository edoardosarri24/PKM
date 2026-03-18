Vediamo i problemi principale nel [coordinamento](coordinazione%20in%20MAS.md) di [sistemi multi agente](agenti.md). Consideriamo $N$ agenti che seguono la [single integrator dynamic](single%20integrator%20dynamic.md).
# Formazione
Il problema della formazione vuole portare gli agenti in uno stato che segue una precisa forma geometrica.
Questo si può esprimere sotto forma della distanza tra gli agenti come $\|x_i-x_j\|=\delta_{ij},\forall i,j$, dove $\delta_{ij}$ è la distanza desiderata tra l'agente $i$ e $j$.
Il goal quindi è rappresentato dall'insieme di stati $X_G=\{x=[x_1,\cdots,x_n]:\|x_i-x_j\|=\delta_{ij},\forall i,j\text{ con }j\in N_i\}$.
##### Soluzione APF
Siccome il goal può essere espresso in funzione di [coppie di vicini](APF%20per%20il%20coordinamento.md#Must), allora possiamo usare l'[APF](APF%20per%20il%20coordinamento.md) per risolvere questo problema.
Il potenziale di formazione sarà definito come $J_{for}(x)=\displaystyle\sum_{\{i,j\}\in E}J_{ij}(x_i,x_j)$, dove ad esempio (ci sono molti altri modi per definirlo, l'importante è che sia una funzione con un minimo nel goal) $J_{ij}(x_i,x_j)=\tfrac{1}{4}(||x_i-x_j||^2-\delta_{ij}^2)^2$; il problema di [questa funzione](example%20of%20potential%20for%20formation.png) è che non è convessa e quindi potrebbe avere dei minimi locali dovuti alla somma di $N$ termini con parametri diversi.
##### Configurazione geometrica
Per definire la forma geometrica che il sistema deve assumere dobbiamo definire un certo numero di distanze (i.e., di collegamenti, cioè di vicini). Nel fare questo possiamo creare varie configurazioni:
- Flessibile
	Possiamo deformare la configurazione definita (e.g., da quadrato a rombo) senza alterare le distanze tra nodi.
- Rigida
	L'unica modifica che possiamo fare è una rototraslazione; non è possibile modificare la forma.
- Localmente rigida
	La forma geometrica è fissa e non può essere modificata, ma abbiamo più configurazioni che portano a tale forma.
# Sincronizzazione
La sincronizzazione è una specializzazione della [formazione](multiagent%20system/multi%20objective%20MAS/coordination/task.md#Formazione) dove lo stato $x_i(t)$ dell'$i$-esimo agente deve convergere a un valore comune $\bar{x}$ (qualunque) per tutti gli agenti, cioè $\displaystyle\lim_{t\to+\infty}x_i(t)=\bar{x},\forall i$.
Il goal in questo caso diventa l'insieme degli stati $X_G=\{x=[x_1,\cdots,x_n]:x_i=x_j=\bar{x},\forall i,j\}$.
Per risolvere il problema possiamo usare la [legge del consenso](legge%20del%20consenso.md).
# Media distribuita
Il calcolo della media distribuita è una specializzazione della [sincronizzazione](multiagent%20system/multi%20objective%20MAS/coordination/task.md#Sincronizzazione), dove gli agenti devono raggiungere uno stato comune $\bar{x}$ dato dalla media dei loro stati iniziali $x_i(0)$.
Il goal in questo contesto diventa l'insieme degli stati definito da $X_G=\displaystyle\{x=[x_1,\cdots,x_n]:x_i=\tfrac{1}{N}\sum_{i=1}^Nx_i(0),\forall i,j\}$.
##### Sensor network
Uno degli esempi di applicazione è quella in cui ogni agente $i$ è dotato di un sensore che acquisisce una misura $\theta_i$ (nel caso semplice uno scalare ma potrebbe essere qualunque cosa, e.g., una mappa), capacità computazionali e comunicazione con i suoi vicini $N_i$. Gli agenti vicini si scambiano i valori e aggiornano il proprio stato interno $x_i(t)$ con l'obiettivo che $\displaystyle\lim_{t\to\infty}x_i(t)=\bar{\theta}$.
Questo problema può essere modello in termini di un sistema multi agente come:
- Ogni agente inizializza il proprio stato come $x_i(0)=\theta_i$.
- In ogni iterazione $t$ lo stato si aggiorna come $x_i(t+1)=\pi_i(y_i(t))$, dove $y_i(t)=\begin{bmatrix}x_i(t)\\x_j(t),j\in N_i\end{bmatrix}$ è l'[observation model](modello.md#Observation%20model).
# Ottimizzazione distribuita
Nel [federated learning](AIIoT.md#Federated%20learning) e [multiagent learning](multiagent%20learning.md) abbiamo agenti che possiedono dei dati e capacità computazionali per addestrare modelli di ML; ogni agente ha una propria funziona da ottimizzare $J_i$ sconosciuta agli altri; vogliamo evitare di condividere tutti questi dati e addestrare un modello migliore in modo centralizzato (ad esempio per problemi di privacy o larghezza di banda); oltre alle funzioni locali vogliamo ottimizzare anche una funzione $J$ globale definita da $J(w)=\sum_{i=1}^NJ_i(w)$. Se lo stato degli agenti sono i relativi pesi $w$ allora il goal diventa $X_G=\displaystyle\{x=[x_1,\cdots,x_n]:x_i=\arg\min_{w\in X_A}J(w),\forall i\}$.