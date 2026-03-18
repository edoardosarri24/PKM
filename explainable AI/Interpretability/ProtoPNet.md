Ci sono dei modelli, come le K-NN, che sono definite a partire da modelli di ML detti Case-Based reasoning. Questi prendono una decisione sulla base di quelle prese per esempi simili del dataset.
Un modello nel campo dell'[interpretability](Interpretability.md) che utilizza questa idea è detto [ProtoPNet](https://arxiv.org/abs/1806.10574).
##### Interpretabilità
È interpretabile perché possiamo vedere quali prototipi sono associati all'immagine su cui si è fatta inferenza.
# Architettura
![ProtoPNet](ProtoPNet.png)
L'architettura è divisa in tre blocchi:
- CNN
	È una CNN $f$ che lavora da features extractor, i cui pesi sono chiamati $w_{conv}$.
	L'output di $f$ ha una dimensione $H\times W\times D$, dove ognuna di queste dimensioni è un iperparametro del modello. Nell'articolo suggeriscono di usare $H=W=7$ e $D=[128,256,512]$.
- Prototype layer
	È un layer $g_P$ di prototipi $P=\{p_j\}_{j=1}^m$, dove ogni prototipo $p_j$ ha una dimensione $H_1\times W_1\times D$, con $H_1\le H$ e $W_1\le W$.
	Un prototipo $p_j$ rappresenta un patch significativo del train set
	Il numero di prototipi è $m=m_k\cdot k$, cioè il numero di prototipi per ogni classe $k$ moltiplicato per il numero delle classi. Se abbiamo un dataset bilanciato allora vogliamo che ogni classe abbia più o meno lo stesso numero di prototipi; se il dataset è sbilanciato si possono considerare anche un numero di prototipi pesato nella cardinalità della classe.
- Fully Connected
	È un layer fully connected $h$, i cui pesi sono chiamati $w_h$.
# Inferenza
L'inferenza su un'immagine  $\hat{x}$ segue i segunti passi:
- Per prima cosa si calcola $z=f(\hat{x})$. Questo produrrà una features map $z$ che avrà una certa dimensionalità $H\times W$.
- Per ogni prototipo si scorre tutta la feature map e si guarda se (quanto, tramite un distanza) quel prototipo è presente. Otteniamo così una griglia per prototipo, i cui elementi sono le distanze del prototipo dalla patch dell'immagine considerata: l'elemento $(1,1)$ della griglia $i$-esima è la distanza del primo prototipo e della prima patch in alto a sinistra della feature map.
- A questo punto per ogni griglia si fa un [max pooling](Pooling.md#Operazione) e si estrae il pezzo di features map che è più vicino al prototipo.
- Infine per eseguire la classificazione passano i reali (le distanze) ottenuti dal layer $g_P$ a un fully connected layer.
##### Distanza
La distanza del prototipo dal patch dell'immagine è più uno score di similarità: più è alto più le immagini sono simili.
Questo si realizza con $s=\tfrac{1}{dist(\tilde{z},z)}$, dove $z$ è la feature map e $\tilde{z}$ è il prototipo considerato: più la distanza è piccola più lo score è elevato.
##### Prototype layer
Matematicamente l'operazione nel prototype layer per il prototipo $p_j$ è definita come $g_{p_j}(z)=\displaystyle\max_{\tilde{z}\in patch(z)}log\tfrac{||\tilde{z}-p_j||_2^2+1}{||\tilde{z}-p_j||_2^2+\epsilon}$: se la distanza (la norma) va a zero allora $g_{p_j}(z)=1/\epsilon$ e quindi lo score è massimo; se la distanza tende a infinito allora $g_{p_j}(z)=log1=0$, cioè lo score è nullo.
Osserviamo che $\tilde{z}\in patch(z)$ indica tutte le patch della feature map in uscita dal layer $f$.
# Training
Ci sono tre fasi distinte che sono iterate finchè non si ha una convergenza:
- Si congelano i pesi del layer FC e si addestrano i layer $f$ e $g_P$. Nella prima iterazioni, quando nessun peso è mai stato addestrato, esiste un modo efficacie che inizializza i pesi del layer $h$.
	La funzione obiettivo da minimizzare è definita da $\displaystyle\min_{w_{conv},\{p_j\}}\tfrac{1}{n}\sum_{i=1}^nL(h\circ g_p\circ f(x_i),y_i)+\lambda_1\cdot Clust+\lambda_2\cdot Sep$; osserviamo che la loss $L$ è in funzione della classe vera e della funzione di predizione, che è $h\circ g_p\circ f(x_i)$ perché l'input deve passare dai tre layer.
	I due regolarizzatori mi permettono di addestrare i pesi dei prototipi con la stessa idea che c'è nel clustering: si vuole massimizzare la distanza all'interno del cluster e massimare la distanza tra cluster diversi.
	- $Clust=\displaystyle\tfrac{1}{n}\sum_{i=1}^n\min_{j|p_j\in P_{y_i}}\min_{z\in patch(f(x_i))}||z-p_j||^2$. Per ogni prototipo sappiamo a quale classe è relativo. Nel minimo più esterno prendo il prototipo che appartiene alla classe dell'esempio in questione che minimizza la norma; nel secondo minimo prendo il patch che minimizza la norma tra tutti quelli che si applicano al risultato della convoluzione.
	- $Sep=-\displaystyle\tfrac{1}{n}\sum_{i=1}^n\min_{j|p_j\notin P_{y_i}}\min_{z\in patch(f(x_i))}||z-p_j||^2$. È l'opposto di quello sopra perché stiamo massimizzando rispetto ai prototipi che sono relativi alle classi a cui non appartiene l'esempio in considerazione.
- Con questo step, detto prototype projection, vogliamo che i prototipi siano patch non simili a parti di immagini presenti nel train set, ma che siano proprio patch di immagini presenti nel trainset.
	Questo si fa sostituendo ai prototipi le patch di immagini del train set che sono più simili possibile: $p_j=\displaystyle\arg\min_{z\in Z_j}||z-p_j||_2$, dove $Z_j=\tilde{z}|\tilde{z}\in patch(f(x_i)), \forall i|y_i=k$.
	C'è un teorema che assicura che questa sostituzione fa peggiorare la loss imparata in modo trascurabile.
- Si congelano i pesi dei layer $f$ e $g_P$ e si addestrano i pesi del layer $h$.
	La funzione obiettivo di questo layer è definita come $\displaystyle\min_{w_h}\tfrac{1}{n}\sum_{i=1}^nL(h\circ g_p\circ f(x_i),y_i)+\lambda\sum_{k=1}^K\sum_{j|p_j\notin P_k}|w_h^{(k,j)}|$, dove $K$ sono le classi.
	Il regolarizzatore impone di mandare a zero (per quell'esempio) i pesi che legano i prototipi con l'output che rappresenta la classe di cui i prototipi non si occupano.