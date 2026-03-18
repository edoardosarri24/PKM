# Set
Un insieme non ha un ordine e quindi non è rilevante mantenerlo nel database.
Se non ci serve mantenere un ordine è sicuramente la struttura dati più efficiente.

Se usiamo il set allora non abbiamo l’ordine, ma se usiamo la lista dobbiamo controllare l’ordine.
questo si può mantenere in due modi: o direttamente dentro nel databse oppure rimetterle in ordine dopo averle prese da database.

In un caso abbiamo problemi quando inseriamo/eliminiamo un elemento dal database, nell’altro quando li prendiamo dal databse.
# List
Se ci serve stabilire un ordine questo può essere fatto in due modi:
- Quando si scrive sul database
	In questo caso la scrittura sul database sarà più onerosa, ma sarà più efficiente il caricamento in memoria.
	In questo caso si usa $@OrderColumn$ e indica secondo quale colonna del database ordinare la tabella. 
- Quando si caricano gli oggetti
	In questo caso si avrà un costo proporzionale all'ordinamento in memoria di oggetti.
	In questo caso possiamo stabilire l'ordine in memoria tramite $@orderBy$, specificando la colonna secondo cui ordinare; può prendere dei parametri tipo la direzione di ordinamento.