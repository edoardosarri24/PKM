L'adversarial learning si occupa della robustezza del modello, sia da un punto di vista degli attacchi sia per quanto riguarda una classificazione sbagliata delle classi non presenti nel dataset.
# Tipi di attacchi
##### Conoscenza
A secondo della conoscenza del modello si dividono in:
- White box
	Sono gli attacchi dove consociamo il modelli. Questo permette di ottenere i pesi e il gradiente. Un esempio è [FGSM](Fast%20Gradient%20Sign%20Method%20(FGSM).md).
	Non sono molto utili nella pratica ma sono validi per testare il modello.
- Black box
	Non conosciamo il modello e possiamo osservare solo l'output del modello. Questo output può essere sia la risposta ma anche i logits.
	Solitamente non si attacca direttamente questo modello ma un modello surrogato che si comporta come il modello bersaglio.
- Grey box
	È una via di messo degli altri due: si conosce ad esempio l'architettura o solo una parte dei pesi.
	È utile nella realtà perché si conosce ad esempio solo il tipo di modello (es: [ResNet](ResNet.md)).
##### Target
Si dividono in:
- Untargeted
	L'obiettivo è semplicemente ingannare il modello a prendere la decisione sbagliata, qualunque essa sia.
	Ad esempio un classificatore classifica un input in una qualunque altra classe diversa da quella corretta.
- Targeted
	L'obiettivo è quello di far sbagliare il modello in modo mirato.
	Ad esempio si vuole mimizzare la loss rispetto a una classe tarhet in modo che il modello predica per un dato input quella classe.
# Adversial attack
Un adversial attack è una piccola manipolazione dell'input, non visibile a occhio nudo, che porta la rete a una classificazione errata con una corretta confidenza. Si esegue su una rete già addestrata.
##### Funzionamento
L'input di un modello ha una precisione finita e per questo motivo classificare $x$ o $x+\eta$ dovrebbe produrre lo stesso output se $||\eta||_\infty<\epsilon$.
Se guardiamo a un classificatore lineare allora $w^T(x+\eta)=w^Tx+w^T\eta$, che è massimo per $\eta=\epsilon sign(w)$.
Dato $x$ un vettore in $n$ dimensioni, questa quantità cresce come $\epsilon nm$, dove $m$ è la media degli $x_i$: osserviamo che non cresce con l'errore $\eta$ ma con la dimensionalità, quindi una variazione infinitesimale di $x$ può portare a grandi cambiamenti nell'output del modello.
Per decidere come variare l'input abbiamo varie tecniche, una di queste è il [Fast Gradient Sign Method](Fast%20Gradient%20Sign%20Method%20(FGSM).md).
##### Robustezza
Per superare il priblema degli esempi avversari la soluzione è allenare una rete che minimizzi la loss anche rispetto a questi.
Un esempio potrebbe essere definirla come $L(\theta,x,y)=\alpha L(\theta,x,y)+(1-\alpha)L(\theta,x+\epsilon sign(\nabla_xL(\theta,c,y_{true})))$.
##### Ensamble
Un buon modo per difenderci dagli attacchi è usare un ensamble.
In questo modo si aumenta la robustezza del modello e si richiede all'attaccante di conoscere e sfruttare tutti i modelli che formano l'ensamble.
Nella pratica gli ensamble non aumentano molto la robustezza del modello, ma sono molto utili per aumentare la forza dell'attaccante.
Infatti si può usare più modelli per addestrare perturbazioni che catturano meglio le direzioni in cui perturbare.
##### One pixel attack
Si può eseguire un adversial attack non modificando tutta l'imagine, ma solo una sua porzione. In questo caso dobbiamo risolvere il problema $max p(y|x+\epsilon)$, con $||\epsilon||_0<d$. Si usa la norma 0 per indicare il numero massimo (in questo caso $d$) di elementi che devono essere diversi da zero.
Per trovare immagini di questo tipo si usano algoritmi evolutivi: si crea una popolazione con pixel randomicamente variati; si valuta quanto l'output del modello cambia; si tengono solo gli esemplari migliori e gli altri si eliminano, si continua finché non si trova un singolo pixel (o una porzione) che causa una classificazione sbagliata.
# Data poisoning
Gli attacchi di tipo data poisoning sono mirati a infettare il dataset di training; si svolgono quindi durante l'addetsramento e non durante l'inferenza.
##### Obiettivi
Gli obiettivi sono:
- Si peggiorano le performance globali.
- Si vogliono generare errori mirati, come far classificare le immagini di una classe in un altra.
- Aumentare i tempi di addestramento.