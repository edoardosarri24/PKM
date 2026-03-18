Il clusting può essere visto come una riduzione rispetto ai samples: se con la [riduzione della dimensionalità](dimensionality%20reduction.md) si vuole rimuovere delle features, con il clustering si vuole aggregare più samples con le stesse caratteristiche.
Dato un insieme di punti e definita una [Distanza](Distanza.md) tra questi, un algoritmo di clustering raggruppa i punti in un certo numero di cluster tali che gli elementi di un cluster sono punti simili e quelli di cluster diversi sono meno simili.
- Per punti non si intende nello spazio euclideo, ma sono punti in senso generico, ad esempio intesi come elementi.
##### Problemi
I metodi di clustering sono algoritmi e producono un risultato indipendentemente dal fatto che questo abbia effettivamente senso. Anche se i dati non contengono un pattern chiaro, il clustering produrrà una qualche aggregazione e sta a noi capire se questa ci soddisfa.
##### Usi
- Data pre-processing
	Ci sono delle operazioni che sono molto coste; farle su meno dati, cioè meno righe della matrice, è più conveniente.
- Compressione dei file.
- Outliers detection
	È utile nell'anomaly detection.
# Tipi
In base a quello che fa un algoritmo di clustering può essere di più tipi:
- Ben separati
	La distanza tra cluster è molto maggiore della distanza tra i suoi elementi. In questo caso i cluster sono molto distanti l'uno dall'altro.
- Protorype-based - [gerarchico](Clustering%20gerarchico.md)/[K-means](K-means.md)/[SOM](Self%20organizing%20Map%20(SOM).md)
	Gli elementi dello stesso cluster sono vicini a un prototipo (e.g. baricentro) che li rappresenta tutti. Può succedere che ci siano elementi di cluster diversi molto vicini tra loro.
	Come prototipo si hanno due scelte:
	- Il centroide è la media dei punti del cluster (e.g. baricentro). È un elemento che può anche non appartenere all'insieme degli elementi.
	- Il medoide è l'elemento più rappresentativo del cluster (e.g. mediana). È un elemento di quelli contenuti dnell'insieme.
- Graph-based - [MST clustering](MST%20clustering.md)/[Spectral graph partitioning](Spectral%20Graph%20Partitioning.md)
	Gli elementi sono nodi di un grafo e due elementi appartengono allo stesso cluster se sono connessi e se la loro distanza è minore di una data soglia. In generale possiamo dire che i cluster sono rappresentati dalle componenti connesse di un grafo, dove una componente connessa è un insieme di nodi tali per cui tra ogni coppia di nodi c'è un cammino che li unisce.
	Il problema in questo tipo di clustering è il rumore: se ci sono degli outsider allora potrebbero essere inclusi in un cluster a cui non dovrebbero appartenere.
- Density-based - [DBSCAN](DBSCAN.md)
	Il concetto di distanza è sostituito dalla densità e un cluster è una regione di spazio più densa di una data soglia.
	Quando lo spazio non è sparso (ci sono molti punti in qua e la) usando le altre tecniche può succedere che diventi tutto un unico cluster.
- Concettuali
	Un cluster in questo caso è un insieme di elementi che condividono una data proprietà.
	È utile quando non è possibile definire una distanza in senso stretto (e.g. modelli relazionali).