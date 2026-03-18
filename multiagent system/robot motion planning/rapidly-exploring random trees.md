L'RRT è una tecnica stocastica per costruire la road map secondo un approccio [planning-based](robot%20motion%20planning.md#Planning-based%20solution) per il problema del [robot motion planning](robot%20motion%20planning.md).
# Soluzione
![rapidly-exploring random trees](rapidly-exploring%20random%20trees.jpeg)
La road map è rappresentata tramite un albero costruito facendo:
- Si parte con $q_{start}$ unico nodo dell'albero.
- Si campiona una configurazione $q$ nel [configuration space](spazi.md#Configuration%20space). Rispetto al [PRM](probabilistic%20road%20map.md) va bene anche se questo è all'interno di un ostacolo.
- Si trova il nodo $q_{near}$ presente nell'albero più vicino a $q$.
- Si trova il nodo $q_s$ lungo il segmento che unisce $q$ con $q_{near}$ che massimizza la distanza da $q_{near}$ e per cui $q_{near}-q_s$ è collision-free. Per trovare $q_s$ si può campionare lungo $q_{near}-q$.
- Si aggiunge $q_s$ e l'arco $\{q_s-q_{near}\}$ all'albero. Se $q_s\ne q$ allora $q$ si elimina.
- Si arresta quando:
	- La mappa è abbastanza densa.
	- Ogni $n$ iterazioni si considera $q$ come $q_{goal}$ e si termina se questo viene aggiunto all'albero.
# Vantaggi
- Probabilisticamente completo.
	Con il numero di campionamento effettuati che tende all'infinito diventa completo. Questo vuol dire che se esiste un percorso senza collisioni allora la probabilità che l'algoritmo lo trovi tende a 0 se il numero di campionamento tendono all'infinito (supponendo una distribuzione uniforme nello spazio).
- Poligonale
	Vale per qualunque tipo di ostacolo, non solo poligonale.
- Efficienza
	Rispetto agli approcci basati su griglia (e.g., [exact cell decomposition](exact%20cell%20decomposition.md) e [approximate cell decomposition](approximate%20cell%20decomposition.md)), dove le celle aumentano esponenzialmente con la dimensione, il numero di campioni è indipendente dalla dimensionalità. Ovviamente per avere la completezza dobbiamo prendere infiniti campioni e questo non sfugge alla [curse of dimensionality](dimensionality%20reduction.md#La%20maledizione%20della%20dimensionalità).
# Svantaggi
- Completezza
	Non è completo. Siccome non possiamo prendere infniti campioni se un percorso non esiste allora l'algoritmo (senza criterio di arresto) non termina.
- Ottimalità
	L'ottimalità del percorso dipende dal numero di campionamenti e dal local planner.
# Varianti
Ci sono delle varianti di RTT.
##### Bidirectional RRT
Si utilizzano due alberi, uno che si sviluppa dallo start e uno dal gol. Ogni $n$ iterazioni (eventualmente anche $n=1$) si prova a unire i due alberi: si considera il nuovo nodo $q_s$ aggiunto all'albero1 come se fosse $q$ dell'albero2; si cerca quindi $q_{near}$ nell'albero2 e si provano collegare.
##### RRT*
Segue lo stesso concetto di RRT, ma aggiunge il re-writing step. Quando un nodo $q_s$ viene aggiunto si considerano i suoi vicini; per ogni vicino $q_n$ si considera il costo del percorso $(q_{start}-q_n)$ attuale e quello che invece passa per $q_s$; se quest'ultimo è migliore allora si collega $q_n$ con $q_s$ e si elimina il vecchio arco.
- Ha un costo più elevato rispetto agli altri.
- Tende all'ottimo se il numero di campionamenti tende a infinito.