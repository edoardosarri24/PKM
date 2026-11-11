La Standard Template Library (STL) è un insieme di tre elementi implementata tramite [template](template.md) che permette di avere nativamente nel [C++](C++.md) un insieme di strutture dati e algoritmi molto ampio.
# Costituzione
Le istanze degli oggetti della STD solitamente sono molto leggeri, che stanno nello stack e contengono solo metadati. Il volo valore, cioè i dati, sono contenuti nello heap.
##### Metadati
- Puntatore all'inizio
	Punta all'indirizzo del primo byte dei dati nello heap.
- Size
	Indica dove i dati finiscono.
- Capacità
	È un puntatore alla fine dei metadati nella memoria stack.
# Metodi
Ci sono dei metodi che possono essere chiamati su tutti gli oggetti della STD:
- $\texttt{.data()}$
	Restituisce il puntatore al primo byte dei dati nello stack.
- $\texttt{empty()}$
	Booleno che confronta l'indica che $\texttt{size==0}$. Sotto sotto confronta il puntatore di inizio e fine.
- $\texttt{capacity()}$
	Spazio totale allocato nello heap. Indica quanti elementi puoi inserire prima di riallocare memoria.
- $\texttt{size()}$
	Numero di elmenti logici presenti.
# Algoritmi
Mettono a disposizione funzionalità sui contenitori usando gli iteratori.
##### Esempi
- Clamp.
# Classi
- [chrono](chrono.md)
- [random](random.md)
- [string](string.md)