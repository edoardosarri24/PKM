È una tecnica per costruire la road map secondo un approccio [planning-based](robot%20motion%20planning.md#Planning-based%20solution) per il problema del [robot motion planning](robot%20motion%20planning.md).
# Soluzione
![[approximate cell decomposition](approximate%20cell%20decomposition.png)
La road map viene costruita così:
- Si decompone lo spazio delle configurazioni in una griglia, detta occupancy grids. Se siamo in due dimensioni questi sono quadrati altrimenti ipercubi. Una cella della griglia può essere libera od occupata: la prima si spiega da se; una cella è invece occupata se al suo interno contiene una porzione di un ostacolo.
- Si pone un nodo nel centro di ogni cella.
- Si pone un arco tra due nodi adiacenti. Se siamo in uno spazio come [so(2)](spazi.md#$so(2)$) allora si deve fare attenzione all'effetto packman, cioè quando un arco può attraversare anche i bordi.
# Conseguenze
- Completezza
	È un algoritmo resolution complete: se un percorso esiste, può non essere trovato a causa della precisione della griglia. Questo accede perché se le celle sono troppo grandi allora tutte saranno occupate.
	Si risolve con l'[adaptive grids](approximate%20cell%20decomposition.md).
- Spazio non poligonale
	È valido anche per spazi non poligonali.
- Complessità
	È esponenziale nel numero di gradi di libertà dell'agente, cioè essa è $\mathcal{O}{(K^{n_c})}$, dove $K$ è la precisione e $n_c$ è la dimensionalità di $C$: se cresce la dimensionalità dello [spazio delle configurazioni](spazi.md#Configuration%20space) allora la complessità esplode.
- Ottimalità
	Non trova il percorso ottimo.
- Robustezza
	È abbastanza robusto perché non si passa troppo vicino agli ostacoli.
# Adaptive grids
Per risolvere parzialmente il problema della completezza si distingue tra celle totalmente occupate e parzialmente occupate, dove le seconde sono quelle celle che hanno al loro interno un ostacolo ma non sono interamente occupate.
Si segue poi questo approccio:
- Si prova a cercare un percorso con l'attuale dimensione delle celle.
- Se non si trova si dimezza la dimensione delle celle parzialmente occupate e si riprova.
##### Completezza
Rispetto alle conseguenze dall'algoritmo normale questo è semi-completo: se un percorso esiste allora viene trovato; se non esiste allora l'algoritmo non termina; nella realtà poi si mette un criterio di arresto e si fa terminare.