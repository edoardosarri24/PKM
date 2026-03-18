Durante la generazione di un [LLM](LLM.md), quindi di un modello [decoder](encoder-decoder.md) come [GPT](GPT.md), la prossima parola viene scelta in modo stocastico. Questa parte è quella che fa si che i modelli siano appunto stocastiche non deterministici.
# Tecniche
Per decidere quale debba essere il prossimo token generato ci sono varie tecniche.
##### Greedy search decoding
La strategia più semplice è il greedy search decoding, dove si determina $\hat{y}_t=\displaystyle\arg\max_{w\in V}P(w|y_{<t})$, cioè s restituisce quel token $w$ del vocabolario che massimizza la probabilità condizionata da quanto il modello già visto o generato.
Il problema di questo approccio è che si generano frasi con bassa entropia, cioè sempre le stesse e quindi con poca informazione.
##### Beam Search Decoding
Si genera un albero $b$-ario (dove ogni nodo ha $b$ figli); per ogni figlio si prosegue a generare (fino a un certo punto) e poi si scartano i percorsi (i.e., le sequenze di token) che hanno la $\hat{y}$ più bassa.
Produce risposte migliori rispetto alla greedy search deconding, ma anche in questo caso si tende a generare sempre le stesse sequenze di parole e quindi si hanno frasi con bassa entropia.
##### Random sampling
L'approccio è sempre lo stesso della greedy search deconding, ma invece che selezionare il token che massimizza la probabilità data la sequenza già generata, questo viene campionato da tale distribuzione.
La distribuzione sul prossimo token solitamente ha una coda molto lunga: ci sono poche parole molto probabili e quasi tutti le parole invece sono poco prabili. Il problema è che la probabilità di campionare da questo insieme di parole poco probabili non è trascurabile; in questo caso si è un'allucinazione.
##### Top sampling methods
Si esegue un random sampling su un sotto insieme del vocabolario. La scelta di questo sottoinsieme definisce il metodo:
- Top-$k$
	Si campiona secondo la loro distribuzione dalla prime $k$ parole più probabili.
	È utile se la distribuzione della prossima parola è molto simile a una delta del di Dirac; tanto più la distribuzione è flat quanto più stiamo escludend parole probabili.
- Top-$p$
	Si campiona secondo la loro distribuzione da tutte quelle parole la cui probabilità cumulata è $p$.
	Stiamo sostanzialmente mantenendo $k$ (del metodo top-$k$) dinamico; è solitamente un buon metodo.
# Temperature
Se guardimao come viene generata la distribuzione di probabilità in un modello generativo come [GPT](GPT.md#Inferenza), abbiamo che $P(y_t=w_i|y_{<t},x)=softmax(z_{t,i})=\tfrac{exp(z_{t,i})}{\sum_{j=i}^{|V|}exp(z_{t,j})}$. Possiamo rendere la distribuzione più piatta aggiungendo come iperparametro la temperatura $T$ e definendo $P(y_t=w_i|y_{<t},x)=\tfrac{exp(\tfrac{z_{t,i}}{T})}{\sum_{j=i}^{|V|}exp(\tfrac{z_{t,j}}{T})}$.
In questo modo si avvicinano le probabilità dei token più probabili, in modo che il modello sia più incerto sul prossimo token da generare. Se $T<1$ allora le risposte si avvicinano a essere deterministiche; se $T=1$ abbiamo il greedy search decoding; se $T>1$ il modello può inventare maggiormente.