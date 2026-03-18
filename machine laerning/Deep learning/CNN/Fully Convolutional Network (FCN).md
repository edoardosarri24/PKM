Una rete FCN è un tipo di [CNN](Convolutional%20neural%20network%20(CNN).md) usata nell'immage recognition per la segmentazione delle immagini, dove l'output è un'immagine delle stesse dimensioni dell'input dove ad ogni pixel è assegnata un'etichetta con ciò che rappresenta; non ci dice solo cosa è presente nell'immagine ma anche dove si trova.
# Architettura
Rispetto alle reti AlexNet-like come le [VGGnet](VGGnet.md) vengono rimossi i layer fully connected e sostituiti con [layer convoluzionali](machine%20laerning/Deep%20learning/CNN/Convoluzione.md) dove il kernel è $1\times1$.
##### layer convoluzionali $1\times1$
I layer convoluzionali $1\times1$ vengono usati per:
- Ridurre la dimensionalità dei features map senza alterare la dimensionalità dell'immagine. Permettono quindi di compattare le immagini senza ridurre il carico computazionale.
- Compattando l'immagine senza alterarne la dimensionalità permettono di aumentare la rappresentazione semantica.