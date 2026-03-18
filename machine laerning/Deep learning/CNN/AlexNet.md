w	È una [CNN](Convolutional%20neural%20network%20(CNN).md) nata nel 2012, ed ha segnato la storia nell'immage recognition. Si basa sul database Imagenet.
# Architettura
![architettura alexnet 2](architettura%20alexnet%202.jpg)
Prende in input immagini RGB di 227x227x3. È composta poi da 8 layer:
- 5 layer di [convoluzioni](machine%20laerning/Deep%20learning/CNN/Convoluzione.md), di cui 3 di questi sono questi (1-2-5) da [max polling](Pooling.md#Operazione).
- 3 layer fully-connected, cioè un [MLP](MultiLayer%20Perceptron.md) con due hidden layer.
- Alla fine è applicata la [softmax](Neurone%20artificiale.md#Softmax) per ottenere le probabilità della classe.
![architettura alexnet](architettura%20alexnet%201.png)
- Activation function
	Dopo ogni convoluzione e ogni hiddel layer del MLP si applica l'activation function [ReLU](Neurone%20artificiale.md#ReLU).
	Permette una velocità di addestramento 6x rispetto alla [sigmoide](Neurone%20artificiale.md#Sigmoid).
# Overfitting
Nonostante la convoluzioni riduca il numero dei parametri grazie ai pesi condivisi, AlexNet deve ancora allenare circa 60M di parametri. Questo ovviamente porta al fenomeno dell'overfitting, anche se il dataset ha moltissimi samples.
Per ridurre il problema dell'overfitting si utilizzano due tecniche di regolarizzazione:
- [Data augmentation](Tecniche%20CNN.md#Data%20augmentation)
- [Dropout](Tecniche%20CNN.md#Dropout)
- Ensable
	Alex per ridurre la varianza ha addestrato più modellisuigli stessi dati e poi ha fatto un ensamble.
	Ad esempio addestriamo lo stesso modello per 2 volte (solitamente 5 è un buon numero) e usiamo SGD con due seed diversi (usando lo stesso seed il random generator genera li stessi minibatch durante l’addestramento) per calcolare i batch casualmente e poi facciamo un ensamble.
# Considerazioni
##### Trick
- Momentum
	Invece che usare la versione classica di [SGD](Gradient%20descent.md#Stocastic%20Gradient%20Descent%20(SGD)) i usa una versione con momentum. Questo porta ad avere un'addestramento più veloce.
- [Minibatch DG](Gradient%20descent.md#Minibatch)
	Avendo moltissimi salmples non è possibile usarli tutti per calcolare il gradiente e poi aggiornare i pesi, ma se ne usa un sottoinsieme.
- Usare più GPU
	Al tempo di Alex i dati non entravano tutti in una GPU, quindi ha separato i dati in due e optato per usare due GPU. Oggi non sarebbe stato necessario.
- Local Response Normalization (LRN)
	È una tecnica di normalizzazione usata per non dare troppa importanza ad alcuni neuroni (e ridurre quindi la varianza), incentivando quindi la competizione tra neuroni.
	Avviene diminuendo i valori delle activation function alti e aumentando quelli più basso.
##### Vantaggi
- Prestazioni molti migliori. Prima di AlexNet si otteneva un miglioramenti annuale di un punto percentuale usando SVM con kernel. Grazie alla CNN di Alex il tasso di errore è passato dal 26% al 15%.
##### Svantaggi
- L'elevato numero di parametri e il probabile overfitting.