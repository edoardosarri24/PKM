È una libreria della [STL](STL.md) che mette a disposizione un gran numero di elementi.
# Rand
È la funzione base, che ormai è praticamente deprecata e non è consigliata da usare a meno di casi molto semplici.
Soffre del problema del bias, cioè ci sono numeri che escono più frequentemente di altri.
# Distribuzioni
Sono rappresentate tutte le possibili distribuzioni. Invece che usare $\texttt{rand()}$ si possono usare queste che sono molto più precise, senza bias e permettono molte rappresentazioni.
# Engine
Sono algoritmi che generano numeri casuali all'interno di un grande intervallo.
- Mersenne Twister
	È un celebre algortimo che utilizza i numeri primi di Mersenne ed è lo standard de facto perché ha un periodo immenso (i.e., prima di ripetere un numero passa una vita), una distribuzione molto uniforme ed è molto veloce.
- $\texttt{std::default\_random\_engine}$
	Una scelta rapida che il compilatore decide per te.
- $\texttt{std::ranlux48}$ o $\texttt{std::ranlux24}$
	Molto più lento ma con proprietà statistiche estremamente elevate, cioè i numeri sembrano molto casuali.
- $\texttt{std::random\_device}$
	È detto essere il vero generatore casuale: non utilizza tecniche algoritmi ma si affida all'hw e utilizza rumore, temperatura, movimenti del muove e informazioni della tastiera.
	La sua implementazione dipende dal compilatore e non è riproducibile su tutti gli hw.