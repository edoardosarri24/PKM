Una distanza è una funzione $d:S\to\mathbb{R}$, dove $S$ è uno spazio di elementi, che gode delle seguenti proprietà:
- $d(x,y)\ge0$
- $d(x,y)=0\Leftrightarrow x=y$
- $d(x,y)=d(y,x)$
- $d(x,y)\le d(x,z)+d(z,y)$
##### Similarità
La similarità è il complementare della distanza: se la distanza aumenta la similarità diminuisce e viceversa.
Un valore di similarità è solitamente è un numero compreso tra 0 e 1.
##### Spazi non euclidei
Una proprietà fondamentale per uno spazio euclideo è che il valore atteso (i.g. la media) degli elementi deve essere contenuto nello spazio (es: $\mathbb{N}$ non lo è perché la media potrebbe essere un decimale); un esempio di spazio non euclideo è lo spazio delle stringhe o lo spazio degli insiemi.
Se lavoriamo con spazi non euclidei ci servono distanze specifiche e medie specifiche, cioè calcolate ad hoc.
# Norma
La norma $L_r$ è definita come $d(x, y)=\sqrt[r]{\sum_{i=1}^d(x_i-y_i)^r}$, dove $d$ la dimensionalità dello spazio.
Le norme più famose sono:
- Distanza euclidea per $r=2$
- Manhattan distance per $r=1$. È chiamata così perché si calcola la differenza in valore assoluto per ogni componente; se siamo in due dimensioni allora si va prima in verticale e poi in orizzontale (come se fossimo in una città fatta di quadrati).
- Norma infinito. In questo caso è pari alla massima differenza tra le varie componenti; infatti andando a infinito conterebbe solo quella.
# Mahalanobis
La distanza di Mahalanobis è la distanza euclidea applicata in quei casi dove i dati sono distribuiti in modo normale, cioè secondo una gaussiana; si tratta quindi di una distanza euclidea normalizzata.
Si calcola come $d(x,y)=\sqrt{\sum_{i=1}^d(\tfrac{x_i-y_i}{\sigma_i})^2}$, dove $\sigma_i$ è la deviazione standard dei punti lungo la direzione $i$-esima.
# Spazi euclidei
##### Coseno
È una distanza usata in uno spazio euclideo, in cui gli elementi possono essere decomposti in funzione delle direzioni.
Gli elementi sono pensati come vettori e la distanza è calcolata come l'angolo che due vettori formano. Questo vuol dire che non si considerano i multipli dei vettori: due vettori che puntano nella stessa dimensione e che hanno moduli diversi sono uguali. L'angolo viene calcolato tenendo conto che $a\cdot b=|a|b|cos\theta$ e allora $cos\theta=\tfrac{a\cdot b}{|a||b|}$; non calcolando direttamente $\theta$ si permette che la distanza sia compresa tra 0 e 1.
La similarità è calcolata come $1-cos\theta$.
# Insiemi
##### Jaccard
È una distanza calcolata tra due insiemi $S_1$ e $S_2$.
Viene calcolata a partire dalla similarità dei due insieme, definita come $sim(S_1, S_2)=\tfrac{|S_1\cap S_2|}{|S_1\cup S_2|}$; questo è un valore compreso tra 0 e 1.
La distanza è definita come $d(S_1,S_2)=1-sim(S_1,S_2)=1-\tfrac{|S_1\cap S_2|}{|S_1\cup S_2|}$; è un valore compreso tra 0 e 1.
# Sequenze
##### Edit
Si calcola tra due sequenze (solitamente stringhe) non per forza della stessa lunghezza. La $ED(x,y)$ è definita come il minimo numero di operazioni elementari necessarie per trasformare $x$ in $y$.
- Per definire le operazioni elementari ci serve una sua implementazioner. Solitamente si usa Levenshtein, le cui operazioni elementari sono l'inserimento, la cancellazione e la sostituzione.
- Solitamente le operazioni hanno lo stesso peso, ma nienete vieta di includere operazioni che hanno pesi diversi.
##### Hamming
Si calcola tra due sequenze (solitamente stringhe) della stessa lunghezza e indica il numero di posizioni in cui ci sono caratteri differenti.
Possiamo calcolare anche la similarità di Hamming come $HamSim(x,y)=\tfrac{\sum_{i=1}^dIdentical(x[i],y[i])}{d}$; questo è un numero compreso tra 0 e 1 e indica la percentuale di caratteri uguali.
- Può essere vista come un caso particolare di [edit distance](Distanza.md#Edit).