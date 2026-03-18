Una classe in [C++](C++.md) estende il concetto delle [strutture](strutture.md) in C.
# Sintassi
Si dichiara con $\texttt{class nome\_classe}$.
##### Riferirsi
Per riferirsi a un membro si usano gli stessi concetti di una struttura in C:
- Variabile
	Se abbiamo un oggetto in una variabile allora si usa $\texttt{var.nome\_membro}$.
- Puntatore
	Se abbiamo un puntatore all'oggetto allora si usa $\texttt{ptr->nome\_membro}$.
# Visibilità
I campi di una struct in C sono utilizzabili da tutti coloro che hanno un riferimento alla struttura stessa, i.e. che include l'header dove è dichiarata.
In C++ ogni membro può avere tre livelli di visibilità, che è sempre bene specificare.
- Public
	È visibile a tutti. È quello che succede in [C](C.md).
- Protected
	È visibile nella classe e nelle [sotto classi](ereditarietà.md).
- Private
	È visibile sono all'interno della classe. È il modificatore di default.
# Astratte
Una classe astratta è una classe di cui non è possibile istanziare direttamente oggetti e che è pensata per essere poi [ereditata](ereditarietà.md), cioè serve per definire la specifica di un elemento sofrtware.
Per definire una classe come astratta si deve dichiarare almeno un [metodo virtuale](ereditarietà.md#Virtuali)  seguito da $\texttt{= 0}$; questo è detto metodo puramente virtuale. Se poi la classe ha solo metodi puramente virtuali allora si dice puramente astratta.