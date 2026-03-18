Prima di essere passate in input a un qualche [language model](language%20model.md), le parole (in realtà i token) devono essere, dopo la fase di [pre-processing](pre-processing.md), rappresentate in un qualche modo:
- Bag of words
	Prima degli embedding per rappresentare le parole in un qualche spazio veniva usato il bag of word e il [vectorial space model](modelli%20IR.md#Vectorial%20space%20model).
	Il problema è che le parole diventano vettori della dimensione del numero di documenti, cioè hanno un qualche peso in ogni documento, e non contengono nessuna informazione semantica o relazione tra loro.
- word-word matrix
	Oltre alla matrice di bag of words, posso costruire la matrice quadrata $n\times n$, con $n$ cardinalità del vocabolario, word-word: definisco una finestra di dimensione $k$; scorro il testo e considero la parola target e le sue $k$ adiacenti (sia a destra che sinistra); sulla riga della parola target incremento la cella delle parole di contesto.
	Ogni parola definisce un vettore nello spazio delle parole; due parole hanno la stessa semantica se i relativi vettori sono simili (secondo la distanza del coseno).
	Questa matrice nella realtà non viene usata, ma ci fa capire che le parole possono essere rappresentate in molti modi diversi, anche e soprattutto sulla base del loro contesto.
- Embedding
	Nella rappresentazione tramite embedding si vuole mappare i token in uno spazio vettoriale piccolo e denso, le cui dimensioni non hanno un qualche significato noto e stabilito, ma per cui due parole vicine sono [semanticamente](embedding.md#Semantica) simili.
	Avere una bassa dimensione ci facilita il costo dei calcoli. La similarità tra parole è in funzione della distanza tra vettori (i.e., distanza del [coseno](Distanza.md#Coseno) o prodotto scalare tra vettori).
# Semantica
Se vogliamo rappresentare la semantica di una parola, ci sono diverse cose che possiamo considerare:
- Lemma
	Il lemma di una parola è la definizione che troviamo sul dizionario.
	Per una stessa parola ci sono più definizioni; il significato quindi è anche in relazione al contesto in cui la usiamo.
- Sinonimi
	Se metto in relazione le parole sulla base del loro significato allora ottengono i sinonimi.
- Simili
	Sono parole che condividono una qualche informazione: se scambiate nella frase ottengono un significato diverso, ma la frase continua a funzionare.
- Relazione
	Due parole sono in relazione quando non sono interscambiabili ma sono solitamente collocate nella stessa frase.
# Tipologie
Gli algoritmi producono due tipi di embedding: statici e dinamici.
##### Statici
Il vettore di embedding per una parola non cambia a seconda della sua semantica e del suo contesto. Questo è un problema quando vogliamo avere delle informazioni sul contesto.
Ad esempio banca in “sono andato in banca” e in “mi siedo sulla banca del fiume”, l'embedding di banca è lo stesso vettore.
- [word2vec](word2vec.md)
##### Contestuali o dinamici
Per una stessa parola in due posizioni diverse della frase, quindi dove ogni occorrenza della parola ha un contesto e una semantica diversi, vengono prodotti due embedding diversi.