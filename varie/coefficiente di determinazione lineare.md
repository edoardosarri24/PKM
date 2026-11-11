Il coefficiente di determinazione $R^2$ si usa quando vogliamo capire quanto un insieme di dati è proporzionale a un altro secondo una regressione.
# Matematica
Dato un set di coppie $(x_{i},y_{i})$ un modello di [regressione lineare](regressione%20lineare.md) definisce la retta che $y=mx+q$ che minimizza l'errore quadratico medio $\sum(x_{i}-y_{i})^2$. Se grafichiamo queste coppie in uno scatter plot, dove gli assi sono le due grandezze, allora vorremmo che tutte le coppie si trovassero sulla bisettrice $y=x$.
La regressione è la funzione lineare che minimizza lo scarto quadratico.
# Interpretazione
La singola $R^2$ ci dice quanto possiamo spiegare una delle due quantità in relazione all'altra, cioè quanto la forma delle due curve è uguale. Un coefficeinte di 1 non ci dice che le due curve sono sovrapposte, ma che hanno la stessa forma, cioè che $y_{i}=mx_{i}+q$, qualunque sia il valore di $x_{i}$: dato $x_{i}$, il valore di $y_{i}$ si trova sempre con la funzione di regressione.
- Slope
	Un valore di $m=1$ ci dice che non ci sono errori di proporzionalità.
	Un valore diverso ci dice che c'è un errore proporzionale tra i due.
- Intercept
	Un valore di $q=0$ ci dice che non ci sono errori di offset.
	Un valore diverso ci dice che c'è un errore costante tra i due.