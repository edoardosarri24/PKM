Gli smart pointer in [C++](C++.md) sono delle classi che, insieme ai [RAII](RAII.md), fanno scrivere codice più sicuro.
Il loro scopo è quello di gestire la memoria ed evitare memory leak: non si deve infatti rilascaire la memoria con $\texttt{delete}$. Si consiglia infatti di usare gli smart pointer al posto dei [puntatori](puntatori.md) classici ogni volta che è possbile.
# Tipi
Ci sono tre tipi di classi, dichiarate nell'header $\texttt{memory}$ implementate con i [template](template.md):
- $\texttt{unique\_ptr}$
	Solo uno smart pointer può gestire l'oggetto associato. Se si va fuori dallo scope dello smart pointer allora l'oggetto viene distrutto.
- $\texttt{shared\_ptr}$
	Un qualunque numero di $\texttt{shared\_ptr}$ può gestire l'oggetto. Solo quando l'ultimo $\texttt{shared\_ptr}$ viene distrutto allora l'oggetto viene distrutto.
- $\texttt{weak\_ptr}$
	Verifica se l'oggetto esiste ancora. Non è nello scope e non viene usato epr decidere la distruzione.