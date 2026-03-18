Nei [grafi](grafi.md) indiretti un arco $\{i,j\}\in E(i,j)\subseteq N\times N$ è considerato percorribile in entrambe le direzioni.
# Vicini
I vicini di un nodo sono tutti i nodi direttamente collegati, cioè che possono essere raggiungibili attraverso un solo arco (un solo hop).
Si scrive $N_i=\{j\in N:\{i,j\}\in E\}$.
# Grado
Il grado $d_i$ del nodo $i$-esimo è il numero dei suoi vicini.
- Medio
	Ci fornisce una misura grezza del grado di [connettività](connettività.md) di un grafo. In grafo completo si ha $\bar{d}=d_i,\forall i$.
	Dato $N$ il numero di nodi si ha $\bar{d}=\displaystyle\tfrac{1}{N}\sum_{i=1}^Nd_i$.
- Massimo
	Dato $N$ il numero di nodi si ha $d_{max}=\displaystyle\max_{i\in N}d_i$.
- Distribuzione
	È una distribuzione dei gradi: $P_d$ indica la percentuale di nodi di grado $d$. Ci da delle informazioni sul tipo di grafo, sopratutto se facciamo il grafico; è utile soprattutto per situazioni estreme, come il grafo completo e quello a hub.
	In quanto distribuzione valgono:
	- $P_d\ge0,\forall d$
	- $\displaystyle\sum_{d=0}^{d_{max}}P_d=1$
# Matrice dei gradi
È una matrice diagonale quadrata dove l'elemento diagonale $i$-esimo rappresenta il grado $d_i$ del nodo $i$-esimo.
Si scrive $d_i=|N_i|$.
# Matrice di adiacenza
È una matrice binaria, simmetrica e quadratica definita tramite $a_{ij}=\begin{cases}1&\text{ if }\{i,j\}\in E\\0&\text{ altrimenti}\end{cases}$. C'è una relazione biunivoca tra la matrice di adiacenza e un grafo, quindi ci da le informazioni complete su di esso.
Vale $d_i=\displaystyle\sum_{j=1}^Na_{ij}$, cioè vale che il grado del nodo $i$-esimo è la somma della riga $i$-esima della matrice di adiacenza.
# Distanza
La distanza $\delta_{ij}$ tra i nodi $i$ e $j$ è definita come il numero di archi che si devono attraversare per raggiungere $j$ da $i$, cioè il numero di hop tra $i$ e $j$.
- Media
	Si può definire come $\displaystyle\bar{\delta}=\tfrac{1}{N(N-1)}\sum_{i=1}^N\sum_{j=1}^N\delta_{ij}$.
- Massimo
	Si definisce come $\delta_{max}=\max_{i,j}\delta_{ij}$.
# Teorema spettrale
Se un grafo è indiretto allora la sua matrice di adiacenza è simmetrica e allora per il teorema spettrale:
- Tutti gli autovalori sono reali.
- Esiste un insieme di autovettori $\{v_1,\cdots, v_N\}$ ortonormali, cioè tali che $v_i^T\cdot v_i=1$ e $v_i^T\cdot v_j=0$.
##### Diagonalizzazione
Da questo teorema segue che una matrice simmetrica $L$ può essere diagonalizzata come $L=V\Lambda V^T$, dove:
- $\Lambda$ è la matrice diagonale degli autovalori di $L$.
- $V$ è la matrice ortonormale degli autovalori, cioè $V=[v_1\cdots v_N]$, con $VV^T=V^TV=I$.