Si tratta di un algoritmo per trovare gli [embedding statici](embedding.md#Statici).
# Idea
L'idea si basa su addestrare un classificatore binario, in particolare una logistic regression, a predire se una parola $w$ è vicina a una parola target.
I pesi del modello sono la rappresentazione semantica, cioè gli embedding.
##### Self-supervised
Si basa sul [self supervised learning](Self%20supervised%20learning.md) (non da studiare, solo cosa è) perché in realtà il task di classificazione non serve a nulla, ma è utile solo per apprendere la rappresentazione.
# Addestramento
Word2Vec non è altro che una rete con un input, un hidden layer e poi un output layer. L'input e l'ouput sono della dimensionalità del vocabolario $|V|$; l'input è un vettore in onehot; l'output rappresenta la probabilità che una parola sia nel contesto della parola target; l'hidden layer definisce una matrice di pesi $W_{in}\in\mathbb{R}^{|V|\times d}$ (i.e., i pesi in entrata nell'hidden), dove $d$ è la dimensione del vettore di hidden layer e la dimensione degli embedding.
##### Algoritmo
Per l'addestramento, una volta raccolti i dati, si:
- Assegna a ogni parola un vettore di embedding casuale, cioè si inizializza la matrice dei pesi della rete in modo casuale.
- Si esegue SGD in modo da ottimizzare la loss.
	La loss è definita come $L=-log[P(+|w,c_{pos})\prod_{i=1}^kP(-|w,c_{neg_i})]$, dove $P(+|w,c_{pos})$ è la probabilità che $c_{pos}$ sia nel contesto di $w$; viceversa per $P(-|w,c_{neg_i})$.
##### Probabilità
Per calcolare la probabilità che $w$ e $c$ siano nello stesso contesto, si usa questo ragionamento: se $w$ e $c$ sono nello stesso contesto allora i loro embedding sono simili; se gli embedding sono simili allora il loro prodotto scalare sarà alto, cioè $Sim(w,c)\propto w\cdot c$; per normalizzarli e prendere le probabilità si usa la regressione logistica, cioè la sigmoide; possiamo allora scrivere $P(+|w,c)\approx\sigma(w\cdot c)=\tfrac{1}{1+exp(-c\cdot w)}$.
##### Loss
In questo modo la loss diventa $L=-[log\ \sigma(c_{pos}\cdot w)+\sum_{i=1}^klog\ \sigma(-c_{neg_i}\cdot w)]$. Osserviamo che questa massimizza la similarity delle coppie $(w,c_{pos})$ e minimizza quella di $(w,c_{neg})$.
##### Dati
Per i dati ci servono esempi positivi, cioè parole $c_{pos}$ che sono simili al target $w$, e parole $c_{neg}$ che non lo sono.
- Per le parole simili è facile: basta scorrere i documenti e vedere i vicini.
- Per i negativi si campiona valutando anche la frequenza: se la parola target $w$ è frequente allora si dovrà scegliere una non vicina che però è anch'essa frequente; solitamente per ogni esempio positivo si considerano $k$ esempi negativi.