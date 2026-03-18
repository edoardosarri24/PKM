L'anonimizzazione è un task dell'[NLP](NLP.md) che può essere svolto in vari modi, tra cui usando i [transformers](transformers.md).
Si tratta di passare al sistema un documento e farsi restituire un documento dove le informazioni sensibili sono sostituite con altro.
# Pipeline
Per svolgere il compito di anonimizzazione si segue solitamente questa pipeline:
- Raccogliere i documenti.
- [Tagging](#Tagging)
	Nel tagging si identificano le informazioni sensibili. È la parte in cui si utilizza un qualche modello.
- [Labeling](#Labeling)
	Nel labeling si oscurano le informazioni sensibili. È un insieme di regole con una mappatura 1:1.
# Tagging
Supponendo di non volerlo fare a mano, si deve costruire un dataset su cui poi addestrare un modello:
- Dataset
	La prima fase è quella di creare un dataset etichettato a mano. Si può fare in due modi:
	- First token
		Si usa un tokenizer (e.g., [wordpiece](wordpiece%20tokenizer.md)) per tokenizzare le parole (eventualmente dopo averle normalizzate). Si assegna poi un tag (e.g., luogo, persona, organizzazione) a ogni token.
	- Label with index
		Senza fare la tokenizzazione, ogni parola sensibile viene indicata con la sua posizione di inizio, di fine e il suo tag.
		È più human readble.
- Training
	A partire dal dataset definito al passo precedente, si allena un modello from scratch (e allora il dataset dovrà essere grande), oppure si fa fine tuning di un modello pre-addestrato come [BERT](BERT.md) (e allora basta un dataset piccolo).
# Labeling
Una volta che sappiamo quali informazioni sono sensibili, dobbiamo oscurarle. Per fare questo ci sono due metodi:
- Mascheramento
	Si oscurano tutte le informazioni sensibili nello stesso modo.
	È molto sicuro e veloce da implementare; si perdono le informazioni infromative su chi ha fatto cosa.
- Labeling
	Si associa a ogni informazione sensibile un'etichetta sulla base di un ID e del suo tag.
	Se ad esempio Mario ha ottenuto il tag PER (persona) allora questo verrà oscurato con person_1.