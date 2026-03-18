Parleremo di connettività in un [grafo indiretto](grafi%20indiretti.md).
Un grafo è connesso quando esiste un percorso tra ogni coppia di nodi. Questo concetto binario deve essere esteso a una metrica: ci interessa associare a un grafo un livello di connettività.
# Misure
Le misure quantitative per la connettività sono molte e dipendono dal contesto in cui le applichiamo.
##### Grado dei nodi
Si può prendere il massimo, il minimo o la media.
##### Distanza tra nodi
Solitamente è usata per capire la velocità di propagazione dell'informazione tra nodi.
Se prendiamo la media valutiamo il caso medi; se prendiamo il massimo valutiamo caso pessimo.
##### Robustezza rispetto alla disconnessione
Vale $v_n\le v_e\le d_{min}$: la seconda disuguaglianza è data dal fatto che rimuovere tutti gli archi del nodo che ha grado minimo comporta portare il grafo a due componenti connesse (se prima era connesso).
In merito a questo, un risultato importante si ottiene osservando lo [spettro](laplanciano.md#Connettività%20algebrica) del laplanciano.
- Edge connectivity $v_e$
	È il numero minimo di archi che dobbiamo rimuovere per arrivare a un grafo non connesso. Si chiama problema del min cut.
- Node connectivity $v_n$
	È il numero minimo di nodi che dobbiamo rimuovere per arrivare a un grafo (nodi rimossi esclusi) non connesso.