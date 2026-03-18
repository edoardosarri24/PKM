In [C++](C++.md) la costanza rende un elemento non modificabile.
A differenza del [C](C.md), dove modificare un elemento costante è un [undefined behaviours](undefined%20behaviours.md), in C++ si ottiene un errore di compilazione.
# Costanti globali
In C++ è bene non utilizzare i $\texttt{define}$, visto il loro totale non controllo sui tipi. Al loro posto possiamo usare costanti globali tipizzate, su cui il compilatore esegue un controllo del tipo.
# Puntatori
In C++ possiamo rendere costanti i puntatori oppure anche i loro elementi:
- $\texttt{char* const p}$ vuole dire che l'indirizzo memorizzato nel puntatore non varierà, ma i dati possono farlo.
- $\texttt{const char* p}$ vuol dire che il puntatore non varia ma i dati possono farlo.
- $\texttt{const char* const p}$ vuol dire che sia puntatore che dato sono costanti.
# Metodi
Dichiarando un metodo costante stiamo chiedendo al compilatore di controllare che il corpo del metodo non modifichi i dati della classe.
Questi metodi sono anche gli unici che possono essere chiamati sui campi costanti.
##### Sintassi
La sintassi è $\texttt{tipo\_ritorno nome(param) const}$.
# Espressione costante
In C++ c'è la keyword $\texttt{constexpr}$ che può essere applicata a variabili (globali e locali) e funzioni.
Il suo scopo è indicare al compilatore di inferire il suo valore durante la compilazione e non a run-time, aumentando di fatto le prestazioni a run-time.
Quando ci sono delle variabili globali, ad esempio quelle usate per le configurazioni, allora è il modo giusto di dichiararle. Quello che succede in questi casi è che il compilatore le forzerà a essere $inline$, come se fossero delle [define](C.md#Costanti) del C, cioè le sostituisce effettivamente nel codice che le usa.