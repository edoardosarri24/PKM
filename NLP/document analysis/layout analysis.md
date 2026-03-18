L'analisi del layout è un task della [document analysis](document%20analysis.md) che vuole segmentare regioni all'interno di un documento e dare loro un significato semantico.
Le tecniche di analisi si dividono in fisica e logica.
# Analisi fisica
Analizza il documento ed estrae strutture geometriche, chiamate regioni, che hanno features simili.
Ci sono tre livelli a cui possiamo segmentare un documento: basandoci sui pixel, basandoci sui bordi (solitamente non usata) o sulle regioni (la più utile).
Le tecniche sono:
- Bottom-up
	Parte dalle componenti connesse (i.e., caratteri o pixel a seconda del livello) e agrega quelle più vicine secondo una qualche distanza (e.g., minima, massima, centroidi). È più lenta ma porta a risultati migliori.
- Top down
	Parte dal documento e lo divide. Gestisce bene quei documenti che sono composti da regioni rettangolari.
##### Run-Lenght Smooting Algorithm (RLSA)
È un algoritmo bottom-up dove si aggregano pixel per arrivare a regioni. Le regioni solitamente diventano righe di testo o colonne ch rappresentano paragrafi; non è una tecnica buona se sono presenti immagini perché queste diventano un blocco di pixel neri.
Un documento può essere visto come righe o colonne di pixel. Se consideriamo solo pixel binari (i.e., in bianco e nero) ottenuti dopo una fase di [pre-processing](document%20pre-processing.md#Filtraggio) e consideriamo ad esempio le righe, allora si trasformano i pixel bianchi che formano una sequenza di lunghezza $n$ in pixel neri se $n<k$, con $k$ iperparametro.
##### Minimum Spanning Tree (MST)
È un algoritmo bottom-up che funziona bene quando le componenti connesse che appartengono alla stessa regione hanno una distanza minore rispetto a quelle di regioni diverse.
L'idea è simile a quella del clustering e quindi funziona bene per regioni che definiscono una sfera.
- Si definisce un grafo dove i nodi sono le [componenti connesse](document%20pre-processing.md#Componenti%20connesse) (solitamente caratteri) e gli archi uniscono le componenti connesse e sono pesati per la loro distanza.
- Si trova un MST.
- Si pota l'albero in modo iterativo eliminando l'arco che ha il peso maggiore.
##### Docstrum (document spectrum)
È un algoritmo bottom-up.
- Si crea un grafo dove ogni componente connessa (solitamente caratteri) è collegata solo con i kNN (i.e., le $k$ componenti connesse più vicine).
- Ogni arco può essere rappresentato come un vettore in coordinate polari $(d,\phi)$, dove d è la distanza tra i centri delle componenti connesse e $\phi$ è l'angolo che il segmento definisce con l'asse orizzontale.
- Si può graficare queste coppie in due dimensioni e ottenere una heat map (o un grafico in tre dimensioni) dove la terza componente indica la quantità di punti simili. Le regioni più colore nella heat map rappresentano gruppi di componenti connesse.
- Si può graficare anche i due valori separatamente: il grafico del grado avrà un massimo vicino all'angolo 0° (indica l'inclinazione delle righe) e uno vicino all'angolo 90° (indica la verticalità delle colonne); il grafico della distanza avrà un picco sulla distanza media dei caratteri, uno sulla distanza media delle parole e uno per la distanza media tra righe.
##### Delaunay triangulation
È un algoritmo bottom-up che risolve il problema (di quelli sopra) che si presenta quando ci sono caratteri con dimensione diversa o quando non è allineato orizzontalmente.
- Si scelgono dei punti generatori. Si possono considerare come generatori:
	- I centroidi delle componenti connesse.
	- Se abbiamo forme complesse si usano come generatori dei punti campionati dai bordi delle componenti connesse.
- Si partiziona lo spazio con le partizioni di Voronoi: la partizione $V(p_i)$, dove $p_i$ è l'$i$-esimo generatore, è l'insieme dei punti che sono più vicini a $p_i$ rispetto a qualunque altro generatore $p_j$ con $i\ne j$. Ottengo un insieme $V(P)$ di partizioni di Voronoi.
- Si calcolano le Delaunay triangulation, che è il duale delle partizioni di Voronoi: i nodi sono sempre i generatori; abbiamo un arco tra due nodi se questi appartengono a regioni di Voronoi comunicanti.
- Si eliminano gli archi che sono più lunghi secondo una qualche distanza.
##### XY tree
È un algoritmo top-down che si basa sul [profilo](projection%20profile.png) di un documento: si tratta di un istogramma della quantità di pixel neri calcolato sulle righe e/o sulle colonne.
L'idea è di [costruire un albero](XY%20tree.png) dove per ogni nodo i figli rappresentano divisioni orizzontali e verticali alternate del documento e dove tali divisioni sono fatte in presenza di una valle nel profilo.
L'estensione MXY divide sia in presenza di una valle che di un picco. Questo permette di considerare divisione anche quando abbiamo una linea verticale od orizzontale che divide delle regioni di documento.
# Analisi logica
Analizza il documento e le strutture fisiche per assegnare loro un qualche significato semantico (e.g., titolo, autori, abstarct). È quindi un problema di classificazione dove si assegna una etichetta a ogni regione.
##### Tecniche
- Rule-based
	Per identificare gli elementi si usano regole. Il problema è che questo funziona bene solo per determinate classi di task, perché le regole sono molto stringenti e gli approcci basati su regole non sono tolleranti al rumore.
	Esempi sono: il titolo ha un determinato font size; l'abstract ha una determinata posizione nella pagina; gli spazi o i tab hanno un determinato significato.
- Syntactic
	Attribuiscono una label a un blocco sulla base dal suo layout fisico o logico (i.e., in relazione con altri).
- Knowledge-based
	La struttura logica è definita sulla base di una qualche conoscenza di base, cioè in questo caso delle convenzioni di layout.
##### Livelli di classificazione
La classificazione logica può essere fatta con varie granularità
- Pixel
	Solitamente è una classificazione binaria. Un esempio è definire se un pixel appartiene a un testo oppure no. Per questi compiti si usano solitamente le convoluzioni.
- Zona
	Una finestra analizza una porzione del documento e un'etichetta è assegnata a tutti i pixel della finestra.
- Regione
	Dopo aver estratto una regione gli si assegna un'etichetta.
	Per questo compito solitamente si usa il self-supervised learning: si associa testo a regione di documento; si maschera il testo e si impara a predire il testo data la regione; a questo punto si fa fine tuning per classificare il testo come appartenente a una regione.
# Applicazioni
Le due tecniche di layout analysis fisica e logica sono utilizzate in molte applicazioni:
- Libri
	Una volta che un libro viene acquisito ([OCR](OCR.md)) ci sono vari task che posisamo definire:
	- Identificare capitoli, sezioni e sottosezioni.
	- Identificare l'abstract e linkarlo con i capitoli e le sezioni a cui fa riferimento.
- Ricevute
	Le assicurazioni devo ad esempio riconoscere il layout di una fattura che deve essere rimborsata. Le difficoltà sono: fatture con layout diversi hanno all'interno gli stessi dati; ci possono essere errori numerici.
	Gli errori numerici sono difficili da osservare perché non possiamo usare un contesto (un carattere sbagliato in una parola si vede bene; una cifra in un numero no).