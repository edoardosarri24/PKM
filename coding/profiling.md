Il profiling è una tecnica di analisi delle prestazioni di un sistema: serve a capire dove il codice spende la maggior parte del tempo e quante volte vengono chiamate le varie funzioni.
# Tecniche
Ci sono due tecniche con i profiling lavorano:
- Sampling
	Il profiler interrompe il programma a intervalli regolari e guarda quale funzione è in esecuzione incrementando un contatore.
	È leggero ma non riesce a catturare quelle funzione molto veloci.
- Strumentazione
	Il compilatore aggiunge del codice extra all'inizio e alla fine di ogni funzione per cronometrarla.
	È precisissimo ma rallenta l'esecuzione del programma.
# Tool
Un tool possibile è [pprof](https://github.com/google/pprof).