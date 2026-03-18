Si tratta di un [obiettivo](multi%20obiettivo%20in%20MAS.md) sempre utile in un sistema multi agente.
# Definizione
Si definisce $\delta$ come la distanza di sicurezza tra un agente $i$ e un agente $j$ (o un ostacolo); abbiamo allora che $||x_i(t)-x_j(t)||\ge\delta,\ \forall i,j\in N$.
Tra la distanza di sicurezza $\delta$ e il [raggio di connettività](#Vicini) $\rho$ c'è una relazione per cui $\delta\le\rho$: se $\delta>\rho$ allora non ha senso che due agenti debbano stare più lontani della distanza per cui non c'è connessione.
# Potenziale
![potenziale repulsivo per la collision avoidance](potenziale%20repulsivo%20per%20la%20collision%20avoidance.png)
Per implementare questa idea si usa l'[APF](artificial%20potential%20field.md) introducendo un potenziale repulsivo $J_{ij}^{col},\ \forall i,j\in N$ tale che abbia minimo dove l'$i$-esimo e il $j$-esimo agente sono più lontani.
Una possibile (ma ce ne sono altre) implementazione è data da $J_{ij}^{col}=\begin{cases}0,&\text{ if }||x_i-x_j||>\delta_0\\\tfrac{1}{\beta}(\tfrac{1}{||x_i-x_j||-\delta}-\tfrac{1}{\delta_0-\delta})^\beta,&\text{ if }||x_i-x_j||\le\delta_0\end{cases}$, per un qualche valore di $\beta$ (solitamente 2 o 3); in questo modo si vede che $J_{ij}^{col}\to\infty$ se $||x_i-x_j||\to\delta$, cioè se i due agenti si avvicinano troppo; invece $J_{ij}^{col}=0$ se $||x_i-x_j||>\delta_0$, cioè diventa nullo se i due agenti sono abbastanza lontani (rispetto alla scelta di $\delta_0$).
In generale, definito un qualche $J_{ij}^{col}$ il potenziale attrattivo di connettività del sistema è dato da $J^{col}(x)=\displaystyle\sum_{i=1}^N\sum_{j\in N_i}J_{ij}^{col}(x_i,x_j)$.
# Miglioramento
Una soluzione più precisa, rispetto a usare APF con il [single integrator dynamic](single%20integrator%20dynamic.md), è usare APF con il [double integrator dynamic](double%20integrator%20dynamics.md#Vantaggi).