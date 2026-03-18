Il pre-processing è una parte fondamentale di quando si lavora con il testo.
# Passi
Prima dell'elaborazione, considerando le [unità](unità.md) che compongono il testo, si deve:
- Si collezionano i documenti.
	Questo passo non sempre è facile da definire. I documenti infatti possono avere vari formati, varie lingue e ci possono essere numeri (non indicizzati fino a poco tempo per la loro complessità).
- Si [normalizzano](normalizzazione.md) i termini in modo che parole simili siano rappresentate dallo stesso token. Si deve normalizzare nello stesso modo sia i documenti che la query.
- Si [tokenizzano](tokenizzazione.md) i termini normalizzati presenti nei documenti.
# Herdan (o Heaps) rules
Quando si analizza un documento il numero di [parole](unità.md#Definizioni) nuove che si incontrano è proporzionale a quante ne abbiamo già incontrate.
Questa crescita segue la legge di Herdan (o Heaps rule): $|V|=kN^\beta$, dove $k$ e $\beta$ sono parametri empirici:
- $|V|$ è il numero di parole uniche trovate.
- $k$ empiricamente è tra 10 e 100.
- $\beta$ è all'inizio è 1. Col passare delle parole tende a $(0.4,0.6)$.