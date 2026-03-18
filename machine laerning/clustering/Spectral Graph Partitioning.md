È un algoritmo di [clustering](Clustering.md) basato sul grafo che permette di trovare cluster come cerchi concentrici.
# Funzionamento
- Si crea il grafo di similarità e la relativa matrice $W\in\mathbb{R}^{n\times n}$, con $n$ nodi; un valore maggiore in posizione $ij$ indica che i nodi $i$ e $j$ sono vicini (simili). 
	Con questa matrice sarebbe già possibile identificare le componenti connesse: i gruppi di nodi connessi sono infatti i blocchi della matrice. Il problema però è il rumore: se ci sono archi con pesi molto piccoli oppure la matrice è permutata (cioè elementi vicini non sono celle vicine) allora diventa molo difficile.
- Si calcola la matrice diagonale dei gradi $D\in\mathbb{R}^{n\times n}$. L'elemento diagonale $d_{ii}$ è costruito con la somma dei pesi degli archi del nomo $i$-esimo o con la somma della riga $i$-esima della matrice $W$. Rappresenta quindi quanto il nodo $i$-eismo è simile a tutti gli altri.
- Si calcola la Laplacian del grafo $L=D-W$. Questa matrice $L$ è simmetrica, quadrata, semidefinita positiva (i.e. $v^TLv\ge0,\forall v$), e quindi tutti i suoi autovalori sono $\lambda_i\ge0$.
- Si calcolano gli autovettori e i relativi autovalori di $L$. Si può [osservare](spctral%20graph%20autovalore%200.png) che il primo autovalore è sempre 0: $We=De\Rightarrow(D-W)e=0\Rightarrow Le=0e$, con 0 autovalore e $e$ il relativo autovettore.
- Da $L$ si costruiscono due matrici:
	- La matrice diagonale $\Lambda$ i cui elementi diagonali sono i suoi autovalori ordinati in modo crescente. Il numero di autovalori nulli rappresenta il numero di componenti connesse del grafo; se un valore è vicino a 0 allora vuol dire che non c'è un'altra componente connessa per via di un arco con un peso molto basso.
	- La matrice $V$ le cui colonne sono i suoi autovettori. Le prime due colonne rappresentano come gli elementi sono distributi in due cluster tramite blocchi di 0.
- Nella realtà le prime due colonne non sono a blocchi. Si utilizza [K-means](K-means.md) sulle prime $k$ colonne di $V$, cioè sugli elementi del grafo proiettati in uno spazio a $k$ dimensioni. Questo semplifica il calcolo perché è come fare il clustering dopo aver fatto una riduzione della dimensionalità.
# Conseguenze
- Si riescono a catturare pattern non lineari.