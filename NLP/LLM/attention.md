L'attenzione, nata con il paper [attention is all you need](https://arxiv.org/abs/1706.03762), è un particolare layer utilizzato nelle reti neurali, sopratutto nei [transformers](transformers.md) per ile poi negli [LLM](LLM.md).
# Architettura
![attention layer](attention%20layer.png)
Il layer dell'attenzione mappa una sequenza di input, o meglio una sequenza di input rappresentata secondo i propri [embedding](embedding.md), in una sequenza di output della stessa dimensione.
Per produrre l'output $k$-esimo, si compara l'input $k$-esimo con gli input precedenti (i.e, $x_1,\cdots,x_{k-1})$ per ottenere uno score di correlazione; questo permette di definire l'output in funzione dell'intero contesto precedente.
##### Granularità attention
A seconda di cosa osserviamo per produrre un output si hanno tipi di attention diversi:
- Self-attention
	Si osservano solo gli input della propria sequenza.
	- Casual attention
		Si guardano solo i token precedenti.
	- Full-attention
		Si osservano anche gli input successivi e non solo quelli precedenti.
		Questo ha senso non per task di next word prediction, ma solo se dobbiamo osservare tutta la frase.
- Cross-attention
	Si osservano gli input anche di altre sequenze, oltre alla propria. Un esempio è nei task di [traduzione](machine%20transaltion.md#Architettura) dove si osserva oltre alla frase che stiamo traducendo anche quella da tradurre.
##### Vantaggi
Rispetto alle architetture sequenziali (i.e., [RNN](RNN.md)) non abbiamo il concetto di stato precedente della sequenza. Non esistendo i collegamenti ricorrenti che trasmettono quanto appreso fino a quel punto della sequenza, abbiamo due grandi vantaggi:
- L'architettura è totalmente parallela: ogni risultato $y_k$ è indipendente dagli altri. Questo permette di eseguire i calcoli in parallelo e avere una computazione molto efficiente.
- Ogni output $y_k$ è calcolato accedendo direttamente a tutti i dati della sequenza precedenti (o anche successivi nella full-attention) e non basandosi solo su uno stato memorizzato fino a quel momento.
##### Positional embedding
Il vantaggio del calcolo parallelo è anche uno svantaggio: si perde l'informazione temporale delle parole tokenizzate, cioè il loro ordine (cosa che ad esmepio non succede nelle [RNN](RNN.md) per la loro sequenzialità).
Questa informazione viene aggiunta usando i [positional embedding](positional%20embedding.md).
# Output $y_i$
Abbiamo detto che l'attention produce un output $y_i$ per ogni input $x_i$ cercando una correlazione con gli input precedenti. Per ottenere una rappresentazione di questo concetto per un input $x_i$ si procede come segue:
- Si produce $score(x_i,x_j)=x_i\cdot x_j$.
	Più grande è questo prodotto scalare più gli embedding degli input sono simili e quindi più in correlazione.
- Si normalizza con una softmax per ottenere una distribuzione su tutti gli input $x_j$ con cui si è confrontato $x_i$, ottenendo $\alpha_{ij}=\displaystyle softmax(score(x_i,x_j))=\tfrac{exp(score(x_i,x_j))}{\sum_{k=1}^iexp(score(x_i,x_k))}$ per ogni $j\le i$, compreso quindi $x_i$ stesso.
	Si ha che $\alpha_{ij}$ è un peso su quanto $x_j$ è rilevante per l'attuale $x_i$ in esame.
- Si ottiene l'output come $y_i=\sum_{j\le i}\alpha_{ij}x_j$.
# Query-key-value
Rispetto a quanto detto sopra notiamo che ogni input $x_i$ ha tre ruoli nel processo, ognuno dei quali ha significato diverso.
- Query
	È l'attuale input in esame. Sarebbe $x_i$.
- Key
	È l'attuale input che stiamo comparando con la query. Sono tutti gli $x_j$.
- Value
	È l'input quando viene usate per ottenere l'output.
##### Spazzi
Siccome lo stesso $x_i$ ha questi tre ruoli diversi, ha senso che venga mappato, prima di essere usato, in tre spazi diversi.
- $q_i=W^Qx_i$
	Quando $x_i$ viene usato come query allora si mappa usando $W^Q$.
- $k_i=W^Kx_i$
	Quando $x_i$ viene usato come key allora si mappa usando $W^K$.
- $v_i=W^Vx_i$
	Quando $x_i$ viene usato come value allora si mappa usando $W^V$.
##### Rappresentazione $y_i$
![output attention](output%20attention.png)
A questo punto quanto descritto qui sotto diventa quello che vediamo nella figura (dove si usa $x_i$ come query):
- $score(x_i,x_j)= q_i\cdot k_j$
- $\alpha_{ij}=\displaystyle softmax(score(x_i,x_j))=\tfrac{exp(score(x_i,x_j))}{\sum_{k=1}^iexp(score(x_i,x_k))}$
- $y_i=\sum_{j\le i}\alpha_{ij}v_j$
# Multihead attention
![multihead attention](multihead%20attention.png)
È un'estensione dell'attention layer dove abbiamo più head con pesi (i.e., $W^Q$, $W^K$ e $W^V$) diversi.
Questo permette di avere $k$ rappresentazioni diverse dell'input: si può pensare che ogni testa sia specializzata per apprendere qualche rappresentazione.
Queste sono poi concatenate e riportare nuovamente nella dimensionalità dell'input.