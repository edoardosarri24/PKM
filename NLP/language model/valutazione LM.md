Per valutare un [language model](language%20model.md) ci serve un corpus che faccia da test set.
# Tecniche
Le tecniche mostrate non sono correlate: migliorare una non vuol dire migliorare l'altra.
##### Estrinseca
È la migliore soluzione per comparare due modelli in uno scenario reale ed è detta anche task-based evaluation. Ci serve un sistema completo in cui eseguire il modello e delle metriche.
Il problema è che non sempre questa è possibile: manca il sistema completo; è costosa in termini di tempo e costi; non è generalizzabile ad altre applicazione se non a quella sotto studio.
##### Intrinseca
Si basa su statistica e matematica. La metrica usata si chiama perplexity e misura la qualità delle predizioni del LM sul test, cioè quanto il modello è sorpreso di vedere una sequenza di parole: si misura la probabilità che il corpus di test (l'istanza del test set) appaia sul modello A e sul modello B e si guarda quale funziona meglio.
- Calcolo
	Si calcola come l'inverso della probabilità della frase di test, cioè $perplexity(W)=P(w_1,\cdots,w_n)^{-\tfrac{1}{n}}=\displaystyle\sqrt[n]{\prod_{i=1}^n\tfrac{1}{P(w_i|w_1,\cdots,w_{i-1})}}$: si sta normalizzano la probabilità della frase per la sua lunghezza, in modo da non avvantaggiare frasi più corte. Si nota che più bassa è la perplexity migliore è il modello.
- Problema
	Il problema è che se gli LM vengono addestrati su tutto il web allora è molto difficile introdurre nel test set cose mai viste.
##### Campionamento
È un'altra tecnica basata sulla generazione che permette di valutare cosa il modello ha imparato.
L'idea è che un LM rappresenti una distribuzione di probabilità del prossimo token data una certa finestra di dimensione $k$. Per valutare la conoscenza appresa da un LM si può campionare dalla sua distribuzione. Campionando stiamo scegliendo il valore (i.e., il token) che massimizza la likeliwod che il LM ha imparato.
# LLM
Valutare un [LLM](LLM.md) può essere molto più difficile di quello che si crede. Oltre a quanto sopra c'è il problema dell'overffiting and text contaminization: trovare dataset che non sono inclusi nel training set può essere molto difficile. Spesso si devono creare nuovi dataset, che però poi finiranno nel training set del modello successivo.