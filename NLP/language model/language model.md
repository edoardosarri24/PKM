Un language model è un modello probabilistico usato nell'[NLP](NLP.md) che predice una distribuzione di probabilità su un vocabolario di parole.
Ad esempio, data la sequenza $W=w_1,\cdots,w_n$, vogliamo poter calcolare $P(W)$ (la probabilità di quella esatta sequenza) oppure $P(w_n|w_1,\cdots,w_{n-1})$ (la probabilità della prossima parola data la sequenza).
# Chain rule approch
La probabilità condizionata è $P(B|A)=P(A,B)/P(A)$ che si può scrivere come $P(A,B)=P(A)P(B|A)$.
Per il nostro problema possiamo usare quindi la [chain rule](probabilità%20condizionata.md#Chain%20rule) e ottenere $P(x_1,\cdots,x_n)=P(x_1)P(x_2|x_1)\cdots P(x_n|x_1,\cdots,x_{n-1})$ e quindi $P(x_1,\cdots,x_n)=\displaystyle\prod_iP(x_i|x_1,\cdots,x_{i-1})$.
##### Problema
Il problema di questo è che potrei non conoscere una delle probabilità condizionate: più una sequenza è lunga in realtà e più è possibile che nel training set quella sequenza esatta non ci sia.
# Markov hypotesis (k-gram model)
Per risolvere questo problema si usa un'ipotesi markoviana, cioè si suppone che $x_i$ dipenda solo dalle $k$ parole precedenti. Abbiamo quindi $P(x_1,\cdots,x_n)\approx\displaystyle\prod_iP(x_i|x_{i-k},\cdots,x_{i-1})$: stiamo approssimando $P(x_i|x_1,\cdots,x_n)\approx P(x_i|x_{i-k},\cdots,x_{i-1})$.
Osserviamo che quello che facciamo è una specie di $k$-gram sulle parole e non sui caratteri: questi modelli si chiamano unigram se $k=1$, bigram se $k=2$ e così via.
##### Problema
- Il linguaggio ha dipendenze molto lontane e considerare $k$ piccoli non funziona perché la prossima parola solitamente dipende da molte altre prima. Usando gli unigram si generano parole a caso, dove magari ci sono più stop word rispetto alle parole con informazione; già con due si ottengono frasi con un minimo senso.
- Con $k$ troppo grande si torna al problema iniziale: la probabilità di una frase esatta è bassa o non addirittura quella frase nel train set non esiste; moltiplicando tra loro molte probabilità basse si potrebbe avere un underflow oppure se c'è una probabilità nulla si ha 0.
	Per risolvere questa cosa ci sono due tecniche:
	- Una tecnica per risolvere questo è il Laplace smoothing: si somma 1 al numeratore e $|V|$ al denominatore: $P(w_i|w_{i-1})=\tfrac{c(w_{i-1}w_i)+1}{c(w_{i-1})+|V|}$.
	- Si usa meno contesto: se un $k$-gram non è presente nel train set allora si usa un $k-1$-gram e così via. Questo gestisce l'assenza di una frase troppo lunga, ma non l'assegna di una parola.
	- Si usa meno contesto pesando i risultati (interpolazione): $P(w_n|w_{n-2}w_{n-1})=\lambda_1P(w_n|w_{n-2}w_{n-1})+\lambda_2P(w_n|w_{n-1})+\lambda_3P(w_n)$, dove i $\lambda_i$ sono iperparametri appresi da un validation set.
- Con $k$ troppo grande si ha un overffitting rispetto a quelle occorrenze trovate e quindi non si ha generalizzazione.
##### Bigram
Se consideriamo $k=2$, per calcolare le probabilità dei bigram si utilizza la Maximum likelihood Estimate (MLE) come $P(w_i|w_{i-1})=\tfrac{c(w_{i-1}w_i)}{c(w_{i-1})}$ usando il conteggio.
# Neural language model
I NLM sono [language model](language%20model.md) che utilizzano reti neurali al loro interno. I primi sono stati i [MLP](MultiLayer%20Perceptron.md) (non da sapere); poi abbiamo avuto soluzioni basate sulle [RNN](RNN.md) e infine oggi abbiamo soluzioni basate su [transformers](transformers.md).
##### Features
Se nei primi LM le features venivano rappresentate a mano, in questo caso si utilizza il representation learning: data una sequenza di parole la rete costruirà la rappresentazione delle parole in modo automatico (e.g., tramite [word2vec](word2vec.md)) durante l'addestramento. Durante l'inferenza da una frase si passerà a una serie di embedding e poi questi saranno gli input dell'MLP.