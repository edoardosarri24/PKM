Generative Pre-Trained Transformer (GPT) è un [LLM](LLM.md) [decoder-only](encoder-decoder.md) basato sull'architettura dei [transformers](transformers.md).
# Storia
I punti di svolta sono stati GPT2 e GPT3.
##### GPT2
Nella storia di GPT il punto di svolta è stato GPT2. Con questo si è capito infatti che eliminando la parte [encoder](encoder-decoder.md), aumentando la dimensione del decoder e allenando il modello su una grande quantità di dati, si poteva instillare nei pesi una grande quantità di consocenza. Il fatto di allenare il modello su moltissimi dati è stato possibile grazie al passaggio dal supervised all'unsupervised: nei modelli basati su encoder i dati dovevano essere annotati per l'addestramento (e.g., in BERT si usa il [Next Sentence Prediction](BERT.md#Next%20Sentence%20Prediction) task durante l'addestramento, che richiede di annotare le frasi); con GPT si inizia a passare tutto il testo che vogliamo e questo, usando il testo vero per determinare un errore, stabilisce l'errore e aggiorna i pesi tramite back propagation.
##### GPT3
Se GPT2 (1.5B di parametri) da l'idea su come creare un buon modello, allora GPT3 permette di scalare tale idea: il modello è molto più grande (175B di parametri) e il corpus su cui è fatto addestramento è molto maggiore.
##### Vantaggi
Con questa idea sulla dimensione del modello e sull'utilizzo del unsupervised-learning, si sono ottenuto numerosi vantaggi:
- [In-context leaning](prompting.md#In-context%20leaning)
	Il modello si adatta al prompt e può rispondere al taskche gli viene assegnato. Questo si può vedere come una sorta di fine tuning per un task specifico.
- Zero-shot learning
	Non c'è bisogno di mettere nel prompt esempi su cosa vogliamo, ma sarà il modello a contestualizzarlo rispetto a cosa ci serve. Si può vedere come una specie di self-supervised learning senza esempi etichettati.
##### GPT4
Ha introdotto:
- Multimodal LLM
- Chain-of-Tought (CoT)
	Internamente (i.e., senza che l'utente faccia nulla) il prompt viene suddiviso in task e il model.
# Input
L'input di GPT dipende dall'iterazione in cui siamo: all'inizio è solo il prompt che inseriamo a cui facciamo la fase di [pre-processing](pre-processing.md) e lo mappiamo negli input e positional [embedding](embedding.md); dopo la prima iterazione l'input al modello diventa il prompt e il nuovo token generato (tutto tokenizzato e in formato embeding).
# Architettura
![GPT architecture](GPT%20architecture.png)
- Tokenizzazione
	Si usa l'algoritmo [BPE](Byte%20Pair%20Encoding%20(BPE).md).
- Embedding
	I positional embedding appresi durante l'addestramento e non si usano quindi tecniche algoritmiche come seno e coseno.
- Masked multi head attention
	Si usa una masked attention, in modo che quanto appreso dagli hidden layer siano relazioni causali.
	Si usa il termine masked per indicare che nell'addestramento (nell'inferenza è ovvio che sia così) si usa una [self-attention](attention.md#Granularità%20attention) causale (non fully); masked in questo caso si riferisce al fatto che l'attenzione della query è rivolta solo alle key precedenti.
- Linear
	Dopo i blocchi di attention si porta l'output alla dimensione voluta tramite una trasformazione lineare.
- Softmax
	Per ottenere una distribuzione di probabilità sul vocabolario e poter prevedere il prossimo token si usa una softmax. Questa viene fatta sulla riga relativa all'iterazione in questione TODOOO.
# Training
Durante il training l'obiettivo è, data una sequenza $U=\{u_1,\cdots,u_N\}$, massimizzare la log-likelihood $L_1(U)=\displaystyle\sum_ilog\ P(u_i|u_{i-k},\cdots,u_{i-1};\Omega)$, dove $\Omega$ sono i parametri della rete e $k$ è la dimensione del contesto.
- Il primo layer è definito da $UW_e+W_p$, dove $U\in\mathbb{R}^{k\times |v|}$ (con $k$ lunghezza della sequenza) è la sequenza, $W_e\in\mathbb{R}^{|v|\times d}$ (con $d$ dimensione degli embedding) è la matrice di embedding e $W_p\in\mathbb{R}^{k\times d}$ è la matrice degli embedding posizionali. Osserviamo che $UW_e\in\mathbb{R}^{k\times d}$ non è implementata come una moltiplicazione algebrica ma una lookup operation: essendo $U$ una matrice di vettori in one-hot, dove ogni vettore rappresenta una parola della sequenza, tale prodotto permette di ottenere il vettore degli embedding per ogni parola nella sequenza.
- La probabilità è definita come $P(u)=softmax(h_nW_e^T)$, dove $h_n$ è l'ultimo hidden layer dell'ultimo blocco transformers. Il prodotto $h_nW_e^T$, con la trasposta di $W_e$, serve a riportare l'output di questo hidden layer nella dimensione del vocabolario per poi generare una distribuzione di probabilità sul vocabolario. Quello che si ottiene è una matrice in $\mathbb{R}^{k\times|v|}$: ogni riga corrisponde a una parola, cioè a ogni istante della sequenza ,; ogni colonna corrisponde al logits di un token.
# Inferenza
Durante l'inferenza, data $x=\{x_1,\cdots,x_k\}$ come sequenza in input, vogliamo massimizzare $P(y|x)$, dove $y=\{y_1,\cdots,y_t\}$ è la sequenza di output.
Per farlo usiamo la chain rule $P(y|x)=\displaystyle\prod_tP(y_t|y_{<t},x)$, dove $y_{<t}$ indica tutti i token generati prima del $t$-esimo.
L'implementazione di questo meccanismo si basata sulla rappresentazione della probabilità durante l'addestramento: se $P(u)=softmax(h_nW_e^T)$ durante il training, allora ha senso definire $P(y_t=w_i|y_{<t},x)=softmax(z_{t,i})$, dove $z$ è la matrice dei logits sul vocabolario (i.e., la dimensione delle righe è quella del vocabolario) prodotta dall'ultimo hidden layer $h_n$ e quindi $z_{t,i}$ rappresenta la riga dell'iterazione in corso in cui consideriamo il valore della colonna $i$-esima (i.e., quella della parola $i$-esima del vocabolario).
##### Terminazione
La generazione termina quando viene prodotto il token $endOfSequence$ ($EOS$). Questo è fattibile perché il modello ha appreso quando la sequenza generata ha un senso abbastanza compiuto e allora genera questo token.