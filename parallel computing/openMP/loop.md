È una direttiva [pragma](compilazione.md#Pragma) per [openMP](openMP.md).
Permette di applicare la [data decomposition](decomposizione.md#Data%20decomposition) dividendo gli indici di un ciclo in chunk e assegnare ogni chunk a un thread.
##### Necessario
Per permette a omp di eseguire la parallelizzazione di un loop sono necessarie due cose: il numero di iterazioni per ciclo deve essere noto in anticipo; il numero di iterazioni non deve cambiare durante l'esecuzione del ciclo.
Questo è perché OpenMP è un [framework esplicito](parallel%20pragramming.md#Framework) e quindi ci semplifica le cose ma ci limita lo spazio in cui possiamo lavorare.
##### Indipendenza
Siccome quando si esegue in parallelo non abbiamo garanzia sull'ordine di esecuzione, è da usare solo quando le iterazione del loop sono totalmente indipendente e l'ordine della loro esecuzione non può portare in nessun modo a una [race condition](race%20condition.md).
##### Break/continue
Nel loop non ci devono essere $break$ che interrompono il ciclo. In generale in omp vale che non si può uscire dai confini di una direttiva se non per la fine della direttiva stessa.
Nell'esecuzione sequenziale un $break$ porta a una semplice interruzione del ciclo. Nell'esecuzione parallela invece questo impone che i thread si debbano sincronizzare per interrompersit tutti.
È permessa invece l'operazione $continue$ perché è locale al solo thread, cioè è solo lui che va alla prossima iterazione e non deve essere una comunicazione tra thread.
# Direttiva
La direttiva è $\#pragma\ omp\ for$
Può essere unita nella direttiva della [parallelizzazione](parallel%20region.md).
# Scheduling
È possibile dichiarare se gli indici devono essere [assegnati dinamicamente o staticamente](decomposizione.md#Asseganzione) con $\texttt{\#pragma\ omp\ for\ schedule(value, block\_size)}$, dove:
- $value$ può essere:
	- $dynamic$
	Di default $\texttt{block\_size}=1$. Questo introduce un grande overhead, visto che dopo aver processato un solo elemento il thread deve chiedere a openmp di passargli il prossimo indice.
	- $static$
	- $guided$
		Diminuisce via via il pool di iterazioni assegnate. Non ha molto senso.
	- auto
		È il compilatore che decide.
	- runtime
		Usa il valore presente nella variabile d'ambiente openMP $OMP\_SCHEDULE$.
- $block\_size$ è il numero di iterazioni che sono date a un thread.
# Sincronizzazione
La direttiva loop definisce una [barriera](parallel%20computing/openMP/sincronizzazione.md#Barrier) di sincronizzazione implicita alla sua fine: quando il blocco termina tutti i thread devono sincronizzarsi secondo il [relaxed consistency model](memory%20consistency.md#Relaxed%20consistency).
Possiamo eliminare questa barriera implicita aggiungendo $nowait$ nella pragma.
# Nested loop
Di default la pragma per i cicli apporta la suddivisione delle iterazioni solo al ciclo più esterno.
Dobbiamo valutare se il ciclo interno è dipendente da quello esterno.
##### Direttiva
Usando $\#pragma\ omp\ for\ collapse(n)$, dove $n$ è il livello di nested, assegniamo a ogni thread una porzione del ciclo esterno e una di quello interno.
Se non specifichiamo $collapse$ è come se fosse $collapse(1)$.
##### Importante
Oltre a non dipendere da quello esterno deve essere anche un nested loop perfetto: non ci devono essere istruzioni tra i due $for$.