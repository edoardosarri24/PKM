È una tecnica per costruire la road map secondo un approccio [planning-based](robot%20motion%20planning.md#Planning-based%20solution) per il problema del [robot motion planning](robot%20motion%20planning.md).
# Assunzione
Stiamo assumendo che il mondo sia poligonale, cioè che gli ostacoli possano essere definiti da poligoni (insieme dei punti del piano delimitati da una curva spezzata chiusa e non intrecciata) all'interno dello spazio delle configurazioni. Se così non fosse dobbiamo approssimarli a dei poligoni.
# Soluzione
![exact cell decomposition](exact%20cell%20decomposition.png)
La road map viene costruita così:
- Lo spazio delle configurazioni viene suddiviso in trapezi usando segmenti verticali (od orizzontali) che partono dai vertici degli ostacoli.
- Si pone un nodo sulla metà di ogni segmento e all'interno di ogni trapezio (ad esempio nel baricentro).
- Si connettono nodi adiacenti. Se siamo in uno spazio come [so(2)](spazi.md#$so(2)$) allora si deve fare attenzione all'effetto packman, cioè quando un arco può attraversare anche i bordi.
- Lo start e il goal sono collegati al nodo nello stesso trapezio.
# Conseguenze
##### Vantaggi
- Completo
	Dato uno spazio delle configurazioni in due dimensioni, se esiste un percorso senza collisioni allora l'algoritmo lo trova; altrimenti dice che non esiste.
- Complessità
	La complessità è di $N_vlog N_v$, con $N_v$ numero di vertici degli ostacoli.
- Robustezza
	È robusto perché l'agente segue il centro del percorso creato dagli ostacoli.
##### Svantaggi
- Supposizione
	Richiede la supposizione di un mondo poligonale. Altrimenti richiede un'approssimazione.
- 2D
	Applicabile solo quando lo spazio delle configurazioni ha due dimensioni.
- Ottimalità
	Non trova il percorso ottimo.