L'obiettivo principale del MLP è quello di approssimare funzioni complesse, mappando un input in un output. Un MLP infatti, tramite un giusto numero di unità e un singolo layer interno (c'è anche la versione deep di questo teorema) può approssimare qualunque funzione definita in un[ compatto](Ottimizzazione.md#Insieme%20compatto) con una data precisione.
Di per se un MLP è usato per scopi di classificazione, ma oggi viene usato all'interno delle [CNN](Convolutional%20neural%20network%20(CNN).md).
##### Perceptron
Il perceptron è la prima rete neurale. È un algoritmo del 1958 usato per l'image recognition (classificatore).
- L'[algoritmo](perceptron%20algorithm.png) usa come funzione di attivazione la [funzione a scala](Neurone%20artificiale.md#Scala).
- Ha problemi quando il dataset non è linearmente separabile, ad esempio per lo XOR esclusivo.
# Struttura
Il [multilayer perceptron](multilayer%20perceptron.png) (MLP) è un'estensione del singolo perceptron. Si usano più layer, ognuno dei quali è costruito da più [neuroni](Neurone%20artificiale.md) (unità); quanti più layer interni ci sono quanto più la rete neurale diventa più profonda (deep).
L'output prodotto da un'unità, che sarà uno degli input delle unità del prossimo layer, è il valore restituito dal neurone che la definisce.
- La funzione per un MLP con un solo hidden layer è data da $\hat{y}(x)=\sigma(w_2^T\sigma(w_1^Tx+b_1)+b_2)$. Essendo un insieme di nueroni, questa è lineare a meno dell'activation function $\sigma$. Il MLP infatti non svolge operazioni lineari solo grazie all'activation function.
# Addestramento
Il MLP è detto feed-forward network.
In un primo momento i pesi vengono stabiliti in modo casuale e i bias inizializzati a 0. Per addestrare i pesi al fine di ottenere una loss minima si utilizza un algoritmo iterativo, come [SGD](Gradient%20descent.md#Stocastic%20Gradient%20Descent%20(SGD)) o Adam. Tali algoritmi si basano sul gradiente, che per funzioni complesse è impossibile da calcolare a mano; per questo scopo si usa la back propagation.
##### Backpropagation
La back propagation si basa su due passi: il passo forward pass passa i dati nella rete calcolando al loss tramite i pesi ottimizzati all'epoca precedente; il backword pass calcola le varianzioni intermedie (i gradienti) e li moltiplica tramite la chain rule, arrivando alla direzione di discesa usata per l'ottimizzazione dei pesi.
La back propagation quindi è l'algoritmo che calcola le derivate della funzione rispetto ai pesi tramite moltiplicazioni di derivate parziali, mentre SGD utilizza questo gradiente per aggiornare i pesi.
I problemi principali, che sono mitigati entrambi usando activation funnction non saturanti (come la [ReLU](Neurone%20artificiale.md#ReLU)), sono:
- Saturing units
	Se si usano quelle activation function che sono flat negli estremi ([Sigmoid](Neurone%20artificiale.md#Sigmoid) e [Tanh](Neurone%20artificiale.md#Tanh)), allora per input dopo un certo valore si otterrà sempre lo stesso risultato. In pratica le activation function sono poco sensibili alle variazioni degli input da un certo valore in poi.
- Vanishing gradient
	La backpropagation porta a moltiplicare molti (soprattutto se le reti sono molto deep) gradienti piccoli e quindi si può ottenere un gradiente nullo anche se poi non è così.
- Memoria
	Il forward pass si basa su un DAG: in teoria i valori ottenuti al passo precedente non servono per i calcoli futuri. Il problema è che questi devono essere tenuti in memoria perché saranno poi usati per il backword pass: se sono moltissimi allora la memoria della GPU esaurisce.
# Considerazioni
- Il suo vantaggio principale è quello di riuscire ad apprendere dalle features di dati generali senza bisogno di praticare un'ingegneria dei dati.
- I problemi principali invece stanno nella difficoltà della scelta degli iperparametri (profondità e ampiezza) e dell'esplosione dei parametri al crescere della complessità; per questo le CNN sono più adatte all'image recognition.