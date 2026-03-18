Le CNN sono usate prevalentemente per compiti visivi.
Le operazioni usate nelle CNN sono le [convoluzioni](machine%20laerning/Deep%20learning/CNN/Convoluzione.md) e il [polling](Pooling.md).
Solitamente lavorando con le CNN ci basiamo su modelli già addestrati, [adattandoli](R-CNN.md) al compito che dobbiamo svolgere.
Le architetture principali sono [AlexNet](AlexNet.md), [[VGGnet]], [Fully Convolutional Network(FCN)](Fully%20Convolutional%20Network%20(FCN).md) e [ResNet](ResNet.md).
##### Storia
Il primo approccio all'image recognition è stato quello addestramento supervisionato: si cercava di estrarre feature che rappresentassero un certo oggetto e si addestrava un classificatore (solitamente SVM) a riconoscere queste features. Il problema con questa soluzione è che codificare ogni immagine con le sue relative features è troppo complesso, viste le molte situazioni diverse che possiamo avere.
Un altro approccio è stato quello di non usare un classificatore monolitico, ma usare un classificatore per ogni parte dell'immagine; queste parti poi sono collegate con delle molle, che sappiamo essere descritte da una funzione quadratica e quello che cercavamo di fare era limitare la distanza di queste parti.
Successivamente ci fu l'idea di Marr, che adotto un approccio gerarchico: partiva dal riconoscere ciò che delineava tutte le superfici, per poi arrivare a definire i punti che appartenevano alla stessa superficie e poi definire cosa le variesuperfici rappresnetavano.
##### Gap
- Sensor gap
	È la differenza tra l'oggetto nel monto reale e come questo viene captato dal sistema.
	Se ci pensiamo questo gap esiste anche nell'uomo: l'occhio non riesce a percepire come effettivamente è l'oggetto nel mondo reale. Quindi la risoluzione delle fotocamere o delle telecamere sicuramente falserà l'oggetto da come effettivamente è nella realtà.
- Semantic gap
	È la differenza tra il significato che l'uomo da un oggetto e come questo viene rappresentato in un sistema.
# Funzionamento
Le CNN lavora in modo sequenziale. Questo vuol dire che più si va in profondità, più si apprende il significato semantico della cosa, cioè ciò che definsice effettivamente l'immagine.
In pratica nei primi layer si distinguono i bordi e i limiti degli oggetti; più che si va in profondità e più che si capisce di cosa si sta trattando.
##### Grad-CAM
Uno strumento che permette di vedere come una CNN lavora è [GRAD-CAM](Gradient-base%20explainability.md#GRAD-CAM).
Dato un input si fa il forward pass, raggiungendo un insieme di probabilità; supponiamo che la classe che ha probabilità più alta sia gatto. Analizziamo la backpropagation per ogni neurone di ogni layer per capire quali pesi dovrebbero essere modificati maggiormente, cioè quali sono stati più importanti nella previsione di gatto. A questo punto per layer convoluzionale si fa un GAP (Global Average Pooling), producendo un tensore $1\times1\times D$, con $D$ numero di canali del layer; questo riduce le attivazioni di ogni canale a un singolo valore, che rappresenta quanto quella features (quel canale) è stata fondamentale nel determinare il risultato per quel layer. I valori GAP vengono poi usati per determinare quali aree dell'input sono state fondamentali per categorizzare l'immagine.