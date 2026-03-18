Si tratta di una delle soluzioni più semplici per la [mappatura](mapping.md) online in sistemi multi agente in ambienti statici (in ambienti dinamici richiede modifiche).
L'ambiente viene rappresentato tramite una griglia con una grana più o meno fine.
# Modello probabilistico
Vista l'imperfezione dei sensori, per definire una cella libera od occupata si usa un modello probabilistico: data una precisione $\epsilon\in[0,0.5]$, la cella $m_k$ è libera al tempo $t$ se la probabilità che sia occupata al tempo $t$ è minore di $\epsilon$ (i.e., $P_t(O_k)<\epsilon$) ed è occupata se abbiamo che la probabilità che sia occupata al tempo $t$ è maggiore di $1-\epsilon$ (i.e., $P_t(O_k)>1-\epsilon$); all'istanze iniziale $P_0(O_k)$ è definita secondo una qualche prior; se questa non è nota allora si sua $P_0(O_k)=0.5$.
# Aggiornamento
Supponendo di avere una sequenza $\{Z(i)\}_{i=1}^t$ indipendente (per semplicità) di osservazioni, vogliamo calcolare $P_t(O_k|Z(t))$.
Per risolvere questo problema si usa la regola di bayes e si calcola $P(O_k)=\tfrac{P(Z(t)|O_k)P_{t-1}(O_k)}{P(Z(t))}$, dove:
- $P(Z(t)|O_k)$ è la likelihood e dipende dai dati, cioè dal sensore.
- $P_{t-1}(O_k)$ è la prior. In questo caso è l'osservazione precedente.
- $P(Z(t))$ è calcolata, come dice la regola di bayes, tramite la total probability come $P(Z(t))=P(Z(t)|O_k)P_{t-1}(O_k)+P(Z(t)|F_k)P_{t-1}(F_k)$, dove $P_t(F_k)$ è la probabilità che la cella $k$-esima sia libera all'istante $t$.
- L'aggiornamento avviene poi trasformando $P_t(O_k)=\tfrac{P(Z(t)|O_k)P_{t-1}(O_k)}{P(Z(t))}$ come $P_t(O_k)=\tfrac{P_{t-1}(O_k)L_k(t)}{P_{t-1}(O_k)L_k(t)+1-P_{t-1}(O_k)}$, dove $L_k(t)$ è il likeliwood ratio $L_k(t)=\tfrac{P_t(Z(t)|O_k)}{P_t(Z(t)F_k)}$ ed esprime quando è verosimile che la $k$-esima cella sia occupata data l'osservazione $Z(t)$. Per intuizione vedi [questa immagine](aggiornamento%20griglia%20di%20occupazione.png).
# Conseguenze
- Vantaggi
	- È un algoritmo semplice ed efficiente.
	- È un buon compromesso per sistemi real-time con basse capacità computazionali.
- Limiti
	- Richiede memoria per ambienti 3D ad alta risoluzione, cioè dove la mappa ha una grana fine.
	- In ambienti dinamici servono dei cambiamenti. Ad esempio se una cella non è vista (e.g., per un ostacolo che prima non c'era) da un po può crescere gradualmente l’incertezza associata a essa.