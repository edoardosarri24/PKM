Bidirectional Encoder Representations from Transformer (BERT) è un [LLM](LLM.md) [encoder-only](encoder-decoder.md) (non è un modello generativo) basato sull'architettura dei [transformers](transformers.md).
# Task
I task finali, cioè quelli eseguito dopo il fine tuning, meglio svolti sono quelli di comprensione e classificazione del testo, come ad esempio la sentiment analysis e l'[anonymization](anonymization.md).
Vedi [questo notebook](https://github.com/edoardosarri24/obsidian/blob/master/PKM/UniFi/NLP/transformers/MultiClass_Sentiment_Classification_with_BERT.ipynb) per esempio.
# Training
BERT si basa sul concetto di [Self supervised learning](Self%20supervised%20learning.md): il training si basa su due task: Masked Languages Modeling e Extractive Question Answering. L'idea è di apprendere in questo modo il contesto generale della lingua; alla sua architettura pre-trained si attaccano poi dei layer finali su cui fare fine tuning consentendo di eseguire molti downstream task.
Il corpus su cui è addestrato è di circa 3B di parole (rispetto agli ultimi modelli molto poco).
##### Masked Languages Modeling
Con il task Masked Languages Modeling (MLM) si prende in input una frase e si maschera il 15% dei token. L'obiettivo del modello è capire quale token va al posto di quello mascherato.
Il maskeramento di quel 15% è così distribuito:
- 80% sono effettivamente maskerati.
	Maskerare troppi token vorrebbe dire specializzare il modello su questo task, mentre l'obiettio del training di bert è quello di fargli apprendere la semantica dei token all'interno del testo.
- 10% sono rimpiazzati con un token casuale.
	Permette di aumentare la robustezza del modello a errori (e.g., typo).
- 10% sono rimasti invariati.
##### Next Sentence Prediction
Usare quest task durante l'addestramento di BERT permette di usarlo in più task downstream.
L'obiettivo è, date due frasi, capire se la seconda è conseguenza della prima.
# Tokenizzazione
L'algoritmo usato per la tokenizzazione è [wordpiece tokenizer](wordpiece%20tokenizer.md).
##### Speciali
Durante la tokenizzazione si aggiungono dei token speciali.
- CLS
	È il primo token della sequenza. All'output dei layer di attention questo token contiene un aggregato delle informazioni apprese dalla rete.
	Possiamo basarci su questo token per svolgere compiti di classificazione come la sentiment analysis.
- SEP
	Rappresenta la separazione tra due frasi.
# Embedding
L'embedding passato al primo layer di [attention](attention.md) è dato dalla somma di tre embedding:
- [Input embedding](embedding.md)
- Segment embedding
	Sono introdotti per l'addestramento sul task di [next sentence prediction](#Next%20Sentence%20Prediction): è un valore binario che rappresenta se il token appartiene alla prima o alla seconda frase.
- [Positional embedding](positional%20embedding.md)
	Sono appresi usando le funzioni seno e coseno.
# Modelli
Bert possiede circa 110M di parametri: la dimensionalità degli embedding (e quindi degli hidden layer) è di 768; il numero di teste della [multihead attention](attention.md#Multihead%20attention) è di 12; il numero di layer di attention è 12.
Oltre a bert, ci sono anche latre soluzioni:
- Distilbert
	Un modello più leggero.
- XLM-roberta
	Più grande con 550M di parametri.