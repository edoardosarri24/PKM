La traduzione del testo si basava su regole scritte a mano fino agli anni 90. Successivamente siamo passati ai [language model](language%20model.md) e poi ai [neural language model](language%20model.md#Neural%20language%20model). L'idea è di imparare un modello e, data una frase in input $x$, si restituisce $\arg\max_yP(y|x)$, cioè quell'output che massimizza la sua probabilità dato l'input.
Successivamente siamo passati a un approccio end2end (data una frase in input si restituisce una frase in output) basato [encoder-decoder](encoder-decoder.md) con architetture diverse.
# RNN
La prima architettura E2E fu quella basate sulle [RNN](RNN.md).
##### Inferenza
![rnn econder-decoder](MT%20rnn%20econder-decoder.png)
L'input viene elaborato (sotto forma di embeddings) dall'encoder come nel caso di una classica RNN, ma in questa situazione non applichiamo nessuna softmax all'ultimo hidden layer del MLP, passando al prossimi istante di tempo $t+1$ solo la rappresentazione appresa al tempo $t$.
L'output del MLP all'ultimo istante temporale (i.e., l'utlimo embedding della sequenza da tradurre) è una rappresentazione contestuale di quanto appreso da tutte le istanze della RNN.
La prima istanza del decoder prende come input il contesto e un token speciale che indica l'inizio della frase. Inizia a questo punto la generazione (autoregressiva) della traduzione prevedendo la prossima parola come una classica RNN, dove input al tempo $t$ è quanto prodotto da $t=1$ fino a $t-1$ e lo stato del MLP al tempo $t-1$.
Filosoficamente quello che stiamo facendo è usare la chain rule (come nei primi [language model](language%20model.md)) e calcolare $P(y|x)$ come $P(y|x)=P(y_1|x)P(y_2|y_1,x)\cdots P(y_m|y_{m-1},\cdots,y_1,x)$.
##### Memoria
Il problema di quanto descritto fino a questo momento è che il contesto perde potenza man mano che l'informazione passa nel decoder: le ultime istanze del decoder si baseranno quasi totalmente su quanto prodotto e non su quanto appreso dell'input.
Per risolvere questo problema si può pensare che ogni istanza $t$-esima del decoder prenda in input quanto generato fino a quel momento, lo stato precedente e [anche il contesto](MT%20rnn%20encoder-decoder%20context.png) appreso dall'encoder.
# Transformenrs
Successivamente siamo passati ad architetture basate su [transformers](transformers.md).
##### Architettura
![full-attention in MT encoder](full-attention%20in%20MT%20encoder.png)
Si tratta della classica ascritteura encoder-decoder.
- L'encoder è costituito da tanti [transformers](transformers.md) blocks, ognuno dei quali usa una [full-attention layer](attention.md#Granularità%20attention), in modo che la rappresentazione tenga conto dell'intero contesto e non solo di quello precedente. Restituisce una rappresentazione semantica di quanto appreso sull'input.
- Il decoder è anch'esso costituito da un blocchi transformers, ognuno dei quali usa la [cross-attention](attention.md#Granularità%20attention) per usare solo quanto preceduto.