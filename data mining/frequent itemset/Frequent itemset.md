Si basa sul market-basket model: abbiamo un grande numero di items (e.g. i prodotti in un supermercato) e un grande numero di basket che contengono un sottoinsieme di items (e.g. i carrelli che contengono un certo numero di prodotti); i basket sono anche detti transaction (e.g. un carrello corrisponde a uno scontrino).
A partire da questo modello vogliamo trovare dei trend nascosti, cioè non visibili a occhio nudo.
##### Definizioni
- $I=\{i_1,\cdots,i_d\}$ è l'insieme di items.
- $T=\{t_1,\cdots,t_N\}$ è l'insieme delle transaction, dove $t_i\subseteq I$.
- Un itemset $X$ è un sottoinsieme di una o più transaction (e.g. un insieme degli elementi del carrello). Se ha $k$ elementi è detto $k$-itemset. Una transaction $t_i$ contiene un itemset $X$ se $X\subseteq t_i$.
- Il support count di un itemset è $\sigma(X)=|t_i:X\subseteq t_i,t_i\in T|$, cioè è il numero di transaction in cui compare $X$ (e.g. il numero di carrelli che contengono il sottoinsieme di prodotti).
- Il supporto è $s(X)=\tfrac{\sigma(X)}{N}$, cioè è la percentuale di tutti gli $N$ basket che contengono l'itemset. Si dice che $X$ è frequente se $s(X)>minsup$, cioè di una soglia; serve per non valutare quegli itemset che sono poco frequenti ([esempio](frequent%20itemset%20esempio.pdf)).
##### Applicazioni
- Supermercato
	Gli item sono i prodotti, i basket sono l'insieme dei prodotti acquisitati contemporaneamente.
	Un esempio di utilizzo è capire quali prodotti sono acquistati insieme per promuovere sconti.
- Pagine web simili
	Gli item le parole, i basket sono le pagine web.
	Un esempio di utilizzo è capire quali pagine web hanno parole rare simili. Per rare si intende non stop-word (e.g. articoli, congiunzioni).
- Plagio
	Gli item i documenti, i basket le frasi. È l'esempio per dire che non sempre gli item sono contenuti nel basket, ma questo dipende cosa stiamo cercando; in questo caso stiamo cercando le pagine.
# Regole di associazione
Una regola di associazione relativa a un basket è un'espressione della forma $X\to Y$ ($X$ implica $Y$), con $X\cap Y=\emptyset$, e vuol dire che quando $X$ è contenuta nel basket, allora lo è anche $Y$ ([esempio](esempio%20regola%20di%20associazione.pdf)).
- Il supporto di una regola è definito come $s(X\to Y)=\tfrac{\sigma\{X\cup Y\}}{N}$ e rappresenta la percentuale con cui la regola è applicata in tutti gli $N$ basket.
- La confidenza di una regola è definita come $c(X\to Y)=\tfrac{\sigma\{X\cup Y\}}{\sigma\{X\}}$ e rappresenta la percentuale di basket che contengono $Y$ quando contengono $X$.
- L'interesse di una regola è definita come $interest(X\to Y)=|\tfrac{\sigma\{X\cup Y\}}{\sigma\{X\}}-\tfrac{\sigma\{Y\}}{N}|$. Se questo valore è basso allora ci interessa poco questa regola.
##### Scoperta delle regole
Il problema è: dato un insieme di transazioni $T$ trovare le regole di associazione con $support>minsup$ e $confidence>minconf$.
Questo problema è composto da due sotto problemi: trovare i frequent itemset; trovare le regole che soddisfano le soglie. Il motivo di questa suddivisione è il costo che avrebbe generare tutte le possibili regole e poi valutarle; invece si può trovare prima i frequent itemset perché se uno tra $X$ e $Y$ non è frequente allora la regola $X\to Y$ non può essere frequente.
Noi vediamo solo la scoperta dei frequent items. Un algoritmo efficiente, non come le soluzioni sotto, è [A-Priory algorithm](A-Priory%20algorithm.md).
# Frequent 2-itemset
Il problema di trovare i frequent 2-itemset è: dato un inseme di basket, trovare quelle coppie di item che sono comprese in almeno $s$ basket, cioè che hanno un support count pari ad $s$.
Questa è una possibile soluzione, ma quanto specificato nell'algoritmo [A-Priory](A-Priory%20algorithm.md) è più efficiente.
##### Query SQL
La prima soluzione potrebbe essere quella di fare una query sul database; il problema di [questa query](sql%20frequent%202-itemset.pdf) (non da sapere) è che ha un costo troppo alto e ci serve un algoritmo più efficiente e che scali nel numero degli item.
##### Sorting
Si fanno due passaggi:
- Per ogni basket $A$ si generano tutte le possibili coppie, cioè si fa il prodotto cartesiano $A\times A$. Il costo di questo è $\mathcal{O}{(n^2)}$, con $n=|A|$; il risultato sarà una grande lista di coppie.
- Si ordinano le coppie; si contano le coppie uguali; si rimuovono quelle il cui support count è minore di $s$.Il costo di questo è il costo dell'ordinamento, cioè $\mathcal{O}{(nlog\ n)}$, dove $n$ in questo caso è il numero di coppie totali generate, e quindi quadratico rispetto alla dimensione del basket; è troppo costoso.
##### Counting
Teniamo in memoria una matrice $n\times n$, dove $n$ è il numero di item, che rappresenta un counter. Generiamo tutte le coppie di item $A\times A$ per ogni basket $A$, e per ogni coppia aggiorniamo il contatore della matrice. A questo punto prendiamo solo le coppie il cui contatore è maggiore dell supporto scelto.
Il problema è che questa tabella non sta in memoria se $n$ è molto grande; anche se conto solo fino al supporto (se il supporto è 5, che la coppia compaia 5 volte o 10 è uguale), la cosa non cambia, uso troppa memoria. Non posso usare la memoria virtuale e fare swap perché non ho alcuna continuità spaziotemporale.
##### Hash
L'idea è sempre quella di contare le coppie frequenti come al passo prima; con questa tecnica però il conteggio avviene solo tra quelle coppie il cui supporto è maggiore della soglia $s$; in questo modo la tabella counter è più piccola ed entra in memoria (supponiamolo, altrimenti diventerebbe più complesso e andrebbe fatto con [map/reduce](Map%20reduce.md)).
Osservando che il costo è quello di generare tutte le possibili coppie di item $A\times A$, il procedimento è il seguente:
- Si costruisce una tabella hash grande quanto mi entra in memoria e che usa come chiave la coppia di item.
- Per ogni basket $A$ genero $A\times A$ e conto il numero di 2-itemset che restituiscono lo stesso valore hash. In questo modo a causa delle collisioni potrebbero esserci coppie il cui support count è minore della soglia che passano comunque il test perché si aggregano con altre; non mi interessa perché l'obiettivo è quello di limitare le coppie su cui si fa effettivamente il conteggio.
- Per ogni basket $A$ genero $A\times A$ e controllo se il support count legato al loro hash è maggiore della soglia $s$: se è minore la scarto; se è maggiore la coppia passa il controllo ed è una candidata a frequent 2-itemset. 
- Conto effettivamente le coppie che hanno passato il passo precedente e genero i frequent 2-itemset.