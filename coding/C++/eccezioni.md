Le [eccezioni](coding/eccezioni.md) in [C++](C++.md) sono implementate come in tutti i linguaggi.
# Dichiarazione
Un metodo può solo garantire di non lanciare nessuna eccezione con la clausola $\texttt{noexcept}$, altrimenti può sollevare qualunque eccezione.
# Gerarchia
La gerarchia delle eccezioni è la seguente:
- $\texttt{std::exception}$
- $\texttt{std::logic\_error}$
- $\texttt{std::out\_of\_range}$
##### Metodi
Tutte le eccezioni hanno un metodo $\texttt{what}$ che riporta una stringa con l'errore.
# Throw
Il lancio è fatto usando la sintassi $\texttt{throw ExecptionClass(param)}$.