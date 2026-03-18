Una convoluzione calcola la somma pesata dei valori vicini rispetto al centro e la sua formula è data da $h_i=\sigma(\beta_i+w_1x_{i-1}+w_2x_i+w_3x_{i+1})$ (per una convoluzione con un canale e size 3), dove $\sigma$ è una [activation function](Neurone%20artificiale.md#Activation%20function). In pratica è una combinazione lineare dei valori presenti nella finestra seguita da un'activation function.
L'output della convoluzione è detto features maps. 
Quando si scorre la finestra i pesi restano gli stessi: quando si esegue una convoluzione su un input i pesi sono sempre gli stessi.
##### Variazioni
Ci sono vari [variazioni](variazioni%20su%20una%20convoluzione.png) che possiamo applicare a una convoluzione, e questi variano in base a:
- Finestra/kernel: input su cui si fa la convoluzione.
- Size: numero di input considerati.
- Stride: scorrimento della finestra a ogni passo. Se $n$ è lo stride allora la dimensionalità dell'input viene ridotta di un fattore $\tfrac{1}{n}$.
- Dilatation: quanti input sono saltati all'interno della convoluzione.
##### Border effect
È il problema in cui gli input estremi non ci sono.
Si risolve o considerandoli 0, oppure partendo da input centrali che permettono di considerare tutti gli input.
##### Dimensione output
Sia una input di dimensione $I$ e una finestra dimensione $K$, un padding di $p$ e uno stride di $s$ allora l'out avrà dimensione $\tfrac{I-K+2p}{s}+1$.
##### Numero parametri
Il numero di parametri di una convoluzione, dato un input $w\times h\times c$ e un output  $u\times v\times c'$, è esattamente $(u\times v\times c\times c')+c'$.

# Tipi
##### Multichannel
Una [convoluzione mutichannel](convoluzione%20mutichannel.png) si trattano più vettori, o in input o in output. Nel caso di RBG abbiamo 3 canali.
- Quando ci sono più canali in input si usa un vettore di pesi per ogni canale e poi si combinano in un'unica operazione lineare, applicando poi l'activation function.
- Quando ci sono più canali in output invece ogni canale viene prodotto da pesi diversi applicati al vettore di input.
##### Multidimensional
Quando si parla di multichannel si deve pensare a più vettori posti uno dietro l'altro.
Nel caso della convoluzione multidimensionale si deve pensare invece alla situazione in cui ogni canale non è composta da un vettore, ma da un elemento di $\mathbb{R}^n$; solitamente $n=2$, cioè una matrice.
# Obiettivo
Le convoluzioni sono usate per fare una combinazione lineare dell'input seguita da una funzione di attivazione non lineare.
- L'obiettivo è sicuramente quello di aiutare la rete ad apprendere caratteristiche non lineare dell'input.
- Un altro obiettivo è quello di mappare lo spazio (profondità) inziale in un nuovo spazio di features. Questo viene fatto per vari scopi, come ridurre il numero di parametri su cui fare convoluzioni grandi.