La convoluzione è l'operatore matematico ($\star$) che si usa per sommare due [random variable](random%20variable.md) indipendenti.
# Calcolo
Per capire mettiamoci nel caso discreto. Se abbiamo due variabili casuali $X$ e $Y$ con la lista dei tempi che possono assumere dove ognuno ha la sua probabilità, allora la probabilità che la variabile casuale $Z=X+Y$ assuma valore $k$ è data da $P(Z=k)=\displaystyle \sum_{i}P(X=i)P(Y=(k-i))$
##### Matematicamente
Se abbiamo variabili continue allora la convoluzione si calcola come $(f_X​\star f_{Y}​)(t)=\int_{-\infty}^\infty f_{X}(\tau)f_{Y}(t-\tau)d\tau$.
##### Concentuaalmente
Si tratta della media di $f$ pesata per i valori di $g$. Ad esempio se $g$ ha una forma a [campana](distribuzione%20gaussiana.md) allora la convoluzione $(g\star g)$ darà più peso ai valori centrali.
# Usi
I possibili scenari di utilizzo sono:
- Composizione sequenziale
	Abbiamo due task che eseguono sequenzialmente e vogliamo ottenere la distribuzione del tempo di esecuzione E2E.
# Indipendenza
Se utiliziamo la classica convoluzione per sommare variabili casuali che non sono indipendenti quello che otteniamo è un'approssimazione ottimistica del risultato.
##### Soluzioni
In questi casi si può usare la variante biased convolution (convoluzione polarizzata) o gestire le correlazioni con Copule.