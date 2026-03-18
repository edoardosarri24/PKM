Si tratta di un [obiettivo](multi%20obiettivo%20in%20MAS.md) sempre utile in un sistema multi agente.
Il grafo a tempo variante $G(t)=(N,E(t))$ che definisce il sistema multi agente deve essere sempre connesso per ogni $t>0$.
Si deve supporre che $G(0)$ sia connesso, cioè che inizialmente nessun agente sia isolato; se così fosse allora questo agente potrebbe trovare la connessione con gli altri solo per caso ma comunque non avremmo controllo su cosa succede.
# Vicini
Nel modello più usato, l'$i$-esimo agente ottiene le informazioni solo dagli agenti che sono più vicini di un certo raggio di connettività $\rho$, cioè $j\in N_i\Leftrightarrow||x_i-x_j||\le\rho$.
Siccome la distanza tra robot è a tempo variante (i.e., cambia nel tempo) allora l'insieme dei vicini dell'$i$-esimo agente è definito come $N_i(t)=\{j\ne i:||x_i(t)-x_j(t)||\le\rho\}$.
# Implementazione
Per l'implementazione ci servono due cose:
- Insieme di archi.
- Forza il grafo composto da questi archi a restare connesso.
##### Insieme degli archi minimale
Una soluzione per garantire la connessione in un grafo è definire un insieme $E_f\subseteq E$ di archi tale che $G(N,E_f)$ sia connesso, e poi fare in modo che le coppie di agenti che hanno una connessione in $E_f$ restino connessi; si deve quindi garantire che $||x_i(t)-x_j(t)||\le\rho,\ \forall\{i,j\}\in E_f,\ \forall t>0$.
Siccome il vincolo sulla libertà degli agenti è definito da $E_f$ (oltre che dal valore di $\rho$), non voglio che la cardinalità di questo insieme sia troppo alta; in questo modo impongo si un limite, ma questo non è troppo restrittivo.
Una soluzione è scegliere $E_f$ come l'insieme di archi minimale del grafo $G=(N,E)$ in modo che $G(N,E_f)$ sia connesso.
##### APF
![potenziale attrattivo per la connettività](potenziale%20attrattivo%20per%20la%20connettività.png)
Per implementare questa soluzione si usa l'[APF](artificial%20potential%20field.md) introducendo un potenziale attrattivo $J_{ij}^{con},\ \forall\{i,j\}\in E_f$ tale che abbia minimo dove l'$i$-esimo e il $j$-esimo agente sono più vicini.
Una possibile implementazione (ma ce ne sono altre) è data da $J_{ij}^{con}=\begin{cases}0,&\text{ if }||x_i-x_j||\le\rho_0\\\tfrac{1}{\beta}(\tfrac{1}{\rho-||x_i-x_j||}-\tfrac{1}{\rho-\rho_0})^\beta,&\text{ if }||x_i-x_j||>\rho_0\end{cases}$, per un qualche valore di $\beta$ (solitamente 2 o 3) e di $\rho_0$; in questo modo si vede che $J_{ij}^{con}\to\infty$ se $||x_i-x_j||\to\rho$, cioè se i due agenti si avvicinano al punto in cui non sono più connessi; invece $J_{ij}^{con}=0$ se $||x_i-x_j||\le\rho_0$, cioè diventa nullo se i due agenti sono abbastanza vicini (rispetto alla scelta di $\rho_0$).
In generale, definito un qualche $J_{ij}^{con}$ il potenziale attrattivo di connettività del sistema è dato da $J^{con}(x)=\displaystyle\sum_{\{i,j\}\in E_f}J_{ij}^{con}(x_i,x_j)$.