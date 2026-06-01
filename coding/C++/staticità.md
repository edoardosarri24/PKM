In [C++](C++.md) la staticità rende un membro (attributo e/o metodo) di classe, cioè non legato a una singola istanza ma presente per tutte le istanze.
# Header
Dichiarare una costante come $\texttt{static}$ in un header era la prassi fino al C++17. Quello che succedeva però è che ogni file che importava l'header aveva un copia diversa di quella variabile.
Quello che si fa adesso è invece usare $\texttt{inline constexpr}$: la [costante](costanza.md#Espressione%20costante) viene sostituita al posto del suo nome nel file che la importa.