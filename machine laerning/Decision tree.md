I decision tree nel [Machine learning](Machine%20learning.md) sono modelli predittivi non parametrici usati nella classificazione con dataset tabulari, cioè dove ogni sample è un vettore di più features.
Ogni nodo rappresenta una variabile (una feature) e gli archi uscenti da essa rappresentano possibili valori per quella variabile; una foglia è la classificazione per un sample, stabilita tramite una [regola](regola%20decision%20tree.png), cioè da un path che dalla radice va al nodo foglia. Per questo motivo sono adatti solamente a features di tipo categorico.
##### Linearità
Gli alberi di decisioni sono molti decisamente non lineari.
Se i dati fossero linearmente separabili, allora l'albero dovrebbe apprendere moltissime regole per definire un decision boundary che assomiglia a una [retta](linearità%20decision%20tree.jpeg) fatta a gradini.
##### Interpretability
L'albero di decisione è il massimo dell'[interpretability](Interpretability.md): ogni classificazione (rappresentata da una foglia) può infatti essere spiegata dal path (cioè dalla regola) che abbiamo seguito per arrivarci.
Ovviamente più l'albero è profondo meno è interpretabile; questo comporta che vorremmo regole di classificazione non complesse (cioè non profonde, che attraversano pochi livelli).
# Addestramento
Per imparare le regole, un albero di decisione solitamente usa degli algoritmi greedy (es: cart, C4.5).
Gli algoritmi di questo tipo prendono la miglior decisione possibile in quel momento, cioè ottimizzano la decisione locale e non quella globale.
##### Conseguenze
A causa della proprietà greedy degli algoritmi all'interno dell'albero si possono avere sotto alberi che si ripetono; questo è causato dalla valutazione sulla stessa feature (i.e., i valori su cui partizionare saranno gli stessi) in due livelli diversi.
La conseguenza è che gli alberi di decisione, in generale, non sono ottimi per quanto riguarda la compattezza.
##### Idea
L'algoritmo di addestramento può essere riassunto così:
- Si definisce uno [score](Decision%20tree.md#Score/Metriche).
- A un dato livello avremmo un sottoinsieme dei dati iniziali. Si sceglie la miglior feature in base allo score su quei dati.
- Sulla base di questo attributo si partizionano i dati in più batch (almeno due).
- Si genera così più figli e si ripete il processo.
- Si arresta quando il criterio di convergenza, determinato dall'algoritmo, è raggiunto.
##### Overfitting
L'albero di decisione può arrivare ad avere un'accuracy del 100% sul train set: finché ogni sample non è classificato correttamente si definisce un nuovo livello.
Questo ovviamente porta perdere di generalizzazione, cioè porta all'overfitting: su dati mai visti l'albero si comporterà molto male.
# Score/Metriche
La bontà di una features (cioè il punto due dell'algoritmo) viene valutata secondo varie metriche.
##### Idea
A livello intuitivo vogliamo una regola che porti una sbilanciamento massimo tra le classi nei nodi figli: se all'inizio la distribuzione di samples relativa alle classi a cui appartengono è 50/50 (i.e., in un nodo il 50% dei campioni appartengono alla classe positiva e l'altro 50% a quella negativa), vorremmo una regola che definisce due partizioni dove la distribuzione è 100/0 e 0/100; non vogliamo invece una regola che genera due partizioni con distribuzione 25/25 e 25/25, perché questa non migliora nulla e aggiunge solo profondità.
##### Metriche
Tra le metriche più usate ci sono:
- Gini index
	È un indice di purezza, cioè spiega con un reale quanto la distribuzione dei sample è sbilanciata in una partizione.
	Matematicamente è definito come $gini(N)=\sum_{i=1}^B\rho_i(1-\sum_{k=1}^Cp_{ik}^2)$, dove $\rho_i$ è la percentuale di samples nell'$i$-esimo figlio, $B$ è il numero di figli, $C$ il numero delle classi e $p_{ik}$ è la percentuale di esempi di classe $k$ in $i$.
	Si può osservare come se nel nodo $i$-esimo ci sono solo samples della classe $k$-esima, allora $\sum_{k=1}^Cp_{ik}^2=1$ e $gini(N)=0$.
	Vogliamo quindi scegliere l'attributo che minimizza il gini index.
- Entropia
	L'entropia di Shannon si può vedere come la quantità di informazione presente in un'entità.
	Matematicamente è definita come $\sum_{i=1}^{|C|}p_i\cdot log\ p_i$, dove $|C|$ è il numero di classi possibili e $p_i$ è la probabilità che un’istanza appartenga alla classe $i$-esima, cioè $p_i=\tfrac{n_i}{n}$, dove $n$ è il numero totale di istanze e $n_i$ è il numero di istanze che appartengono alla classe $i$-esima.
	Con un'entropia massima si ha con la distribuzione 50/50; vogliamo quindi scegliere l'attributo che minimizza l'entropia.