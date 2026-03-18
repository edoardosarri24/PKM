L'algoritmo BFR è un algoritmo di [clustering](Clustering.md) che estende [K-means](K-means.md) per grandi quantità di dati.
##### Assunzioni
- I dati sono distribuiti in modo normale (non uniforme) attorno a un centroide in uno spazio euclideo.
- La deviazione standard può essere diversa a seconda della direzione.
- I cluster sono allineati agli assi; cioè non sono in diagonale.
# Funzionamento
- I dati sono caricati in memoria centrale in batch.
- Alla prima iterazione sul primo batch si selezionano $K$ centroidi. Le tecniche per fare questo sono le stesse di [k-means](K-means.md#Funzionamento).
- Quando arriva un batch si prova ad assegnare (nell'ordine) ad uno dei sequenti insiemi:
	- Discard Set
		Sono i nodi vicini almeno $\epsilon$ al centroide. Quando arriva un elemento che appartiene al discard set, si aggiornano i vettori $SUM$ e $SUMSQ$ e si elimina.
		Per ogni centroide si deve quindi memorizzare il numero di samples che rappresenta, la somma dei vettori $SUM_i$ per ogni direzione $i$ dello spazio e la somma dei quadrati $SUMSQ_i$ per ogni direzione $i$. In questo modo se abbiamo $d$ dimensioni i valori che rappresentano il set sono $2d+1$. Il centroide in ogni dimensione è dato da $SUM_i/N$ e la deviazione standard $SUMSQ_i/N-(SUM_i/N)^2$.
	- Compression Set
		Sono gruppi di punti vicini tra loro ma più lontani di $\epsilon$ dal centroide. Questi vengono aggregati, cioè si memorizzano in qualche modo, ma non appartengono a un centroide.
	- Retained Set
		Sono gli outliers. Devono essere mantenuti e memorizzati.
##### Distanza
Per decidere se un punto è abbastanza vicino a un centroide o vicino ad altri punti si usa la distanza di [Mahalanobis](Distanza.md#Mahalanobis) è inferiore di una data soglia.