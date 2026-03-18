È una direttiva [pragma](compilazione.md#Pragma) per [openMP](openMP.md).
È l'istruzione che implementa il pattern [Fork-Join](parallel%20computing/pattern.md#Fork-Join%20parallelism): all'inizio c'è il fork e alla fine c'è il join; il fork e il join implicano dell'overhead e quindi se è possibile usarne il meno possibile: usare [single](parallel%20computing/openMP/sincronizzazione.md#Single) o [master](parallel%20computing/openMP/sincronizzazione.md#Master) al posto di far fare un join e poi di nuovo un fork; usare una sola direttiva $parralel$ e poi le altre in modo separato.
Fa partire più thread che eseguono il codice all'interno del blocco. Il numero di thread creati è deciso da omp e solitamente è pari al numero di core.
# Direttiva
La direttiva è $\#pragma\ omp\ parallel$.
Possiamo unire una delle keyword (i.e., [reduction](reduction.md), [loop](loop.md), [sections](sections.md)) delle altre direttive per scrivere in modo più compatto un'operazione parallela.
##### Specializzazione
Ci sono dei modi per specializzare la direttiva:
- $num\_threads(integer\_expression)$
	Di default omp alloca tanti thread quanti sono i core più quelli dell'[hyper threading](hyper%20threading.md) se disponibile. Con questa keyword abbiamo controllo su quanti thread vengono allocati.
- $if(expression)$
	Permette di decidere se eseguire la parallelizzazione oppure no in base al valore dell'espressione (0 è false).
# Variabili
In openMP ci sono delle pragma che sono fondamentali per gestire le [variabili](parallel%20computing/openMP/variabili.md) condivise.
# Nester
OpenMP di default non permette ai fork thread di creare altri fork. Se però vogliamo comunque fare più fork annidati allora dobbiamo definire $void\ omp\_set\_nested(int\ nested)$ con $nested\ne0$.
Possiamo eventualmente controllare se è abilitato il nested con $omp\_get\_nested()$.
##### Prestazioni
Il motivo per cui non è permesso di deafult avere fork annidati è non avere un degrado sulle prestazioni.
Se infatti osserviamo lo speedup che otteniamo dalla parallelizzazione di un algoritmo, possiamo notare come questo incrementi all'ultimentare dei thread fino a un valore soglia; dopo questo valore si ha invece un degrado sulle prestazioni.