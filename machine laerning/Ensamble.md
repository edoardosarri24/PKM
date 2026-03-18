Quando si parla di ensamble stiamo componendo il risultato di più modelli (solitamente stesso tipo di modello) contemporaneamente tramite una media o qualche operazione di combinazione degli output prodotti dai vari modelli.
Un ensamble è un'ottima base line per valutare le prestazioni di un modello. Ci sono delle situazioni in cui una [rete neurale](Deep%20learning.md) potrebbe fare meglio (es: dati non lineari, moltissimi dati), ma in molte situazioni è difficile fare meglio.
##### Vantaggi
- Si riduce la varianza.
- L'errore atteso è $\tfrac{1}{M}$ dell'errore medio dei sotto modelli, con $M$ numero di modelli usati.
##### Interpretabilità
Un ensamble perde di [interpretabilità](Interpretability.md) rispetto al singolo modello. Ci sono comunque tecniche permettono di capire quali sono le features più rilevanti per tutti i modelli che compongono l'ensamble.
# Addestramento
##### Bagging
È un apprendimento di tipo parallelo, dove ogni modello viene addestrato in modo indipendente su un sotto insieme del dataset (con overlapping).
Il modello risultante è una combinazione pesati del risultato ottenuto da tutti i sotto modelli.
- Nel caso in cui i sotto modelli siano [Decision tree](Decision%20tree.md) si parla di random forest. I sotto insiemi di dati sono presi considerando le features: ogni sotto insieme sarà composto da tutti gli elementi ma non da tutte le features.
##### Gradient boosting
È un apprendimento di tipo sequenziale, dove ogni modello che compone l'ensamble lavora su tutto il dataset.
Il primo modello imparare qualcosa dai dati; il secondo modello si basa su quanto appreso da primo e migliora l'apprendimento. Alla fine del processo il modello risultate farà una combinazione pesata dei risultati dei sotto modelli.