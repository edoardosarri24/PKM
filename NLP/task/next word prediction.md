È un task dell'[NLP](NLP.md) dove si deve prevedere il prossimo token data una sequenza di token in input.
# MLP
La prima soluzione che usa un [neural language model](language%20model.md#Neural%20language%20model) è quella basata sui [MLP](MultiLayer%20Perceptron.md).
![next word prediction in NLM](next%20word%20prediction%20in%20NLM.png)
Come per i [language model](language%20model.md), l'idea è di scorrere una finestra e sulla base di questa si fa la predizione.
##### Svantaggi
- La lunghezza della finestra non può essere lunga all'infinito, motivo per cui si usa la [Markov hypotesis](language%20model.md#Markov%20hypotesis%20(k-gram%20model))
- Vorrei che gruppi di parole che stanno sempre insieme durante lo scorrimento della finestra mobile fossero considerate sempre nello stesso modo. Ad esempio se "the book is" sta sempre insieme vorrei che fossero considerate in questo modo sia che "the" sia in prima seconda o terza posizione nella finestra. Siccome però via via che la finestra si muove i pesi dei termini della frase non sono gli stessi questo non è possibile.
##### Vantaggi
- Le prestazioni sono migliori rispetto ai semplici LM.
- Usando gli [embedding](embedding.md) non si ha più il problema della sparsità.