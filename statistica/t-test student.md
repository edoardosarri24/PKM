Il test di Student (t-test) è un test statistico parametrico (basato su un parametro di una distribuzione, come media o varianza) usato per valutare se la media si discosta significativamente da un valore di riferimento.
# Stima della varianza
Il suo utilizzo principale è quello di stimare la varianza (o la deviazione standard) di dati di cui non abbiamo tutte le statistiche esatte. Un esempio è quello di dati campionari ottenuti da una distribuzione non nota, cioè ottenuti da esperimenti.
La varianza della distribuzione viene stimata tramite lo stimatore della varianza campionario $s^2=\displaystyle\tfrac{\sum_{i=1}^n(x_i-\bar{x})}{n-1}$, dove $n$ è il numero di campioni e $\bar{x}$ è la media campionaria.
# Distribuzione t
La distribuzione $t$ di Student si definisce a partire dalla statistica $t=\tfrac{\bar{x}-\mu}{s/\sqrt{n}}$, dove $\bar{x}$ è la media campionaria, $n$ è il numero di campioni, $s$ è la devizione standard (calcolata come sopra) e $\mu$ è la media ipotizzata.
La distribuzione si ottiene basandoci su questo valore e si tratta di una distribuzione simile alla [distribuzione normale](distribuzione%20gaussiana.md) con delle code più ampie.
# Intervalli di confidenza
Un utilizzo che si può fare di questo test è quello di ottenere gli intervalli di confidenza dei nostri dati: se abbiamo dati sperimentali, e quindi non abbiamo la distribuzione esatta da dove sono stati ottenuti, allora possiamo ottenere una confidenza dell'intervallo in cui il prossimo campione sperimentale finirà.
##### Puntuale
Si tratta in questo caso di una confidenza puntuale e non sull'intera distribuzione (e.g., CDF). Eventualmente si può ripetere per tutti i punti della distribuzione.
##### Funzionamento
L'idea è di ricavare il valore della t di student e poi l'intervallo di confidenza è dato da $CI=\bar{x}\pm(t_{\alpha/2,n-1}\cdot\tfrac{s}{\sqrt{n}})$, dove $\bar{x}$ è la media campionaria e $n$ è il numero di campioni e $1-\alpha$ livello di confidenza.
A questo punto possiamo essere confidenti all'$x$%, con $x$ scelto, che il nostro nuovo campione cade in $[\mu+CI,\mu-CI]$. Solitamente più il livello di confidenza è alto più l'intervallo $CI$ sarà ampio.