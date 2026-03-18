È un algortimo di [clustering](Clustering.md) basato su prototipi.
# Dendogramma
I cluster sono rappresentati in modo gerarchico tramite un albero, che in questo contesto prende il nome di [dendogramma](dendogramma.pdf): la radice è il cluster che comprende tutti gli elementi; i nodi sono i cluster che comprendono un solo elemento.
Sull'asse delle ascisse il dendogramma contine gli elementi, che formano i singoli cluster. Sull'asse delle ordinate invece le distanze tra i sotto cluster che appartengono al cluster in questione.
Il modo giusto per leggere il dendogramma e capire l'ordine in cui sono stati uniti i cluster è dall'alto verso il basso.
##### Split/merge
L'obiettivo del dendogramma è memorizzare i vari split o merge. Un cluster può essere formato in due diversi modi:
- Bottom up
	Si parte da tanti cluster unitari e si arriva a più cluster delle dimensioni necessarie.
- Top down
	Si parte da un solo cluster che contiene tutti gli elementi e si arriva a tanti cluster delle dimensioni opportune.
# Pseudo codice
L'algoritmo bottom-up è così definito:
- Si calcola la matrice di prossimità, dove la cella $ij$ contiene la distanza tra gli elementi $i$ e $j$.
- Consideriamo ogni elemento come un cluster.
- Si ripete finchè non abbiamo un criterio di arresto.
	- Uniamo i due cluster più vicini, secondo una qualche [distanza](Clustering%20gerarchico.md#Distanze%20tra%20cluster).
	- Aggiorniamo la matrice di prossimità.
##### Complessità
È un algoritmo costoso:
- Rispetto allo spazio ha una complessità $\mathcal{O}{(n^2)}$, necessaria per memorizzare la matrice di prossimità.
- Rispetto al tempo ha una complessità $\mathcal{O}{(n^3)}$ perché abbiamo $n$ passi per ognuno dei quali si deve cercare e scrivere la matrice di prossimità.
##### Conseguenze
- Una volta che due cluster sono uniti non si può più tornare indietro.
- Non minimizza una funzione di costo globale.
- È un algoritmo deterministico.
# Distanze tra cluster
A prescindere da come definiamo il criterio con cui valutare i due cluster più vicini, il primo merge (due cluster formati da un punto) è sempre lo stesso.
Non c'è un metodo migliore, ma ognuno ha i suoi problemi.
##### Minimo
La distanza tra due cluster è definita come la minima distanza tra ogni coppia formata da elementi di cluster diversi.
- Dobbiamo memorizzare tutti gli elementi che appartengono al cluster.
- Il vantaggio è che favorisce cluster con forme complesse (non ellittiche).
- Lo svantaggio è molto suscettibile al rumore.
##### Massimo
La distanza tra due cluster è definita come la massima distanza tra ogni coppia formata da elementi di cluster diversi. Possiamo osservare che la distanza calcolata tra due cluster che poi saranno niti corrisponde al diametro del nuovo cluster.
- Dobbiamo memorizzare tutti gli elementi che appartengono al cluster.
- Tende a favorire cluster sferici.
- Il vantaggio è che è meno suscettibile al rumore.
##### Media
La distanza tra due cluster è definita come la media delle distanze di tutti i punti dei cluster diversi; la distanza tra due cluster $A$ e $B$ è $d(A, B) = \frac{1}{|A| \cdot |B|} \sum_{x \in A} \sum_{y \in B} d(x, y)$.
- Dobbiamo memorizzare tutti gli elementi che appartengono al cluster.
 - Può essere visto come un compromesso tra il minimo e il massimo.
##### Centroidi
La distanza tra due cluster è definita come la distanza tra i loro centroidi.
- Dobbiamo memorizzare tutti gli elementi che appartengono al cluster. Per aggiornare il centroide dobbiamo memorizzare l'informazione su quanti punti sono contenuti nel cluster.
# Criteri di arresto
Ci sono varie possibilità, ma ne possono anche essere descritte altre con vantaggi e svantaggi diversi.
- Quando si raggiunge un certo numero di cluster.
- Se la densità del nuovo cluster è minore di una soglia. La densità può essere definita come il diametro del cluster sul numero di elementi che ci appartengono.
- Se il diametro del nuovo cluster è maggiore di una soglia.
- Non ci fermiamo, cioè costruiamo tutto il dendogramma e poi controlliamo quando le distanze diventano troppo grandi (anche a occhio).