Si definisce aliasing quando ci sono più puntatori che riferiscono alla stessa area di [memoria](memoria.md) (o allo stesso insieme di dati).
# Parallelizzazione
In situazioni dove è possibile usare dei puntatori, e quindi dove non è sicuro che non possa esserci il fenomeno dell'aliasing, allora il compilatore non forza la parallelizzazione del codice, perché sarebbe unsafe generare codice [vettorializzato](vettorializzazione.md).
##### Esempio
Vediamo due situazioni safe e unsafe quando si parla di aliasing.
- Safe
	Se abbiamo una funziona che al suo interno definisce più puntatori a più array in modo statico (i.e., allocati sullo stack), cioè definendo anche la dimensionalità degli array, allora possiamo essere sicuri che quei tre array, e quindi i relativi puntatori, non siano mai in overlapping.
- Unsafe
	Se abbiamo una funzione che prende come parametri più puntatori, allora non possiamo essere sicuri che quella funzione non sia usata con puntatori in overlapping.
	In questa situazione il compilatori automaticamente non vettoriazzerà la funzione.
##### Soluzioni
- Supponendo di scrivere il codice in modo corretto e ottimizzabile, possiamo indicare al compilatore che è una nostra responsabilità passare a una funzione dei puntatori che non sono in overlapping. In questo modo il compilatore non è responsabile di errori e può forzare la vettorizzazioen del codice. Questo viene fatto tramite la keyword $restrict$.
- Un'altra soluzione è usare dei flag durante la compilazione del codice. Questi flag sono dipendenti dal compilatore quindi dobbiamo verificare quali ci servono al momento giusto.