Una CTMC è un [processo stocastico](stochastic%20process.md) a tempo continuo $\mathbb{X}=\{X(t),t\in\mathbb{R}\}$ che soddisfa la [condizione di markov](condizione%20di%20markov.md). Solitamente il tempo continuo si concretizza con il tempo trascorso dall'inizio del processo.
Siamo interessati alle CTMC che hanno un numero di stati finiti e che soddisfano la proprietà [tempo omegeneo](condizione%20di%20time-homogeneous.md); gli stati sono identificati dalle marcature raggiungibili.
# Tempo di soggiorno
Il tempo di soggiorno nello stato $i$ è pari a $Exp(-q_{ii})$, dove $q_{ij}$ è un elemento del generatore infinitesimale (vedi sotto).
# Soluzione
Per [risolvere](markov%20chain.md#Risoluzione) una catena di markov a tempo discreto ci servono due ingredienti:
- Vettore delle probabilità iniziali
	È il vettore $\pi^0=\pi(0)$. Il suo elemento $\pi_i(0)$ definisce la probabilità che all'inizio il sistema sia nello stato $i$-esimo.
- Generatore infinitesimale $Q$
	I suoi elementi sono $q_{ij}=\tfrac{d}{dt}P_{ij}(t)$, dove $P_{ij}(t)=Prob\{M(t)=j|M(0)=i\}$ è la continuous transition probabiity ed è la probabilità si trovarsi nello stato $j$ dato che all'inizio del processo siamo nello stato $i$. Per analizzare una CTMC ci serve la derivata in 0 della transition matrix $P$; per trovare questa derivata è sufficiente il rate $\lambda$.
##### Proprietà di $Q$
- La somma sulle righe è 0, cioè $\sum_jq_{ij}=0$.
	Questo viene dal fatto che, per la total probability $\sum_jP_{ij}(t)=1$ (si vede dalla definizione di $P_{ij}(t)$), e facendo la derivata a destra e sinistra rispetto al tempo otteniamo $\sum_jq_{ij}=0$.
- Gli elementi fuori dalla diagonale sono non negativi, cioè se $i\ne j$ allora $q_{ij}\ge0$.
	Questo viene dal fatto che $P_{ij}(t)$ è una probabilità e quindi è non negativa, ma $P_{ij}(0)=0$ (si vede dalla definizione di $P_{ij}(t)$); allora la sua derivata in 0, cioè $q_{ij}$, deve essere una pendenza positiva.
- Gli elementi sulla diagonale sono il complemento a uno degli altri, cioè $q_{ii}=-\sum_{j\ne i}q_{ij}$.
##### Soluzione transiet-probability
Si può sempre trovare e si deve risolvere un sistema di equazioni differenziali lineari del primo ordine, dette Chapman-Kolmogorov $\tfrac{d\pi(t)}{dt}=\pi(t)Q$ la cui soluzione formale è data da $\pi(t)=\pi(0)Q^{Qt}$.
##### Soluzione steady-state
Se il sistema è irriducibile e il numero di stati è finito allora risolvendo il sistema lineare $\pi Q=0$ si trova la probabilità di essere in uno dei possibili stati dopo molto tempo.
