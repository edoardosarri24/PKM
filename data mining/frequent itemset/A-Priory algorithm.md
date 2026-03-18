È un algoritmo per trovare i [frequent itemset](Frequent%20itemset.md) che limita l'uso della memoria centrale.
Si basa su due proprietà:
- Antimonoticità
	Se un itemset X è frequente, allora lo deve essere anche ogni suo sottoinsieme.
- Support-based pruning
	Se un itemset non è frequente, allora anche tutti i suoi supersets (i.e. gli insieme che lo contengono) non sono frequenti.
##### Ottimizzazioni
Ci sono due ottimizzazioni possibili:
- [PCY algorithm](PCY%20algorithm.md)
- [A-Priory approssimato](A-Priory%20approssimato.md)
# Memorizzazione basket
Solitamente dati di questo tipo non sono memorizzati usando un DBMS, ma sono semplicemente in un flat file molto lungo. In questo file gli item sono uno dopo l'altro e i basket sono delimitate da un carattere speciale (e.g. -1).
# Algoritmo
L'idea è, per ogni iterazione $k$:
- Si scorrono tutti gli elementi. Per ogni iterazione $k$ il costo dell'algoritmo è dato da quante volte si scorrono tutti i basket; si fa un passaggio per ogni iterazione $k$.
- Si genera $C_k$, l'insieme di tutti i candidati $k$-itemset che potrebbero avere un supporto $>s$.
- Si trova $F_k$, l'insieme di tutti i frequent $k$-itemset.
![algoritmo A-priori](algoritmo%20A-priori.png)
Analizziamo le varie righe dello pseudo codice:
- 2
	Si trova $F_1$, cioè i frequent 1-itemset, cioè l'insieme di tutti gli item frquenti.
	Il valore soglia $N\times minsup$ è passato dall'utente ed è il numero di items moltiplicato per la percentuale di basket in cui si devono trovare; si poteva anche passare direttamente il numero di basket (i.g. il support count).
- 3
	Si esegue il ciclo finchè non si ha $F_k$ che è l'inseme vuoto, cioè quando il prossimo insieme di frequent $k$-itemset è vuoto.
