Le più grandi differenze tra il deep learning e le classiche tecniche di machine learning sono:
- Nelle Deep Neural Network (DNN) ci sono più hidden layer. Una classificatore o regresso invee si può vedere come una neural network con zero hidden layer, cioè con un vettore di input e un vettore (composto da un valore se è regressione o un vettore se è classificazione) di output. Questo perchè un classificatore (come SVM ad esempio) calcola $\hat{y}=sign(w^Tx+b)$ che è esattamente la funzione di una neural network con zero hidden layer.
- Le tecniche di addestramento sono diverse: nei MLP si usa [GD/SGD](Gradient%20descent.md) e la [backpropagation](MultiLayer%20Perceptron.md#Backpropagation); nelle tecniche di machine learning si [minimizza una funzione convessa](Ottimizzazione.md) con algoritmi iterativi.
- Più le reti neurali sono profonde più perdiamo interpretabilità: ogni hidden layer porta a una nuova rappresentazione dei dati e quindi non abbiamo una relazione diretta tra input e output; nei modelli classici avendo un solo layer si riesce a stabilire una relazione più diretta tra input e output.
# Tecniche
- Supervised learningn
	I dati di training sono completamente annotati.
- [Continual learning](Continual%20learning.md)
- [Self supervised learning](Self%20supervised%20learning.md)