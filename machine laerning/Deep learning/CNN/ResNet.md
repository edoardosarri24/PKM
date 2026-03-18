Le residual net sono delle [CNN](Convolutional%20neural%20network%20(CNN).md) che hanno risolto il problema per cui aggiungere layer e andare più in profondità peggiorava le prestazioni.
I problemi maggiori delle reti molto profonde, oltre alla memoria non infinita delle GPU, sono quelli del vanishing gradient e dell'arrestarsi su certo minimi locali.
# Residual block
![resisual block](resisual%20block.png)
Supponiamo di avere una rete con un certo numero di layer pre addestrata; costruiamo ora una rete più profonda usando i pesi della rete precedente. Per non ottenere miglioramenti in teoria i nuovi layer dovrebbero almeno imparare l'identità. I blocchi residui permettono proprio di fare questo: se in un blocco di due convoluzioni non si ha un miglioramento allora il gradiente che arriva salta quel blocco e passa a quello precedente.
Matematicamente se abbiamo un input $x$ e il residual block apprende una funzione $F(x)$ allora l'input viene passato alla fine del blocco e viene sommato a F(x) prima dell'activation function [ReLU](Neurone%20artificiale.md#ReLU). Il passaggio di $x$ è detto residual connection.
# Architettura
![architettura resnet](architettura%20resnet.png)
Il problema blocco elementare è il residual block composto appunto da due [convoluzioni](machine%20laerning/Deep%20learning/CNN/Convoluzione.md). Molti residual block sono concatenati per arrivare a una rete molto profonda: siamo passati nel 2015 da 22 a 152.
Le residual connection che avvengono dopo un operazione di [maxpool](Pooling.md#Operazione) sono tratteggiate perché l'input $x$ (che è un tensore) non può essere sommato direttamente con $F(x)$, visto che dopo il pooling hanno dimensionalità diverse. Dobbiamo quindi portare $x$ alle dimensionalità di $F(x)$, ad esempio tramite una convoluzione con stride 2.
##### Bottleneck
Anche tramite i residui le reti hanno moltissimi parametri. Per ridurre il loro numero si può usare il concetto del [bottleneck](bottleneck.png) all'interno del residual block. Se il numero di canali in input e in output deve essere ad esempio 256, invece di fare convoluzioni su questo numero di canali, si riduce la profondità del tensore e poi si aumenta con convoluzioni 1x1x256x64 e poi 1x1x64x256; nel mezzo di fa la nostra convoluzione 3x3x64x64 (che è quella che doveva essere fatta).
In questo modo se facciamo la somma dei parametri vediamo che sono meno rispetto a due convoluzioni 3x3.
##### Ultimo layer
Alla fine di resnet non c'è il [FCL](MultiLayer%20Perceptron.md) (come faceva [AlexNet](AlexNet.md)), ma solo un classificatore lineare $W^T\vec{x}+b$ che porta da un tensore a 2048 canali in 1000 classi finali (1000 perché il Imagenet aveva 1000 classi), cioè $W\in\mathbb{R}^{1000\times2048}$. Prima di usare il classificatore si deve vettorializzare il tensore che esce dall'ultimo residual block; questo viene fatto tramite 2048 [avgpool](Pooling.md#Operazione) con finestra la dimensione del tensore, che produce quindi un tensore $1\times1\times2048$.
Questo permette di non dipendere dalla dimensionalità dell'input, che può essere di qualunque dimensione (abbatanza grande per passare in tutti gli strati di pooling e non sparire in dimensionalità). L'addestramento comunque si fa con immagini dove la dimensione è sempre la stessa.
# Problemi risolti
##### Overfitting
Aggiungendo più layer nelle CNN si aumenta anche il numero di parametri, e questo porta all'overfitting nel caso in cui il dataset non abbia cardinalità che praticamente tende a infinito.
Tramite le resnet si permette di diminuire il numero di parametri.
##### Vanishing gradient
Se si hanno molti layer, durante la [backpropagation](MultiLayer%20Perceptron.md#Backpropagation) si può arrivare ad avere gradienti tendenti a 0 e questo porta a imparare molto poco durante i primi layer (che sono gli ultimi della backpropagation).
Le residual connection permettono di saltare layer durante la backpropagation e quindi di non far diventare i grandi troppo piccoli.
# ResNext
È una piccola variante delle resnet e ha l'obiettivo di ridurre ancora il numero di parametri senza peggiorare le performance.
All'interno del [residual block](resnext.png) si riduce prima il numero di canali grazie al concetto del [bottleneck](ResNet.md#Bottleneck) e poi si dividono i canali in blocchi e si applicano convoluzioni sui singoli blocchi; alla fine si concatenano i risultati e si fa il reverse bottleneck per aumentare nuovamente il numero di canali.