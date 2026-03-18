È un algoritmo di [clustering](Clustering.md#Tipi) basato sulla densità. La densità è definita come il numero di punti all'interno di un dato raggio $\epsilon$.
È un algoritmo molto buono quando sappiamo quanti cluster vogliamo. Il tipo e numero di cluster viene trovato sulla base dell'inizializzazione dei parametri $\epsilon$ e $MinPts$; selezionando parametri diversi si ottengono risultati diversi.
# Punti
Dato un valore soglia $MinPts$, i punti di suddividono in tre gruppi:
- Core
	Sono quei punti che hanno all'interno del loro raggio $\epsilon$ almeno $MinPts$ punti.
- Border
	Sono punti non core, cioè che hanno meno di $MinPts$ punti all'interno del loro raggio $\epsilon$, ma che stanno sul bordo di un core point.
- Noise
	Sono tutti gli altri punti. In pratica sono gli outliers.
# Funzionamento
- Si etichettano tutti i punti in base al loro tipo: core, border o noise.
- Si eliminano i noise points.
- Si pone un arco tra tutti i core points che hanno una distanza al più $\epsilon$ tra loro.
- Si assegna ogni border point al core point più più vicino tramite un arco.
# Vantaggi
Costruendo il grafo solo tramite i core point, anche se un border point ha un po di rumore questo non influenzerà l'unione di due componenti connesse.
# Confronto con [K-means](K-means.md)
- Sono entrambi partizionanti: un elemento appartiene a un solo cluster.
- K-means è completo, cioè assegna ogni elemento a un cluster; DBSCAN scarta gli outliers.
- K-means è basato su prototipi, mentre DBSCAN su densità.
- K-means preferisce cluster globulari; DBSCAN aggrega cluster con diverse cardinalità e forme.
- K-means ha un costo lineare nel numero di elementi, DBSCAN quadratico.