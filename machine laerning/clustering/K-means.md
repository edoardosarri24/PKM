È un algoritmo di [clustering](Clustering.md) basato su prototipi.
Si parla di k-means se siamo in uno spazio euclideo (e quindi è definita la media); se non siamo in uno spazio euclideo e dobbiamo prendere una mediana, cioè un rappresentate del gruppo, allora si parla di k-medoid (medoiode).
Se abbiamo molti dati allora una sua variazione è [BFR algorithm](BFR%20algorithm.md).
##### Determinismo
È un algoritmo iterativo. A meno della scelta dei primi $k$ cluster (che sono scelti casualmente) è un algoritmo deterministico.
# Funzionamento
Il parametro del metodo è il numero $k$ di cluster.
- Si inizializzano i cluster scegliendo un elemento per cluster. Se abbiamo $K$ cluster veri (quelli che pensiamo essere cluster) allora la probabilità di avere un centroide per cluster è molto bassa. Per scegliere i centroidi iniziali ci sono varie tecniche:
	- Si inizializzano a caso e si fanno tante iterazioni. È possibile perché il costo dell'algoritmo è basso.
	- Si seleziona il primo centroide in modo casuale e poi si prendono gli altri $K-1$ in modo che siano molto distanziati. Questo è k-means++.
	- Si usa il [clustering gerarchico](Clustering%20gerarchico.md) per determinare i centrodi. Siccome questo è molto costoso si deve usare pochi elementi (anche $\tfrac{1}{100}$ di quelli totali).
- Si ripete fino alla convergenza:
	- Si assegna ogni punto del cluster rappresentato dal centroide puù vicino.
	- Si ricalcola il centroide per ogni cluster. Visto che i centroidi vengono aggiornati dopo aver assegnato tutti i nodi, k-means è anche detto batch k-means (contro ad esempio [SOM](Self%20organizing%20Map%20(SOM).md)).
##### Convergenza
Converge verso l'ottimo. Questo, a seconda della scelta iniziale dei primi $k$ cluster può essere un minimo locale o globale.
##### Arresto
Solitamente il criterio di arresto è quando le cose non cambiano più di una data soglia. Questo solitamente viene raggiunto dopo poche iterazioni.
Il fatto che si fermi in ogni situazione non mi dice che il cluster trovato sia ottimo o che abbia qualche senso.
##### Costo
In generale il costo è $\mathcal{O}{(n\cdot K\cdot I\cdot d)}$, dove $n$ è il numero di samples del dataset, $I$ è il numero di iterazioni, $K$ il numero di cluster e $d$ la dimensione dello spazio in cui sono definiti i punti.
È molto più efficiente del [cluster gerarchico](Clustering%20gerarchico.md#Complessità) perché solitamente le $I$ iterazioni sono poche, il numero $K$ di cluster che ci interessa è trascurabile rispetto al numero di samples e anche la dimensionalità $d$ è minore di $n$.
##### Loss
Si può vedere come un problema di minimizzazione, dove la loss è la SSE (Sum of Squered Error function) $\sum_{i=1}^k\sum_{x\in C_i}dist^2(m_i,x)$: per tutti i cluster e per ogni punto contenuto nel cluster sommo le distanze medie rispetto al centroide del proprio cluster.
# Limitazioni
Non funziona bene quando:
- I cluster non sono sferici.
- I cluster veri hanno una cardinalità molto diversa.
- Ci sono molti ouliers. Infatti gli outliers tirano il centroide verso di loro.
##### Soluzione
Una soluzione è inizializzare molti cluster $K$ e poi fare una fase di post-processing per unire cluster che dovrebbero stare insieme.
Il giusto $K$ si trova in maniera empirica valutando la distanza media dei punti dal porprio centroide. Arriveremo a un valore in cui questa non diminuisce più all'aumentare di $K$.