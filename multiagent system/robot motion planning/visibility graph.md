È una tecnica per costruire la road map secondo un approccio [planning-based](robot%20motion%20planning.md#Planning-based%20solution) per il problema del [robot motion planning](robot%20motion%20planning.md).
# Assunzione
Stiamo assumendo che il mondo sia poligonale, cioè che gli ostacoli possano essere definiti da poligoni (insieme dei punti del piano delimitati da una curva spezzata chiusa e non intrecciata) all'interno dello spazio delle configurazioni. Se così non fosse dobbiamo approssimarli a dei poligoni.
# Soluzione
![visibility graph](visibility%20graph.png)
Il grafo della road map viene costruito definendo:
- L'insieme dei nodi è composto dallo start, dal goal e da tutti i vertici degli ostacoli.
- Due nodi definiscono un arco se il segmento che li unisce non attraversa un ostacolo; i lati degli ostacoli sono ammessi come archi. Se siamo in uno spazio non cartesiano come [so(2)](spazi.md#$so(2)$) allora quando si valuta la visibilità dobbiamo considerare l'effetto packman, cioè quando un arco può attraversare anche i bordi.
# Conseguenze
##### Vantaggi
- Completo
	Dato uno spazio delle configurazioni in due dimensioni, se esiste un percorso senza collisioni allora l'algoritmo lo trova; altrimenti dice che non esiste.
- Ottimalità
	In un ambiente poligonale trova il percorso ottimo.
- Semplicità
	È semplice da implementare in uno spazio a due dimensioni.
##### Svantaggi
- Supposizione
	Richiede la supposizione di un mondo poligonale. Altrimenti si deve introdurre un'approssimazione.
- Robustezza
	Abbiamo poca [robustezza](agenti.md#Intelligenti) perché l'agente navigherà vicino agli ostacoli.
- Complessità
	Costo computazione quadratico nel numero di vertici degli ostacoli, cioè $\mathcal{O}{(N_v^2)}$, dovuto alla creazione degli archi. Questo è un problema se abbiamo molti ostacoli ma sopratutto se lo spazio delle configurazioni ha una dimensione molto alta (perché il numero di archi esplode); per questo è un approccio che si usa in 2D.