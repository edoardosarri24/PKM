Una legge di potenza descrive la relazione tra due variabili $x$ e $y$ dove una varia come potenza dell'altra: $$y=a\cdot x^k,$$
- $a$ è la costante di proporzionali (o fattore di scala).
- $k$ è l'esponente di scala.
# Invariante di scala
Nelle funzioni basate su polinomi o esponenziali, se moltiplichiamo per una costante $c$ l'input $x$, otteniamo un incremento polinomiale o esponenziale dell'output: se $y(x)=e^x$, allora $y(c\cdot x)=e^{cx}$.
Le leggi di potenza invece sono invarianti di scala. Questo vuol che moltiplicando l'input $x$ di una qualunque quantità $c$, l'output varierà sempre di un fattore costante $c^k$, senza dipendere dal punto di partenza dell'input: $y(c\cdot x)=c^k\cdot y(x)$.
# Scala log-log
È sistema di assi cartesiano dove entrambi gli assi riportano i relativi valore in forma logaritmica.
È usata per rappresentare le leggi di potenza: $\log(y)=\log(a\cdot x^k)=\log(a)+\log(x^k)=k\cdot \log(x)+\log(a)$. Questo ci dice che nel piano log-log:
- La pendenza della retta, cioè il tasso di crescita o decrescita, è $k$.
- L'intercetta diventa $\log(a)$.
##### Vantaggi
- Si gestiscono bene enormi ordini di grandezza: l'amento di un fattore 2 o fattore 10 è rappresentato dallo stesso incremento visuale.
- Si riesce subito a capire il fenomeno: le leggi di potenza diventano linee rette e quindi si identificano subito dei pattern.
##### Svantaggi
Si perdono informazioni sulla curva dell'errore. Ad esempio un cambio di concavità su log-log non coincide necessariamente con un punto di flesso su lin-lin.