Vediamo delle famiglie di plot che permetto di spiegare i modelli black box.
# Ceteris paribus Plot
La traduzione è "a parità di tutte le altre condizioni". Si tratta di una tecnica model agnostic  e [outcam explaination](Metodi.md#Outcam%20explaination) usata nel campo dell'[Explainability](Explainability.md).
##### Idea
![Ceteris paribus Plot](Ceteris%20paribus%20Plot.png)
Si parte da un esempio di test e una features.
Si generano dei nuovi esempi variando la feature considerata. Si passano nella rete questi nuove istanze di test e si ottengono delle probabilità (o valori a seconda del compito della rete) per ognuna.
Si plotta la probabilità della classificazione in relazione alla variazione della features.
##### Conseguenze
A seconda del contesto se variamo delle feature che definiscono strettamente un'istanza si può perdere la correlazione con l'istanza di test.
Ad esempio se a un paziente iniziamo a cambiare età e sesso si ottiene tutta un'altra cosa.
La soluzione è valutare le cose giuste. Se per esempio agli estremi dei valori ci sono dei valori strani nel grafico allora vuol dire che si è persa la correlazione.
##### Tipologie
A seconda se la feature è discreta o continua si hanno dei grafici diversi:
- Continua
	Dato un punto $\hat{x}$ e una feature da usare $j$:
	- Si crea una griglia di valori $z_1,\cdots,z_k$, dove $z_1=min(x_j)$ e $z_k=max(x_j)$ con $x_j$ appartenente al train set e $k$ è un iperparametro.
	- Per ogni $z_i$ si crea un nuovo esempio di test come $\tilde{x}=\hat{x}_{x_j=z_i}$, si calcola $f(\tilde{x})$ e si plotta il punto $(z_i,f(\tilde{x}))$.
	- Si aggiunge al grafico l'esempio di test originale, mettendo in evidenza.
- Categorica
	È la stessa cosa di prima ma il grafico diventa un bar plot.
	I valori assunti dalla feature variata sono tutti i possibili valori che quella feature assume nel train set.
##### Libreria
- [dalex](https://dalex.drwhy.ai)
# Individual Conditional Expectation (ICE)
Si tratta di fare un singolo plot contenente tutti i plot ceteris paribus per tutti gli esempi di test data una singola features: si blocca la features e si plottano molte curve in un solo plot.
Il risultato è un singolo plot con molte curve. Questo permette di avere un'idea del trend ma possono essere difficili da leggere.
##### Centered ICE
Per migliorare la leggibilità si può scalare ogni curva (ogni esempio di test) in modo che parta da zero: si sottrae a ogni valore assunto dall'esempio $i$-esimo il delta $f_i(0)$, cioè il valore che quell'esempio di test assume in 0. Questo plot derivato prende il nome di "centered ICE".
##### Libreria
- Scikitlearn
# Partial dependence plot (PDP)
Si tratta di fare la media dei ceteris parius plot (o dell'ICE) per tutti i samples di test.
##### Due features
Si può osservare anche la variazione di due features contemporaneamente.
Il risultato è un plot in tre dimensioni: dove sugli assi orizzontali abbiamo le features; ogni cella del piano ottenuto è un valore eventualmente colorato secondo la magnitudine (heatmap).
##### Libreria
- Scikitlearn