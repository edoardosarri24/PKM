L'idioma Resource Allocation Is Initializzation (RAII) in [C++](C++.md) è una tecnica nata per gestire le risorse (i.e., memoria e file) in modo sicuro anche quando si verificano eventi eccezionali (e.g., [eccezioni](coding/eccezioni.md)) ed evitare così memory leak.
# Funzionamento
L'idea nasce dal fatto che l'unica certezza che abbiamo quando un metodo lancia un'eccezione, è che verrà chiamato il distruttore degli oggetti allocati sullo stack nel corpo della funzione stessa.
Quello che si fa è legare il ciclo di vita di una risorsa a un oggetto gestore inizializzato nel corpo della funzione che può lanciare l'eccezione. In questo modo, se la risorsa è gestita da un oggetto gestore con un distruttore che chiude ed elimina la risorsa, allora nel caso in cui la funzione lanci un'eccezione siamo sicuri che quella risorsa venga rilasciata.
##### Heap
Se l'oggetto gestore viene per errore inizializzato nell'heap, allora l'idioma RAII non viene implementato correttamente. Non verrebbe distruttore l'oggetto e quindi non verrebbe chiamato il costruttore che si occupa di rilasciare la risorsa correlata.
# Garbage collector
Da notare come il RAII non sia associabile a un garbage collector.
Il garbage collector è un meccansimo non deterministico e totalmente fuori il controllo del programmatore; i gestori RAII invece sono totalmente deterministici e implementati dal programmatore.