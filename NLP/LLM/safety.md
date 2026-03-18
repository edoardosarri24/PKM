La safety negli [LLM](LLM.md) è un grande problema che deve essere almeno mitigato. Quando si sviluppa un modello si deve trovare un trade-off tra utilità e nocività: se il modello non risponde mai allora non è utile; se risponde sempre potrebbe rispondere a cose dannose.
# Problemi
- Il modello deve essere allineato con l'idee dell'uomo. E questo si migliora con il [RLHF](reinforcement%20learning%20from%20human%20feedback.md).
- I modelli grandi sembrano includere maggiormente odio e razzismo.
- Il modello potrebbe contenere dati privati delle persone che non devono essere recuperati in nessun modo.
# Attacchi
Ci sono molti attacchi:
- Jailbracking
	Si vuole forzare il modello a non considerare le guardie che controllano la safety.
- Prompt injection
	Si vuole far credere al modello che un prompt malevolo non lo sia.
	Questo può permettere di eseguire codice remoto, recuperare dati e trasmettere malware.
- Adversarial inputs
	Con il prompt si vuole massimizzare la probabilità che la generazioni inizi in modo affermativo.
	Oltre a lavorare normalmente sul prompt, si può fare aggiungendo dei token particolari (i.e., senza senso). Da questo ci si protegge usando la [perplexity](valutazione%20LM.md#Intrinseca), una metrica che misura la qualità di una frase.
- Lingue meno rappresentate
	Le lingue meno rappresentate funzionano meglio per gli attacchi perché ci sono meno guardie.
- Prompt refinement
	Dopo un primo prompt rifiutato il modello ci lascia dei spazi, cioè delle indicazioni, che possiamo usare per sfruttare le vulnerabilità.
# Risoluzione
I problemi di safety si possono risolvere in due modi:
- Priori: si cura il training set per la fase di pre-training.
- Posteriori: si genera la risposta, ma prima di fornirla all'utente si usano dei filtri per valutarne la nocività.