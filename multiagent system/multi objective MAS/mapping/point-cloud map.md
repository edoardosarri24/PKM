Si tratta di una soluzione per la [mappatura](mapping.md) online in sistemi multi agente in ambienti statici (in ambienti dinamici richiede modifiche).
Se guardiamo la tecnica [feature-based map](feature-based%20map.md), possiamo osservare che in raltà le features possono essere estratte a posteriori, cioè una volta che si ha la mappa. Con questa tecnica l'ambiente viene rappresentano come un insieme di punti $M=\{c_1,\cdots,c_k\}$ in uno spazio 3D (i.e., $c_i\in\mathbb{R}^3$).
# Allineamento
Nelle point-cloud map il problema principale è quello dell'allineamento: data l'attuale mappa $M(t-1)=\{c_1,\cdots,c_{k(t-1)}\},\ c_i\in\mathbb{R}^3$ e l'attuale rilevazione $Z(t)=\{z_1,\cdots,z_{d(t)}\},\ z_j\in\mathbb{R}^3$, si vuole creare $M(t)$ fondendo $Z(t)$ con $M(t-1)$ in modo che $Z(t)$ sia allineata a $M(t-1)$ tramite una trasformazione rigida (rototraslazione) definita da $T(z)=Rz+\tau$, dove $R$ è la matrice di rotazione e $\tau$ è il vettore di traslazione.
##### Iterative Closest Point (ICP)
È un algoritmo iterativo che risolvere il problema dell'allineamento.
Si inizia con una trasformazione rigida iniziale definita da $T_0(z)=R_0z+\tau_0$. Se abbiamo l'informazione dei sensori dell'agente riguardo allo spostamento allora possiamo usarli per tale inizializzazione (solitamente le abbiamo); altrimenti $R_0=I$ e $\tau_0=0$.
Si itera in modo che all'iterazione $l$-esima:
- Per ogni $z_d\in Z(t)$ si trova il punto $c_k(d)\in M(t-1)$ più vicino alla vecchia point cloud; vuol dire che si trova $z_d\in Z(t)$ in modo da soddisfare $\left\|T_l(z_d)-c_k(d)\right\|\le\left\|T_l(z_d)-c_k\right\|,\ \forall c_k\in M(t-1)$.
- Per ogni punto $c_k(d)$ della vecchia point cloud più vicino a $z_d$ , si calcola la successiva trasformazione rigida $T_{l+1}(z)$ minimizzando  $\displaystyle\sum_{d=1}^D\mu_d\left\|T_l(z_d)-c_k(d)\right\|^2$, dove $\mu_d$ è il peso associato alla $d$-esima rilevazione (e.g., potrebbe corrispondere a una confidenza su quanto la nuova rilevazione è associata a tale punto più vicino).
##### Osservazioni
- Dobbiamo iterare perché non possiamo essere sicuri che il punto più vicino a $z_d$ nella vecchia point cloud sia effettivamente quello che gli corrisponde, cioè che essi rappresentino la stessa rilevazione in due istanti di tempo diversi.
- La minimizzazione per trovare la successiva trasformazione rigida può essere risolta in forma chiusa tramite la [SVD](dimensionality%20reduction.md#Singular%20Value%20Decomposition%20(SVD)) (non da sapere).
# Conseguenze
- Vantaggi
	- Si ha un alto livello di dettaglio della mappa.
	- Permette di rappresentare strutture complesse.
- Limiti
	- Complessità di calcolo. Ogni volta che l'agente raccoglie informazione queste devono essere allineate con quelle già memorizzate.
	- È sensibile al rumore. È importante avere sensori accurati.
	- Può convergere a minimi locali. In questo caso un minimo locale si ha quando si allinea solamente una parte della point cloud.