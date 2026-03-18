Algoritmi come [A-Priory](A-Priory%20algorithm.md) o [PCY](PCY%20algorithm.md) hanno un costo proporzionale al numero di passaggi che si fanno sui dati (cioè di accessi al disco). In questo caso vogliamo cercare di ottenere i frequent itemset facendo meno passaggi e quindi avendo un costo ridotto.
Il compromesso sta nell'approssimazione della soluzione, visto che in questo caso si trova un insieme di frequent itemset approssimato.
# Random sampling
Invece che voler leggere tutti i dati a ogni iterazione $k$, si seleziona un insieme di basket che stia in memoria e si usa [A-Priory](A-Priory%20algorithm.md) leggendo da questi dati; in pratica il numero di accesso al disco è costante. A questo punto abbiamo un insieme finale di frequent itemset approssimato.
Si deve ridurre la soglia che definisce quali itemeset sono frequenti leggermente di più rispetto alla percentuale usata durante il sampling: ad esempio se abbiamo preso $\tfrac{1}{10}$ dei basket totali allora la soglia sarà $\tfrac{1}{12}$ di quella di partenza.
# SON
Lavoro su tutti i basket, ma in batch. Si basa sull'antimonoticità: se un itemset è frequente allora lo è anche nei vari batch.
L'algoritmo funziona così:
- Si divide l'insieme delle transaction in batch.
- Porto in memoria un batch alla volta e su questo eseguo [A-Priory](A-Priory%20algorithm.md) per trovare insieme di candidati frequent itemset. Devo in questo passo ridurre la soglia nello stesso modo della versione random sampling.
- Unisco tutti i candidati trovati nei vari batch.
- Conto quante volte compaiono realmente in tutte le transaction per eliminare i falsi positivi.
##### Versione distribuita
L'algoritmo SON si presta bene all'implementazione distribuita, cioè mettendo i vari batch in server diversi. Il problema si risolve usando [map/reduce](Map%20reduce.md) in due passi.
- Primo passo
	- map
		Prende in input un batch. Trovo i frequent itemset presenti nel batch usando un algoritmo come SON o random sampling, diminuendo la soglia come detto sempre.
		L'output sarà una lista di coppie $(F,1)$ dove $F$ è un frequent itemset del batch.
	- reduce
		Prende in input tutte le coppie $(F,1)$, dove $F$ è un frequent itemset di almeno un batch, ed elimina i duplicati.
		Il risultato è un insieme di candidati frequent itemset a livello globale.
- Secondo passo
	- map
		Prende in input l'insieme dei candidati frequent itemset e un batch di transaction. Il mapper in questo caso può essere lo stesso del passo precedente in modo da non spostare i dati.
		Il mapper conta le occorrenze dei candidati frequent itemeset nel proprio batch e produce la coppia $(C,v)$ dove $C$ è il candidato e $v$ è il suo contatore.
	- reduce
		Mette insieme gli stessi $C$ che provengono da tutti i nodi sommando i contatori ed elimina i falsi positivi.