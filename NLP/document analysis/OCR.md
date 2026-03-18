È la tecnica con cui a partire da un'immagine si riconosce il testo e lo si trascrive in un formato leggibile dal calcolatore. Oggi si usano tecniche E2E come gli [LLM](LLM.md).
Solitamente funzionano in modo semantico e questo porta a prestazioni non ottimali quando ci sono dei numeri o parole senza contesto.
# Pipeline
I flow è il seguente:
- Pre-processing
	Si aumenta la qualità dell'immagine, eventualmente facendo un'analisi [fisica](layout%20analysis.md#Analisi%20fisica) o [logica](layout%20analysis.md#Analisi%20logica). Sono esempi la correzione dell'inclinazione, la trasformazioni in bianco e nero.
- Riconoscimento
	Si deve riconoscere il carattere, solitamente usando un dizionario di caratteri corretti. Si tratta quindi di un feature extractor seguito da un riconoscitore.
	Le difficoltà possono venire da due caratteri che si toccano perché è un errore o perché devo effettivamente toccarsi (come in alcune lingue).
	Un'altra difficoltà sono i simboli matematici che, se fatti ad esempio in latex, non corrispondono a caratteri unicode, ma a zone di pixel neri.