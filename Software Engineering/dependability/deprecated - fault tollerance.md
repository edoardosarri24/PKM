

# APPROCCI
Gli approcci che si possono usare per implementare la SFT possono essere classificati in:
## [[deprecated - Strutturati]]
## Ringiovanimento
Il SW dopo un po' che è in esecuzione può invecchiare (troppo uso della memoria perche non si libera un puntatore, coda dei processi della CPU troppo lunga e quindi si prendono informazioni ormai vecchie).
Il ringiovinamento può essere fatto a livello di sistama o di singola applicazione.
Può essere (anche combinandoli insieme):
- Time based
	Usato per sistemi che hanno dei momenti di inutilizzo. In questi momenti si può riavviare senza problemi.
- Prediction based
	Usato quando il sistema non può essere riavviato spesso senza problemi o perdite economiche. Si deve fare il ringiovinamento in base alla previsione fatta su quando il sistema andrà in crash.
## Safety net techniques
Sono tecniche general-purpose, non pensate a livello archietturale. Vengono utilizzate al momento del bisogno all'interno del codice per errori che si presentano in tutte le situazioni (Apertura file non esistente, null poiter). Più che per limitare i guasti sono usate per rilevare e gestire gli errori ($Try-catch$ di Java).
Sono usate indipendentemente dall'impiego da altri approcci.
- Protezione della memoria.
- Controllo del tipo dei parametri.
- Watchdog timer (Dopo la scadenza del tempo si manda un segnale o eccezione che deve essere gestita).