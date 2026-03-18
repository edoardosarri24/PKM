Il PRM è una tecnica stocastica per costruire la road map secondo un approccio [planning-based](robot%20motion%20planning.md#Planning-based%20solution) per il problema del [robot motion planning](robot%20motion%20planning.md).
# Soluzione
![probabilistic road mad](probabilistic%20road%20mad.png)
La road map viene costruita considerando:
- Si campiona randomicamente un elemento nel [configuration space](spazi.md#Configuration%20space) finché esso non è in $C_{free}$.  Questo controllo viene fatto con il [collision detector](probabilistic%20road%20map.md#Collision%20detector).
- Si aggiunge il punto al grafo.
- Si cercano i [punti vicini](probabilistic%20road%20map.md#Vicinanza). Si cerca tramite un local planner (e.g., un segmento) un percorso che va da tali punti all'elemento campionato. La fattibilità di questa unione dipende dal local planner.
- Ci si arresta quando il grafo è abbastanza denso.
##### Vicinanza
Ci sono due approcci per definire i punti vicini:
- $\epsilon$-neighbors
	Dato $\epsilon$, i vicini sono i punti che stanno in un intorno di raggio $\epsilon$.
- $k$-nearest neghtborhood
	Dato $k$, i vicini sono i primi $k$ nodi che hanno la distanza minore.
	- Permette di creare archi con distanze maggiori.
	- Funziona bene se ci sono pochi ostacoli.
##### Collision detector
Il collision detector è un modulo (funzione) che controlla se una configurazione o un arco collidono con un qualche ostacolo. Può essere la parte più complicata del robot motion planning.
Per implementare un collision detector si possono usare sia soluzioni basate sulla geometria dello spazio (solitamente troppo complesse) sia che usano il campionamento della curva proposta dal local planner.
# Vantaggi
- Probabilisticamente completo.
	Con il numero di campionamento effettuati che tende all'infinito diventa completo. Questo vuol dire che se esiste un percorso senza collisioni allora la probabilità che l'algoritmo lo trovi tende a 0 se il numero di campioni tende all'infinito (supponendo un campionamento uniforme nello spazio).
- Poligonale
	Vale per qualunque tipo di ostacolo, non solo poligonale.
- Efficienza
	Rispetto agli approcci basati su griglia (e.g., [exact cell decomposition](exact%20cell%20decomposition.md) e [approximate cell decomposition](approximate%20cell%20decomposition.md)), dove le celle aumentano esponenzialmente con la dimensionalità dello spazio delle configurazioni, il numero di campioni è indipendente dalla dimensionalità. Ovviamente per avere la completezza dobbiamo prendere infiniti campioni e questo non sfugge alla [curse of dimensionality](dimensionality%20reduction.md#La%20maledizione%20della%20dimensionalità).
- Flessibilità
	Possiamo scegliere qualunque local planner.
# Svantaggi
- Completezza
	Non è completo. Siccome non possiamo nella pratica campionare infinite volte, se un percorso non esiste allora l'algoritmo (senza criterio di arresto) non termina.
- Ottimalità
	L'ottimalità del percorso dipende dal numero di campionamenti e dal local planner.
- Collosion detector
	Implementare un collision detector che sia computazionalmente efficiente è complesso e richiede (ad esempio) un campionamento dei punti che forma la curva che unisce i due punti.