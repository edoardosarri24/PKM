Dopo l'uscita di [AlexNet](AlexNet.md) la Visual Geometry Group (VGG) dell'università di Oxford ha perfezionato il lavoro di Alex lavorando sugli iperparametri della [CNN](Convolutional%20neural%20network%20(CNN).md) proposta da Alex.
Hanno definito delle reti più profonde rispetto agli 8 layer di AlexNet, in particolare VGG16 e VGG19 che mostrarono come avere reti più profonde migliorava le prestazioni.
# Architettura
Le VGGnet sono AlexNet-like, ma definiscono uno standard di parametri. Il numero di layer varia da rete a rete, ma le più famose sono le VGG16 (16 layer) e VGG19 (19 layer):
- Gli input sono tesori di dimensione fissa $224\times224\times3$.
- Le [convoluzioni](machine%20laerning/Deep%20learning/CNN/Convoluzione.md) sono solo $3\times3\times d$ o $1\times1\times d$, dove $d$ è un numero arbitrario di features map. Queste sono più efficienti delle convoluzioni di AlexNet e diminuiscono il numero di parametri. L'idea è che se vogliamo una convoluzione più grande basta andare più in profondità tramite il [polling](Pooling.md).
- L'operazione di polling è il [maxPolling](Pooling.md#Operazione) non-overllapping con finestre $2\times2$ e stride 2.
- L'activation function utilizzare è la [ReLU](Neurone%20artificiale.md#ReLU).
# Addestramento
- L'inizializzazione dei pesi viene fatta in modo casuale e tramite pre training.
- Si usa mini batch [SGD](Gradient%20descent.md#Stocastic%20Gradient%20Descent%20(SGD)). Il learning rate è inizializzato come $10^{-2}$. Ogni certo numero di epoche si controlla l'accuracy sul test set: se questa diminuisce di controllo in controllo allora si lascia uguale; altrimenti si dimuisce di un ordine di grandezza ($10^{-3}$) quando l'accuracy sul test set smette di migliorare. L'addestramento finisce dopo circa 75 epoche.
# Considerazioni
- Non si usa la [LRN](AlexNet.md#Trick) utilizzata da AlexNet, in quanto è stato visto che non portava vantaggi.
# Input
- Prima dell'addestramento è stata fatta una normalizzazione della media $\mu$: per ogni immagine X si è calcolato $X'=X-\mu$. Non è stata fatta nessuna normalizzazione della varianza, cioè non si è diviso per la deviazione standard.
	- Centrare i dati in modo che abbiano una media zero porta a una convergenza più veloce.
	- L'activation function [ReLU](Neurone%20artificiale.md#ReLU) funziona meglio se i dati sono centrati intorno allo zero.
- Le immagini prima di essere passate in input sono state scalate a $224\times224$. Questo avviene tramite due passaggi:
	- Si scala isotropicamente l'immagine a 256, cioè data un'immagine $x\times y$, con $x$ lato più piccolo, si ottiene un'immagine $\tfrac{256}{x}(x\times y)$. Questo permette di ridimensionare l'immagine senza perdere le proporzioni originali.
	- Si croppa (ritaglia) casualmente un'immagine di $224\times224$.