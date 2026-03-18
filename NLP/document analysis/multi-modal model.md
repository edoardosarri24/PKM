La document AI è una parte della [document analysis](document%20analysis.md) che usa tecnologie di ML e [NLP](NLP.md) che analizza e capisce i documenti.
# Conseguenze
- Efficienza
	Riduce il tempo di pre-processing dei documenti.
- Accuratezza
	Minimizza l'errore umano nell'analisi dei documenti.
- Scalabilità
	Si analizzano molti più documenti.
# Task
- Ricevute
	Automatizzare l'estrazione di dati degli utenti da ricevute.
- Contratti e assicurazioni
	Gestire e analizzare contratti. O ancora analizzare i curriculum in modo automatico.
- Customer
	Automatizzare le risposte ai clienti sulla base della loro documentazione.
- Health care
	Analizzare immagini complesse di esami. Si riducono i cost, si aumenta l'accuratezza.
- Finance
- Legali
	Correlare il caso attuale sulla base della documentazione di casa passati.
- Educazione
# Modelli
Questo modelli sono più piccoli e non generalisti, ma a specifici per documenti. Non hanno bisogno dell'[OCR](OCR.md) ma svolgono il compito in modo E2E.
##### Donut
È un modello encoder-decoder che si basa sui Vision Transformers (ViT) ed era usato per task di classificazione.
Il pre-training avviene su dati generati su coppie (documento; file json strutturato); si fa poi fine tuning su task specifici (e.g., ricevute) per adattarlo a paradigmi specifici.
- Encoder
	- Il documento viene trasformato in patch di dimensione fissata.
	- Ogni patch viene linearizzata e si costruisce l'[embedding](embedding.md) e aggiunto il [positional embedding](positional%20embedding.md).
	- Questi patch embedding vengono passati a più blocchi di [transformers](transformers.md) e alla fine abbiamo una rappresentazione.
- Decoder
	Genera testo in modo autoregressio (come [GPT](GPT.md)) a partire dalle informazioni estratte dal decoder.
	- Si usa un meccanismo di casual atteton con quello già generato e un meccanismo di cross attention con quello estratto dal decoder.
	- Si addestra a prevedere il prossimo token di un output strutturato come un json e che rappresenti le informazioni del documento.
##### LayoutLM
È un modello multi modale basato su [BERT](BERT.md) che cattura allo stesso tempo sia il testo che il layout; l'idea è che un documento il significato semantico di una parola è correlato alla sua disposizione spaziale.
- Tramite [OCR](OCR.md) si estraee il testo e le sue coordinate spaziali.
- Gli embedding sono costruiti dagli input embedding, segment embedding (per capire di che sezione fa parte un token) e positional embedding. Questi ultimi si basano sulle coordinate orizzontali e verticali della bounding box che contiene il token. Stiamo quindi arricchendo le informazioni di un token con il suo layout.
- Il pre-training è fatto come [BERT](BERT.md) usando il masked model languages e usando un dataset che contiene documenti con label su delle regioni.
- Si fa il fine tuning su task particolari come: estrarre coppie chiave-valore; estrarre informazioni come totali o voci dalle ricevute; classificazione di documenti.
##### LayoutLMv2
È uguale a layoutLM ma negli embedding vengono aggiunte, oltre alle informazioni testuali e posizionali anche quelle visive. Queste ultime sono embedding di features estrare con CNN. L'embedding processato dai layer trasnformers è uno solo ed è dato dalla somma di tutti gli embedding.
##### LayoutLMv3
È uguale a layoutLMv2, ma cambiano i task del pre-training.
Si aggiungono masked multi-modal language modeling: il modello deve capire non solo il testo mascherato, ma anche le patch mascherate; in questo modo si riescono ad apprendere rappresentazioni cross-modal.