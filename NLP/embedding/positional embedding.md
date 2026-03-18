Gli embedding posizionali sono delle rappresentazioni univoche di un token che deve essere poi sommato agli [input embedding](embedding.md) in modo da rappresnetare negli embedding finali l'informazione temporale delle parole, cioè la loro posizione della frase.
##### Motivazione
Questo ha senso quando l'elaborazione degli input embedding nei layer della rete avviene in parallelo, perché durante questo processo si perde l'informazione temporale. Questo non succede ad esempio nelle [RNN](RNN.md) che elaborano l'informazione in sequenza, ma succede nei [transformers](transformers.md) che nell'[attention](attention.md) layer eseguono i calcoli in parallelo.
# Indici incrementali
Una soluzione naive potrebbe essere quella di associare a ogni parola nella frase l'indice temporale di occorrenza.
##### Problemi
- Si tratta di una notazione assoluta, che non mette in relazione tra loro le parole: non si riesce a capire la differenza di sistanza tra le posizioni 1 e 2 e tra le posizioni 10 e 1000 quando si sommano poi agli input embedding.
- Gli embedding vengono trattati dall'[attention](attention.md) layer tramite il prodotto scalare tra vettori. Le ultime parole nella frase avranno un positional embedding più grande e quindi dei pesi dell'attention maggiori.
# Indici incrementali normalizzati
Una soluzione agli indici incrementali potrebbe essere quella di normalizzarli sottraendo la loro media e dividendo per la varianza. In questo modo ogni peso di una parola ha un valore in $[0,1]$.
##### Problemi
Se la frase cambia lunghezza allora il positional embedding non è più lo stesso per la stessa parola nella stessa posizione. Questo non ci va bene perché vogliamo che i positional embedding siano univoci.
# Funzioni seno e coseno
Quello che solitamente si fa è usare un vettore di reali, della stessa dimensione dell'input embedding (in modo che si possano sommare). I valori nel vettore del positial embedding sono:
- $PE_{(pos,2i)}=sin(pos/10000^{2i/d_{model}})$
- $PE_{(pos,2i+1)}=cos(pos/10000^{2i/d_{model}})$
##### Significato
Questo vuol dire che per le posizioni pari del vettore di positional embedding restituiamo il valore della funzione seno e per quelle dispari il valore della funzione coseno, dove:
- $pos$ è la posizione del token nel testo.
- $i\in[0,\tfrac{d_{model}}{2}-1]$ è l'indice del vettore di positional embedding. Si calcolano funzioni diversi per gli indici pari e per quelli dispari.
- $d_{model}$ è la dimensione degli input embedding.
##### Conseguenze
- La rappresentazione è univoca data posizione; a prescindere dalla parola perché qua si valuta la posizione e non cosa occorre in quella posizione.
- Siccome seno e coseno sono funzioni in $[-1,1]$ allora il valore del positional emebdding non aumenta provocando problemi come facevano gli indici incrementali.
- Si introduce nell'emebdding finale (quello che si ottiene sommando positional e input embedding) un'informazione sulla relazione di parole nella frase. Questo è possibile grazie alla frequenza (i.e., $\tfrac{1}{10000^{2i/d_{model}}}$): per valori di $i$ bassi la la frequenza sarà molto alta e quindi l'output della funzione varia molto velocemente permettendo di capire quale parola viene prima di quale anche per parole molto vicine; per valore di $i$ alti la frequenza è bassa e la funzione cambia poco, permettendo di cogliere relazioni molto distanti.