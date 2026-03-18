È la struttura dati usata per memorizzare i documenti nell'[information retrival](information%20retrival.md).
L'indice invertito è in pratica un dizionario:
- Chiave
	Ogni termine rappresenta una chiave
- Valore
	È la lista dei documenti che contengono la chiave. Può essere incluso, magari in prima posizione, anche la cardinalità della lista, utile poi per calcolare l'[idf](modelli%20IR.md#Vectorial%20space%20model) del modello spaziale.
##### Osservazioni
- La dimensione della lista è la stessa della bag of words, ma il numero di elementi salvati per ogni termine è molto più piccolo.
- Può essere posizionale e non posizionale in base a cosa viene memorizzato nella lista di posting. Nel caso posizionale, oltre al documento che contiene il termine, si memorizza anche il numero di occorrenze di quel termine in quel documento e la posizione di ogni occorrenza; nel caso di utilizzo del modello spaziale allora si memorizza anche il peso $w_{ij}$.
- Le liste di posting possono essere memorizzate come vettore o linked list. Supponendo che l'invertex index sia costruito staticamente (i.e. prima si costruisce e poi si fa inferenza trovando i documenti) allora ha senso utilizzare un vettore.
# Ricerca
Se voglio trovare i documenti che contengono solo i termini della query, allora dobbiamo fare l'intersezione dei documenti contenuti nelle relative liste di posting.
Questa operazione può essere fatta anche in parallelo.
# Costruzione
Dopo aver fatto una parte di [pre-processing](pre-processing.md), si devono indicizzare i documenti, cioè costruire le righe dei posting.
Questo lo posso fare ordinando le coppie $<token,documento>$ prima secondo il token e poi secondo il documento; a questo punto costruire il vettore dei posting è facile.
# Aggiornamento
- La cancellazione avviene a livello logico e non fisico.
- Se dobbiamo aggiungere un documento allora si utilizzano gli indici dinamici: si aggiorna un invertex index locale e poi ogni tanto quello globale; si ricerca prima nell'invertex index locale e poi si va in quello globale.