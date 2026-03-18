È un algortimo di tokenizzazione di tipo [subword](tokenizzazione.md#Subword%20tokenization).
È molto simile a [wordpiece tokenizer](wordpiece%20tokenizer.md), ma invece che lavorare con una likeliwood lavora con la frequenza bruta.
# Algoritmo
![BPE learner](BPE%20learner.png)
Originalmente era un algoritmo di compressione che lavorava così:
- Il corpus viene spezzato in caratteri e il vocabolario era inizializzato con tali caratteri.
- Si contavano le occorrenza di ogni coppia di caratteri. Dopo la prima iterazione si considerano non più solo coppie ma tutte le combinazioni del vocabolario.
- La coppia di caratteri più frequente viene aggiunta al vocabolario e nel testo si uniscono i caratteri che formano la coppia. Questi caratteri uniti nel corpus non possono essere considerati separati.
- Si continua contando ancora e unendo elementi del vocabolario insieme finché non si raggiunge una certa dimensione $|v|$ fissata.
# Conseguenze
- Si migliora il problema dell'OOD (Out Of Distribution).
- Capace di catturare i [morfemi](unità.md#Morfemi).