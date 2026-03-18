![neurone artificiale](neurone%20artificiale.png)
Il neurone artificiale è alla base delle più complesse reti neurali.
Il suo funzionamento è molto semplice: gli input (vettore) vengono moltiplicati per i pesi $w$; vengono poi sommati tramite il prodotto scalare e si passa il risultato ad un'activation function.
# Activation function
Il neurone applica una funzione che è lineare a meno dall'activation function. Se questa è non lineare allora l'operazione nel suo complesso è non lineare. È utile quando si voglio apprendere relazione nei dati che non sono lineari.
##### Saturazione
Una funzione di attivazione è saturante quano ilsuo valore negli estremi è poco suscettibile (o non lo è affatto) alle variazioni degli input.
Se consideriamo la [sigmoide](Neurone%20artificiale.md#Sigmoid) e ne calcoliamo la derivata vediamo che questa ha valore massimo quando $x=0$ e tende a 0 quando $x\to\pm\infty$. S e invece consideriamo la [ReLU](Neurone%20artificiale.md#ReLU) vediamo che la derivate è costante a 1 se $x>0$ e nulla se $x<0$. Quando aggiorniamo i pesi in una [CNN](Convolutional%20neural%20network%20(CNN).md) usando [GD](Gradient%20descent.md), se abbiamo gradienti tendenti a 0 allora la l'aggiornamento dei pesi avverrà molto lentamente, soprattutto nei primi layer.

##### Funzioni
Le activation function più note sono:
![activation function](activation%20function.png)
##### Scala
È definita come $f(x) =\begin{cases} 1 & \text{se } x>0 \\ 0 & \text{altrimenti} \end{cases}$.
##### Sigmoid
È definita come $\sigma(x)=\tfrac{1}{1+e^{-x}}$.
Ha il problema che è saturante, cioè da un certo punto in poi i valori sono troncati a un valore limite.
##### ReLU
È definita come $\sigma(x)=max(0,x)$.
Non è una sfunzione saturante come la sigmoide, perché i valori grandi non vengono troncati ad un valore limite.
##### Tanh
È la tangente.
È saturante.
##### Softmax
È definita come $\sigma(x_i)=\tfrac{e^{x_i}}{\sum_i e^{x_i}}$.
In pratica è una normalizzazione con l'obiettivo di ottenere un vettore di probabilità. Invece che usare il valore normale si usa l'esponenziale per dare una spinta al più probabile.
È usata soprattutto nell'ultimo layer delle [CNN](Convolutional%20neural%20network%20(CNN).md) nei problemi multi classe. Trasforma un vettore di valori reali in un vettore di distribuzione delle probabilità; infatti la somma dei valori del vettore di output è 1.
In relatà non fa proprio questo, perché tende a essere overconfidence: mette quasi tutte le probabilità a zero tranne qualcuna che è molto alta.
##### ExU
È definita come $\sigma(x)=e^w(x-b)$.
È usata nei [neural additive models](Generalized%20additive%20models%20(GAMs).md#Neural%20additive%20models) al posto della ReLu. Questa funzione permette di osservare variazioni impornati nell'output con piccoli cambiamente dell'input, a seconda di come sono settati $w$ e $b$.