È un algoritmo di [clustering](Clustering.md) basato su prototipi.
Si può usare per clusterizzare documenti: documenti di argomenti simili vanno vicini tra loro.
# Funzionamento
Il parametro del metodo è il numero $K$ di cluster. Non è arbitrario perché l'algoritmo impone una topologia a griglia (non necessariamente quadrata) e quindi $K$ non può essere primo.
- Si inizializzano i cluster scegliendo un elemento per cluster. Per fare questo ci sono varie tecniche (non viste).
- Si ripete fino alla convergenza:
	- per ogni punto alla volta:
		- Si assegna il punto al centroide più vicino.
		- Si aggiornano tutti i centroidi secondo la distanza dal punto: centroidi più vicini al punto saranno spostati di più rispetto ai centroidi più lontani. Visto che si aggiornano i centrodi dopo aver visto un solo elemento di parla anche di online SOM training (contro ad esempio [K-means](K-means.md) che è detto batch).
##### Aggiornamento
Abbiamo detto che l'aggiornamento di centroidi avviene in relazione alla distanza dal punto che stiamo analizzando.
Se $m_1(t),\cdots,m_k(t)$ sono i centroidi dopo l'aggiornamento $t$-esimo allora per il punto $x$ si deve:
- Trovare il centroide $b(x)$ più vicino e assegnare il punto a quel centroide.
- Spostare ogni altro centroide $m_i$ come $m_i(t+1)=m_i(t)+h_{b(x),i}(t)(x-m_i(t))$, dove:
	- $x-m_i(t)$ è la distanza del centroide $i$-esimo dal punto in questione.
	- $h_{b(x),i}(t)=\alpha(t)e^{-\tfrac{||r_i-r_{b(x)}||^2}{2\sigma^2(t)}}$ è in pratica una gaussina centrata in x.
		Mi dice due cose: i centroidi più vicini a $x$ si spostano di più; con il passare del tempo $\alpha$ viene decrementato e allora ci spostiamo più alle prime epoche e meno a quelle dopo.
		- $\alpha(t)$ è il learning rate e decresce monotonicamente.
		- $r_k$ sono le coordinate del $k$-esimo centroide nella griglia. In questo modo $||r_i-r_{b(x)}||$ rappresenta la distanza euclidea tra il punto in questione e quello del suo centroide.
##### Arresto
Solitamente il criterio di arresto è quando i centroidi non cambiano posizione più di una data soglia.
Solitamente non ci vogliono molte iterazioni.