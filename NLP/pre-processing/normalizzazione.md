La normalizzazione è un passaggio del [pre-processing](pre-processing.md) quando si tratta di analizzare testo in forma naturale. Si vuole creare delle classi di equivalenza per termini che rappresentano lo stesso concetto in modo da semplificare il processo di analisi.
Il linguaggio naturale può contenere accenti, maiuscole o minuscole, stessi concetti rappresentati in modo diverso (e.g., date) e cose di questo tipo che farebbero esplodere il numero di [token](unità.md). L'idea è di normalizzare nello stesso modo sia i documenti che la query, in modo da eguagliare la ricerca e il ricercato.
L'alternativa alla normalizzazione è l'espansione, cioè ricercare non solo le parole della query ma anche delle loro variazioni (e.g., la prima lettera sia maiuscola che minuscola).
# Conseguenze
- Quando faccio una qualunque semplificazione diminuisco la complessità dell’[inverted index](inverted%20index.md) però posso creare confusione. Questo aumenta il numero di documenti trovati.
- In generale si diminuisce la lunghezza del dizionario, ma si aumenta la lunghezza delle liste di posting; si tratta di trovare un compromesso.
- Nell'[information retrival](information%20retrival.md) si aumenta la recall perché diminuisce il numero di false negativi: si trovano più cose perché più cose sono rappresentate nello stesso modo. Peggiora la precisione perché aumenta il numero di falsi positivi.
# Tecniche
Ci sono varie tecniche per normalizzare un testo.
##### Case folding
Si riducono tutti i caratteri in minuscolo.
È una tecnica che funziona bene perché tanto l'utente scrive tutto in minuscolo.
Funziona peggio nei casi di sentiment analysis perché il maiusocolo potrebbe indicare qualcosa di interessante.
##### Stop words
Le stop word sono parole molto ricorrenti che non aggiungo informazioni, cioè che hanno una bassa entropia; solitamente si eliminano le $x$ stop words più frequenti.
Questo comporta una riduzione molto limitata della lunghezza del dizionario (con precisione di esattamente $x$ valori), ma le [liste di posting](Memorizzazione%20documenti.md#Inverted%20index) diventano molto più piccole.
Quando si eliminano le stopword si aumenta l'efficienza e l'efficacia dell'IR system. Nonostante questo, molti search engine (e.g., Google) le indicizzano; questo non è un problema perché nel [modello spaziale](modelli%20IR.md#Vectorial%20space%20model) il contributo di queste parole al vettore del documento sarà molto piccolo (in particolare sarà piccolo l'$idf$).
##### Lemmatization
Si vogliono creare classi di equivalenza in modo abbastanza drastico, cioè si riducono le parole al loro lemma (i.e., cosa compare sul vocabolario).
Ad esempio tutte le coniugazione del verbo essere ricadono sotto $be$.
Eseguendo questa tecniche si riduce di molto la dimensione dell'[inverted index](inverted%20index.md). La lunghezza delle liste di posting invece non vengono toccate.
##### Stemming
Il concetto è lo stesso della lemmatization, ma la creazione delle classi di equivalenza non è fatta per lemma ma tramite un algoritmo: in base a delle regole ben precise si decide dove troncare una parola.
È utile quando si vuole implementare un modo specifico per una data lingua.
Come per la lematizzazione eseguendo questa tecniche si riduce di molto la dimensione dell'indice invertito. La lunghezza delle liste di posting invece non vengono toccate.