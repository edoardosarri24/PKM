Si tratta di uno stile architetturale per costruire reti. È composta da due parti che svolgono ruoli diversi:
- L'encoder genera una rappresentazione contestuale dell'input, detta contesto.
- Il decoder utilizza il contesto durante la sua esecuzione per generare un'output sulla base di quanto appreso dall'encoder.
# Architetture
- Solo encoder
	Si usa quando si vuole estrarre una rappresentazione del testo o di una sequenza.
	In generale non sono addestrati a predire la prossima parola, ma si usa solitamente un task di mascheramento per il loro training. Si usa solitamente la [fully-attention](attention.md#Granularità%20attention).
	- [BERT](BERT.md)
- Solo decoder
	Si usa quando si vuole generare testo o sequenze.
	Sono addestrati solo a generare la prossima parola condizionanda all'input e a quanto già predetto. Si usa solitamente la [casual attention](attention.md#Granularità%20attention).
	- [GPT](GPT.md)
- Encoder-decoder
	Unisce entrambe le architetture in una sola separando le fasi.
# Transformers
![architettura](NLP/LLM/file/architettura.png)
I [transformers](transformers.md) possono essere implementati usando l'architettura encoder-decoder e quindi poi usati per la generazione (visto che c'è un decoder). I layer di questa rete in generale sono:
- Tokenizzazione
	La frase in input viene prima [tokenizzata](tokenizzazione.md).
- Embedding
	I token vengono trasformati in [embedding](embedding.md) statici.
	Siccome il calcolo nell'[attention](attention.md) layer avviene in parallelo, si perde l'informazione sulla posizione delle parole tokenizzate nella frase. Per questo motivo si deve aggiungere questa informazioni in qualche modo, cioè con i [positional embedding](positional%20embedding.md), che poi vengono sommati agli input embedding.
- Trnsofrmers layer
	Si usa un later di [transformers](transformers.md)
- Encoder output
	L'output dell'encoder è un [embedding contestuale](embedding.md#Contestuali) per ogni token in input. Data una parola, l'embedding contestuale si può vedere come una versione arricchita di quello relativo alla parola in input; per parole in input uguali avremmo embedding contestuali diversi (proprio perché il loro contesto sarà diverso).
- Masked multihead attention
	Si mascherano i token successiva a quello in esame. In pratica si tratta di una casual attention.