Le Reccurrent Neural Network (RNN) sono [neural language model](language%20model.md#Neural%20language%20model) usate prima dei [transformers](UniFi/NLP/language%20model/transformers.md).
# Architettura
![RNN architecture](RNN%20architecture.png)
L'architettura di una RNN si basa sugli [MLP](MultiLayer%20Perceptron.md) (la scatola blu nell'immagine sopra). Nella figura vediamo più MLP, ma in realtà è lo stesso oggetto in istanti di tempo consecutivi.
Sono reti usate per processare sequenze: al tempo $t$, considera l'informazione proveniente dall'[embedding](embedding.md) di $x_t$ e da $out_{t-1}$, cioè da quanto appreso fino a quel momento.
La funzione applicata all'output del MLP (i.e., quella che produce $y_i$) dipende dal task che vogliamo risolvere: ad esempio per un [next word prediction](next%20word%20prediction.md) sarà una softmax sul vocabolario.
# Training
Per il training si utilizza il self-supervised learning, dove il task fittizio è il next word prediction: al tempo $t$ si genera la distribuzione di probabilità sul vocabolario $P(w_{t+1}=(i|w_1,\cdots,w_t)=y_t[i], \forall i)$, dove $y_t[i]$ rappresenta l'$i$-esima posizione della softmax al tempo $t$.
Ogni passo dell'addestramento consiste in:
- Far predirre al modello la distribuzione della prossima parola e calcolare una loss per ogni parola predetta. Si usa la [cross-entropy loss](Funzioni%20di%20loss.md#Cross-entropy%20loss), che in questa circostanza è definita come $CE_t=-\displaystyle\sum_{w\in V}y_t[w]log\hat{y_t}[w]$. La corretta distribuzione del prossimo token è un vettore in one-hot dove è posta a 1 la probabilità dell'effettiva prossima parola.
- Si considera come loss totale, quella su cui poi si applica back propagation, la media delle loss su ogni istante $t$, cioè su ogni token generato. Prendere la media e non la somma totale è una specie di normalizzazione che non penalizza sequenze lunghe.
- Aggiornare i pesi della rete tramite GD.
##### Teching forcing
Le RNN sono allenate su sequenze: al tempo $t$ viene fornito l'input $x_t$ e l'output al tempo $t-1$; se questo output è sbagliato allora l'errore si propaga e l'apprendimento è più lento.
Per evitare questo si usa il teaching forcing: l'input successivo è dato dal token $x_t$ e dall'output al tempo $t-1$ corretto (quello vero).
##### Backpropagation through time
Per l'aggiornamento dei pesi si usa [GD](Gradient%20descent.md).
Questo deve tornare indietro non solo nei layer del singolo MLP, ma tra tutte le istanze che sono state usate durante la sequenza (che hanno gli stessi pesi). Quando si torna indietro tra le varie istanze quello che si fa è sommare i gradienti dei vari pesi; quando siamo attiva al tempo $t=0$ allora si aggiornano i pesi del MLP con l'aggiornamento di GS: $x^{k+1}=x^k+\alpha_kd_k$.
# Problemi
Ci sono dei problemi principali con le RNN.
##### Vanishing gradient
Nelle RNN durante l'addestramento il gradiente passa lungo tutti gli istanti temporali della sequenza, cioè tra tutti i MLP che sono stati usati in ogni istante di tempo. Sappiamo che la back propagation utilizza la regola della catena: per ottenere il gradiente che determina lo spostamento dei pesi, si moltiplicano tra loro le derivate parziali ottenute in ogni layer; questo viene fatto per ogni istante di tempo $t$. Più lontano si va nel tempo più si ha quindi il problema del vanishing gradient, cioè il gradiente diventa 0 e i primi istanti temporali non contribuiscono all'aggiornamento dei pesi.
##### Memoria
Il problema del vanishing gradient può essere visto anche come un problema di memoria: il contesto nelle RNN non può essere molto amplio prorpio per la loro architettura profonda in cui il gradiente deve passare troppi layer e troppi MLP. Per risolvere questo problema sono state ideate le Long Short-Term Memory RNN (LSTMs), che separano la memoria a breve termine da quella a lungo termine.
##### Sequenzialità
Sia l'addestramento che l'inferenza con le RNN avviene in modo sequenziale. Non avere possibilità di parallelizzare un algoritmo di questo tipo dove i dati analizzati sono molti è un problema.
# Bidirectional RNN
Le RNN bidirezionali non analizzano la frase in modo causale (da sinistra verso destro) ma abbiamo due RNN che la scorrono in parti opposte. Per questo motivo il loro utilizzo non è per fare next word prediction, ma per la [text classification](text%20classification.md) o comunque per task dove si necessita di analizzare tutta la frase e poi prendere una decisione.
##### Output
Le due RNN scorrono le sequenze da sinistra verso destra e viceversa, producendo una rappresentazione per ogni elemento della sequenza, cioè restituiscono lo stato interno del MLP che viene usato in ogni istante di tempo $t$. L'output finale della bidirectional RNN al tempo $t$ in questo modo è costituito dalla concatenazione delle due rappresentazioni.
Possiamo collegare queste rappresentazioni a un MLP e fare previsioni.
# Multilayers RNN
Le RNN possono essere viste come reti profonde in una direzione, quella temporale da sinistra a destra; possiamo però pensare di aumentare la profondità anche nell'altra direzione, cioè aggiungendo layer impilati uno sopra l'altro; in questo modo l'output $y_t,\forall t$ non diventa l'input della softmax o di qualcosa che restituisce un risultato, ma diventa l'input $t$-esimo di un'altra RNN.
##### Osservazioni
- Le reti di più basso livello imparano features di basso livello: relazioni tra parole adiacenti, strutture grammaticali semplici, punteggiatura, o la morfologia. I layer più alti imparano la semantica delle parole e il senso della frase.
- Aggiungere residual connection può migliorare le performance se aumentatiamo la profondità.
# Long Short-Term Memory
Sono un'estensione delle RNN che mitigano il problema del vanishing gradiente per sequenze molto lunghe.
Oltre all'hidden state prodotto dal MLP al tempo $t-1$, l'input del MLP a tempo $t$ anche quanto viene dal cell state, che possiamo vedere come un nastro che trasporta la memoria a lungo termine. Questo è possibile perché il cell state subisce solo piccole trasformazioni lineari durante il percorso.
##### Gates
![LSTM](LSTM.webp)
Le informazioni nel cell state possono essere eliminate, aggiunte o modificate. Questo è possibile grazie a tre gates:
- Forget gate
	Decide cosa eliminare e cosa mantenere dal vecchio cell state.
	Prende in input l'hidden state $h_{t-1}$ e l'input $x_t$ e calcola una trasformazione lineare seguita dalla sigmoide (i.e., $f_t=\sigma(W\cdot[h_{t-1},x_t]+b)$) e a questo punto nel layer passa $f_t*C_{t-1}$. Restituisce quindi un valore in $[0,1]$, dove 1 dimentica l'intero valore e 0 mantiene totalmente il contenuto.
- Input gate
	Decide quali informazioni vengono aggiunte al cell state.
	Osservando $h_{t-1}$ e $x_t$ si calcola $C_t=f_t*C_{t-1}+i_t*\tilde{C_t}$, dove $i_t=\sigma(W_i\cdot[h_{t-1},x_t]+b_i)$ e $\tilde{C}_t=\tanh(W_C\cdot[h_{t-1},x_t]+b_C)$.
- Output gate
	Decide cosa mostrare al MLP al tempo $t$-esimo, e lo fa moltiplicando un'altra sigmoide identica a quella precedente, che prende sempre $h_{t-1}$ e $x_t$, per la $\tanh$ di quanto è adesso presente nel cell state.