I metodi grandient-base explainability sono tecniche di tipo [outcam explaination](Metodi.md#Outcam%20explaination) usate nel campo dell'[Explainability](Explainability.md).
- Sono metodi usati quando la black box è una rete neurale.
- È una tecnica molto usata per dati multimediali (es: immagini) e si può usare anche per dati testuali. Per dati tabulari non si usa molto e conviene usare altre tecnoche come [LIME](LIME.md) o [SHAP](SHAP.md).
# Idea
Questi metodi si basano sul capire quali features dell'input (i.e. pixel) hanno una maggiore influenza sull'output, cioè per quali features una loro piccola variazione provoca un grande cambiamento nell'output.
Per fare questo si calcola il gradiente rispetto all'input (e non rispetto ai pesi come nell'addestramento). Il modo in cui calcolare il gradiente definisce la tecnica utilizzata.
##### Mappa di salienza
Quello che si genera è una saliency map.
Una saliency map ha la stessa dimensionalità dell'input; ogni suo pixel è definito da un peso, che rappresenta l'importanza di quel pixel.
Solitamente l'output di questi metodi è l'immagine in input a cui è stata sovrapporta la mappa di salienza; questo permette, tramite la colorazione in base al peso, di capire quali pixel sono appunto più importanti.
##### Utilità
Le critiche mosse a questi metodi riguarda l'effettiva spiegazione che essi ci forniscono. 
Ci sono due test per valutare la bontà della spiegazione data dalla salincy map:
- Si confronta la saliency map prodotta dalla rete addestrata normalmente con quella ottenuta da una rete non addestrata (i.e. inizializzata casualmente). L'idea è che se non c'è differenza tra queste due saliency map allora le non possono essere buone spiegazioni per quel modello.
- Si confrontano le saliency map ottenute dal modello normalmente addestrato e dallo stesso modello addestrato sullo stesso dataset dove le etichette sono state permutate casualmente. L'idea è che se non c'è differenza allora le salincy map non possono essere buone spiegazioni per quel dataset.
- Vanilla, smooth grad e gradcam passano questi test. Guided cam non passano il test.
# Vanilla gradient
Definiamo $F_C(x)$ come l'ouput della rete neurale per la classe $C$. Ci focalizziamo quindi su una sola classe: se il problema è multi classe quindi ci focalizziamo su un singolo output della softmax.
L'idea è quella di approssimare l'output della rete con uno sviluppo di Taylor al primo ordine: $F_C(x)\approx w^Tx+b$, con $w=\tfrac{\partial F_C(x)}{\partial x}|_{x_i}$, cioè $w$ è il gradiente dell'output rispetto all'input calcolato in $x_i$; questa indica l'importanza del pixel $x_i$.
Avere un gradiente elevato in un pixel vuol dire che piccoli cambiamenti in quel pixel portano a grandi cambiamenti nell'output, cioè che quel pixel è molto importante.
##### Mappa di salienza
Dato l'input $I_0\in\mathbb{R}^{W\times H\times K}$, dove $K$ è il numero di canali (solitamente 3) si ottiene la mappa di salinza $M\in \mathbb{R}^{W\times H\times1}$. L'elemento $M_{ij}=\displaystyle\max_k|w_{ijk}|$, cioè è una specie di max pool tra i tre canali dell'input; in pratica, dato un pixel, si calcola il gradiente per i tre canali in modo indipendente e poi si prende il massimo.
##### Svantaggi
Questo metodo produce mappe di salienza rumorose. In pratica più o meno tutti i pixel sono importanti.
# Smooth-grad
Con questo metodo si vuole risolvere il problema della rumorosità delle salincy map generate dal vanilla gradient.
Si dice che questo metodo aggiunge rumore per togliere rumore.
##### Idea
A partire dall'input si generano tante sue permutazioni introducento un rumore $\epsilon$. A questo punto si applica su ognuna di queste il vanilla gradient e si fa la media tra i risultati ottenuto.
In pratica il gradiente si ottiene come $w=\tfrac{1}{N}\sum_{i=1}^N\nabla_xF_C(x+\epsilon_i)$, dove $N$ è il numero di copie e $\epsilon$ è il rumore.
# GRAD-CAM
Questo metodo, dove CAM sta per Class Activation Mapping, segue lo stesso principio del Vanilla gradient, ma ferma il calcolo del gradiente all'ultimo livello convoluzionale, cioè lo esegue solo sui layer fully connected.
L'informazione del gradiente non è quindi pixel per pixel, ma è a grana più ampia, nel senso che si ha un'informazione su patch di immagini che sono le rappresentazioni tirate fuori dai livelli convoluzionali e si riesce ad avere meno rumore.
##### Idea algoritmo
Suppongo una features map di dimensione $U\times V\times K$ restituite dall'ultimo livello convoluzionale.
Grad cam calcola $\tfrac{\partial F_C}{\partial A_{uv}^k}$ per ogni $k$ features map e per ogni $u$ e $v$ dimensione, dove: $A^k$ è la $k$-esima features map; $A_{uv}$ è un pixel della feature map, cioè la rappresentazione di una patch dell'immagine originale.
Si ottiene la salincy map della classe $C$-esima come $S_{GC}^C=ReLu(\sum_{k}\alpha_k^CA^k)$, dove gli $\alpha_k$ sono dei parametri calcolati da grad cam come segue:
- Si fa il forward pass, cioè si passa l'input tramite tutta la rete.
- Si prende $y^C$, cioè il valore prima del softmax per la classe $C$.
- Si settano a 0 gli score delle altri classi. In questo modo non danno un contributo al gradiente.
- Si fa la back propagation fino all'ultimo livello convoluzionale, ottenendo $\tfrac{\partial y^c}{\partial A_{uv}^k}$.
- $\alpha_k^C=\tfrac{1}{Z}\sum_{u,v}\tfrac{\partial y^C}{\partial A_{uv}^k}$, dove $Z$ è un fattore di normalizzazione (serve per portare i valori tra 0 e 1). Si ottiene quindi l'importanza della features map $A^k$ per la classe $C$.
- Si calcola infine $S_{GC}^C=ReLu(\sum_k\alpha_k^CA^k)$. La dimensione di $S_{GC}^C$ è più piccola di $U\times V$; per poterla mettere sopra l'input si fa un upsamples e si riporta alle dimensioni dell'input. Questo comporta che la spiegazione di grad cam sia più grossolana, cioè non pixel per pixel.
# Guided gradcam
L'idea è di combinare le spiegazioni fornite da [GRAD-CAM](Gradient-base%20explainability.md#GRAD-CAM) con quelle ottenute a livello del singolo pixel (e.g., [vanilla gradient](Gradient-base%20explainability.md#Vanilla%20gradient)).
Si ottiene $S_{GGC}=upsample(S_{GC})\circ S_{VG}$, dove $S_{GGC}$ è la salincy guided gradcam, $S_{GC}$ è la saliency grad cam, $S_{VG}$ è la saliency vanilla gradient e $\circ$ è il bitwise operation.
In pratica stiamo mettendo inseme la grana fine di vanilla con la grana grossolana di grad cam.