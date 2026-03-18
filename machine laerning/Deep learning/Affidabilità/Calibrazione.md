Gli [Adversarial attack](Adversarial%20learning.md) derivano dal fatto che le reti neurali sono overconfidence nella predizione a causa della [softmax](Neurone%20artificiale.md#Softmax), che tende a massimi il risultato per una classe e minimizzare le altre anche se le predizioni sono sbagliate.
##### Definzione
In questo contesto la calibrazione è una metrica che misura quanto possiamo riporre fiducia nella confidenza di un classificatore, cioè la sua affidabilità.
Si dice che una rete è perfettamente calibrata se per una predizione che ha confidenza $p$ in una classe, allora in media quella classe è classificata correttamente con probabilità $p$. Ad esempio se abbiamo 100 predizioni con confidenza media 0,8 allora ci aspettiamo che 80 siano corrette; se solo 60 sono corrette allora la rete è overconfident.
# Reliability diagram
Vediamo costruire un diagramma di confidenza che misura l'affidabilità di un modello.
- Si dividono le predizioni in bin in base alla confidenza. Se ad esempio abbiamo bin di ampiezza 0,1 vuol dire che le predizioni saranno divise in 10 bin da 0,1; in pratica abbiamo un bin per ogni 10 punti percentuali di confidenza.
- Si calcola l'accuracy per ogni bin come il rapporto tra le predizioni corrette su tutte quelle che ricadono.
- Si calcola la media della confidenza all'interno di ogni bin.
##### Osservazioni
- Perfezione
	Si ha una calibrazione perfetta se $acc(B_i)=conf(B_i)$, cioè se l'accuracy e la confidenza all'interno di un bin sono uguali.
- Visualizzazione
	Si mette sulle ascisse le confidenze medie e sulle ordinate l'accuracy.
	L'affidabilità perfetta è la diagonale; sopra la diagona abbiamo underconfidence; sotto la diagonale abbiamo overconfidence.
# Expected Calibration Error (ECE)
La ECE è una misura scalare che ci perfette di definire la calibrazione di un modello.
Valori piccoli di ECE indicano una buona calibrazione.
##### Distribuzione campioni
La ECE si basa sulla distribuzione dei campioni nei bin del reliability diagram. Il numero di campioni che finiscono in ogni bin è variabile a seconda della confidenza della rete per ogni esempio.
Se abbiamo una grande discrepanza tra confidenza media e accuratezza in un bin che contiene pochi campioni, questo è un dato poco interessante, è più rilevante invece quando abbiamo una certa discrepanza in un bin in cui ricadono molti campioni.
##### Definizione
Si calcola come una media pesata delle discrepanze tra confidenza media e accuracy di ciascun bin, cioè come $ECE=\displaystyle\sum_{i=1}^{M} \frac{|B_i|}{n}\cdot|acc(B_i)-conf(B_i)|$, dove: $M$ è il numero di bin, $|B_i|$ è il numero di campioni nel bin $i$-esimo e $n$ è il totale delle predizioni.
# Metodi
Per migliorare la calibrazione nelle reti neurali ci sono diverse tecniche.
Queste si dividono in parametriche e non parametriche.
##### Histogram Binning
È una tecnica di calibrazione non parametrica basata sugli istogrammi usata per problemi di classificazione binaria.
Dividiamo le predizioni in bin $B$ di uguale dimensione; per ogni bin $B_i=[a_i,a_{i+1})$ definiamo uno score di calibrazione $\theta_i$ che rappresenta la media delle etichette positive nel bin; si minimizza l'errore quadratico per trovare i $\theta_i$ come $\displaystyle\min_{\theta_1,\cdots,\theta_M}\sum_{m=1}^M\sum_{i=1}^n1(a_m\le\hat{p}_i<a_{m+1})(\theta_m-y_i)^2$.
Questo serve a correggere la confidenza media iniziale del modello con la reale osservazione dei positivi.
Le conseguenze sono:
- Non ci sono parametri da imparare.
- È sensibile al numero di bin.
- Il numero di bin è fisso e non si adatta ai dati.
##### One-vs-rest
È l'estensione di Histogram Binning per problemi multi classe. Si segue la filosofia di one-vs-rest: si addestrano $k$ classificatori binari, uno per ogni classe; ognuno di questi predice se l'esempio è della sua classe o no; ognuno usa la tecnica sopra per migliorare la calibrazione. 
##### **Platt’s Scaling**
Si tratta di un metodo parametrico.
L'idea è quella di addestrare un modello di regressione lineare che mappa i logits ottenute dalla softmax con valori che migliorano la calibrazione.
Formalmente la probabilità $q_i=\sigma(az_i+b)$, dove: $z_i$ è la predizione originale e $a$ e $b$ sono parametri da imparare tramite la minimizzazione di una loss che non tocca i pesi della rete.