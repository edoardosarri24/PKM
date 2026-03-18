Il testo analizzato non viene trattato come linguaggio naturale, ma deve subire una fase di [pre-processing](pre-processing.md) prima di essere elaborato.
Questa e le successive fasi utilizzano delle unità di base su cui si costruiscono i modelli di [NLP](NLP.md).
# Definizioni
- [Caratteri](#Caratteri)
	È la vera unità di base che forma le parole.
- Parola
	È una sequenza di caratteri delimitata che compare nel testo.
	Le parole si dividono in due categorie:
	- Closed (funtion word)
		Sono parole corte, frequenti e con una funzione grammaticale.
		Sono esempi gli articoli, i pronomi o le preposizioni.
	- Open (content word)
		Sono parole più significative e meno comuni.
		Sono esempi i nomi, i verbi e gli aggettivi.
- Termine
	È una parola [normalizzata](normalizzazione.md) in qualche modo.
- Token
	È un'istanza di una parola che ha subito una [tokenizzazione](tokenizzazione.md).
# Morfemi
Un morfema è una singola unità con significato che, eventualmente unita con altri morfemi, forma una parola.
Un morfema si divide in prefisso e suffisso: solitamente il primo definisce la parola, mentre il suffisso la specializza. Un esempio è che "cats" è formato da due morfemi: "cat" e "s"; il primo indica l'oggetto, mentre il secondo specializza il plurale.
Il problema dei morfemi è che ci sono lingue che hanno pochi morfemi/parole (e.g. 1.1) e altre lingue che hanno molti morfemi/parola (e.g., 3.5)
# Caratteri
I caratteri sono le unità che costituiscono le parole. Rappresentare i caratteri non è banale.
##### ASCII
Il primo tentativo è stato fatto usando ASCII in America: un byte per ogni carattere; si usano 7 bit e quindi abbiamo $2^7$ possibili caratteri. Questo andava bene perché l'ASCII comprendeva solo i caratteri americani; quando però l'informatica si è espansa anche ad altri contenenti che usavano caratteri diversi si è avuto un problema.
##### Unicode
È una codifica che permette di avere più di $2^8$ caratteri. Ogni carattere ha un ID unico (circa 150000 id possibili) rappresentato in esadecimale con il prefisso $U+$.
Quando si memorizza in memoria un carattere non si memorizza tutto l'ID, ma si utilizza una codifica UFT (8, 16 o 32) che trasforma l'ID in byte. La codifica più usata è UTF-8 perché utilizza da 1 a 4 byte per carattere ed è retro compatibile con l'ASCII.
- Se il primo byte è minore di 127 allora si codifica come se fosse ASCII e diventano importanti solo i primi 7 bit. In questo modo si ottiene la back compatibility.
- Se il primo byte è maggiore o uguale a 128 (i.e, 0x7F hex) allora si mappa l'ID in 2, 3 o 4 byte a seconda del carattere.
# N-gram
Un altro modo per rappresentare le unità di elaborazione è usare i [k-gram](Locality%20sensitive%20hashing.md#Shingling).