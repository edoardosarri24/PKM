È un algoritmo di [clustering](Clustering.md) basato sul grafo.
##### Ingredienti
Ci serve calcolare due aspetti:
- Grafo di prossimità
	È chiamato anche grafo di dissimilirità: valori più grandi indicano elementi più distanti.
	È solitamente un grafo completo i cui nodi sono i samples e gli archi sono etichettati con la distzan tra i due nodi.
- Matrice di prossimità
	È la rappresentazione matriciale del grafo di prossimità: nella cella $ij$ c'è il valore della distanza tra il nodo $i$ e il nodo $j$.
##### Sparisificare
Lavorare sul grafo/matrice di prossimità è molto complicato perché è un grafo/matrice completo (cioè non sparso). Per questo motivo si tende a sparsificare il grafo/matrice: si eliminano quegli archi la cui etichetta è maggiore di una data soglia. In questo modo si ottiene un grafo/matrice con molti meno archi/valori e su cui è più facile lavorare.
# Minimum Spannin Tree
Può essere visto come un [clustering gerarchico](Clustering%20gerarchico.md) top-down: si parte da un solo cluster e si arriva a cluster con un solo elemento.
##### Funzionamento
- A partire dal grafo di prossimità non sparsificato si calcola il suo MST; il risultato può essere visto come una sparsificazione del grafo di prossimità. Il MST di un grafo con $n$ nodi è un grafo con $n-1$ archi tale che collega tutti nodi con gli archi la cui somma dei pesi è minima (non è unico).
- Si ripete finchè non si ottiene tutti singleton (cluster composti da un solo elemento) o i cluster hanno un numero di elementi voluto:
	- Si crea un nuovo cluster eliminando l'arco che ha il costo maggiore.