- 5 e 6
	Nel seguito vediamo un modo efficiente per eseguire i passi 5 e 6 per $k=2$. I metodi generali per $k>2$ sono sotto; osserviamo che per $k=2$ i due metodi usati per generare $C_k$ [$F_{k-1}\times F_1$ method](A-Priory%20algorithm.md#$F_{k-1}%20times%20F_1$%20method) e [$F_{k-1}\times F_{k-1}$ method](A-Priory%20algorithm.md#$F_{k-1}%20times%20F_{k-1}$%20method) sono uguali.
	In generale dato un insieme di item di cardinalità $n$ abbiamo $2^n$ possibili itemset; se guardiamo i possibili itemset in base alla loro cardinalità, il numero maggiore è quando un itemset ha cardinalità due; quando la cardinalità sale il numero di possibili itemset diminuisce. Nell'algoritmo A-Priori il passo più difficile è quello di generare i frequent 2-itemset; per questo motivo c'è una suo ottimizzazione che è il [PCY algorithm](PCY%20algorithm.md).
	- Leggere i basket e contare le occorrenze di tutti gli item. Gli elementi che non superano il supporto non sono frequenti. Mi serve una matrice grande al più quando il numero di item; questa sta in memoria, visto che è difficile immaginare scenari dove ci sono così tanti items. In pratica sto generando tutti i frequent 1-itemset.
	- Genero dai basket tutte le possibili coppie e memorizzo in memoria solo quelle composte da due frequent item; questo è perché valgono le due proprietà sopra. Mi serve una memoria che contenga la lista dei frequent item (i.g. la lista del punto precedente, dove ho eliminato gli item non frequenti) e una matrice $n\times n$, dove $n$ è il numero di frequent item.
- 6
	Per $k=2$ questa non serve: l'insieme dei candidati $2$-itemset $C_2$ coincide con quello dei 2-frequent itemset $F_2$.
- 10
	Concettualmente è semplice, ma da implementare on è banale. Diremo poi qualcosa.
# Generazione dei candidati (5)
Basandosi sui frequent $(k-1)$-itemset $F_{k-1}$, si generano i candidati $k$-itemset $C_k$.
La procedura deve essere:
- Completa
	$F_k\subseteq C_k, \forall k$. Vuol dire che tutti i frequent k-itemset devono essere introdotti nell'insieme dei candidati.
- Non ridondante
	Un candidato deve essere generato solo una volta.
##### Brute force
Considero come candidati ognuno dei possibili $k$-itemset. La generazione è facile, ma stiamo delegando tutto il lavoro al pruning che così diventa molto costoso.
La cardinalità di $C_k$ cresce con $k$, in particolare $|C_k|=\tfrac{d!}{k!(d-k)!}$.
##### $F_{k-1}\times F_1$ method
Per generare $C_k$ si estende $F_{k-1}$ con ogni elemento di $F_1$, cioè si aggiunge a tutti i frequent itemset di cardinalità $k-1$ tutti i frequent item.
Il problema è che ci può essere ridondanza, cioè si possono generare dei frequent itemset candidati che sono duplicati. Per evitare la duplicazione si ordinano gli itemset di $F_{k-1}$ e $F_1$; a questo punto estendo l'itemset di $F_{k-1}$ solo con quegli item di $F_1$ che sono maggiori dell'ultimo item dell'itemset di $F_{k-1}$.
##### $F_{k-1}\times F_{k-1}$ method
È la procedura usata da A-Priori.
Una volta ordinati gli itemset di $F_{k-1}$ prendo due itemset che hanno lo stesso prefisso di cardinalità $k-2$ e li estendo con i due elementi diversi, così da generare un itemset di cardinalità $k$.
# Pruning dei candidati (6)
Considerando uncandidato $k$-itemset $X=\{i_1,\cdots,i_k\}$ genero tutti i $X-\{i_j\}, \forall j=\{1,\cdots,k\}$ e controllo se uno di questi è non frequente; in questo caso $X$ non può essere frequente per la proprietà di support-based pruning. Devo controllare solo gli itemset di cardinalità $k-1$ perché le cardinalità prima sono sicuro che siano frequenti perché generati con A-Priori.
Si fa senza considerare il supporto; infatti questo viene aggiornato successivamente (10).
# Conteggio (7/12)
![algoritmo A-priori](algoritmo%20A-priori.png)
Dato un $C_k$ devo ottenere i frequent $k$-itemset $F_k$ eliminando i falsi positivi, cioè quelli finiti in $C_k$ che però non sono effettivamente frequenti. Questo lo faccio contando le occorrenze dei $k$-itemset in $C_k$ ed eliminando quelli che hanno un support count minore del valore soglia stabilito (dall'utente).
##### Problemi
Una prima soluzione per passare da $C_2$ a $F_2$ potrebbe essere quella di usare una matrice $n\times n$ e incrementare la posizione $[i,j]$ quando troviamo la coppia $(item_i, item_j)$.
Ci sono dei problemi nell'utilizzare questa tecnica:
- Se abbiamo triple (o quadruple e così via) e non coppie: allocare matrici in più dimensioni sarebbe troppo costo.
- La sparsità della matrice, che cresce all'aumentare di $k$ (i.e. della cardinalità degli itemset).
Supponiamo di avere i candidati frequent $k$-itemset ordinati.
##### Soluzione
L'idea è di enumerare tutte le possibili combinazioni di $k$-itemset in una transazione e contarle tramite questa enumerazione. Se abbiamo $n$ elementi le possibili combinazioni di cardinalità $k$ sono $\binom{n}{k}$.
Per eseguire la enumerazione ci sono due soluzioni:
- Prefix tree
	L'idea è quella che, data una transaction, si segua un percorso e si arrivi alle foglie, in cui sono memorizzati i candidati frequent itemset e il relativo contatore.
	La costruzione dell'albero avviene con i candidati frequent itemset: si aggregano nelle foglie gli itemset che hanno lo stesso prefisso; si splitta un nodo quando questo raggiunge il numero massimo di nodi (valore stabilito in precedenza).
	Il problema di questa struttura sta nel numero di figli (e quindi nella sparsità dell'albero), soprattutto per i primi livelli: se a un nodo valuto il prefisso in base a $n$ item) e vogliamo trovare tutti i $k$-itemset allora abbiamo $n-k+1$ figli. Per questo motivo si usano gli hash tree.
- Hash tree
	È lo stesso concetto del prefix tree, dove però il ramo da seguire all'$i$-esimo livello viene scelto tramite l'hash applicato all'$i$-esimo item. Il modulo dell'hash definisce il numero di figli dei nodi.
	Una volta che si è generato l'hash tree con i candidati frequent itemset allora si passa ogni transizione dalla radice e si aggiorna il contatore dell'elemento della foglia in cui si arriva.