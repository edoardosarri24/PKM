xxMolti degli algoritmi per imparare [alberi decisionali](Decision%20tree.md) sono greedy: a ogni livello viene presa la miglior decisione senza basarci sui livelli superiori o inferiori. In questo modo non c'è garanzia di ottimalità sulla profondità.
In generale trovare un albero di decisione ottimo è un problema np-hard. Usare alberi ottimi permetti di avere i vantaggi di [interpretabilità](Interpretability.md) degli alberi di decisione e di avere un'ottimalità nella dimensione.
##### Idea
L'idea degli optimal decsione tree è quella di definire l'albero ottimo tramite metodi di [Dinamic programming](varie/Dinamic%20programming.md).
L'albero è definito come $tree^*=\displaystyle\min_{tree}\sum_{i=1}^nL(x_i,y_i,tree)+\lambda(\#leave(tree))$ tale che $depth(tree)\le D$.
- Il primo termine della sommatoria definisce la loss della classificazione, cioè quanto bene $tree$ classifica il train set.
- Vincolo sotf
	Il secondo termine della sommatoria è un vincolo regolarizzato da $\lambda$ che influenza il numero di foglie.
	Se non avessi questo termine allora potrei imparare alberi che hanno una determina profondità (definita da $D$) ma non hanno un limite sul numero di percorsi (il numero di foglie definisce anche il numero di percorsi); questo vuol dire che imparerei anche dei sotto alberi che non sono necessari, e quindi non avrei più la soluzione ottima.
	Il regolarizzatore $\lambda$ permette di imparare percorsi più o meno lunghi e/o numerosi: se $\lambda$ è alto infatti dovranno esserci meno foglie per minimizzare la funzione.
- Vincolo hard
	La condizione hard determina in modo rigido la massima profondità dell'albero che voglio ottenere.
	Se escludo questo vincolo allora il metodo diventa molto inefficiente, in quanto non ho un limite di profondità in cui ricercare l'albero ottimo.
# GOSDT - Global Optimal Sparse Decision Tree
L'idea alla base è che per decidere il nodo alla radice si deve sapere quali sono i nodi del livello inferiore; se facciamo questo ragionamento ricorsivamente dobbiamo arrivare a conoscere le foglie dell'albero per poi risalire fino alla radice.
L'albero di decisione ottimo trovato secondo questa idea non è unico, ne potrebbero esistere altri.
##### Ingredienti
- Binary data
	I dati devono essere binari: dato una feature e un valore soglia, una decisione su quella feature deve dividere i dati in due branch. Se le variabili sono categoriche si introducono tante features quante sono le categorie; se le variabili sono numeriche questo si può fare in due modi:
	- Si calcolano delle features binarie per ogni possibile split ([esempio](GOSDT%20binary%20data%20inefficient.jpeg)).  È una tecnica molto inefficiente perché se abbiamo molte variabili allora il numero delle nuove variabili binarie esplode.
	- Per risolvere il problema del metodo precedente si calcola una features binaria solo per i valori di slit più promettenti, trovati tramite un algoritmo efficiente (es: random forest), cioè i valori che suddividono meglio i dati.
- Rappresentazione binaria dei problemi
	È una rappresentazione binaria che indica quali samples sono presi in considerazione in un dato problema: se alla posizione $i$-esima ho un bit 1, allora l'$i$-esimo sample appartiene al problema. Per problema si intende un nodo del grafo (pallino nero nell'esempio) definito da una sua rappresentazione, da un LB e un UB.
	Se consideriamo l'[esempio](GOSDT%20banary%20problem%20representation.jpeg), allora vediamo che il nodo $S_0$ devo considerare tutti gli esempi (il vettore sarà $[1,\cdots,1]$); al livello successivo i samples finiranno un po' a destra e un po' a sinistra e quindi il problema $S_1$ lavorare su esempi che non appartengono a $S_2$.
- Coda con priorità
	$Q=\{S_0,\cdots,S_k\}$, dove ogni $S_i$ è un problema con una priorità. In generale ogni nodo ha la stessa priorità, ma l'algoritmo può avere bisogno di settare la priorità di qualche problema come massima.
- Dependency Graph $G$
	Ci serve per memorizzare i sotto problemi.
	Per ogni nodo (problema) si deve memorizzare un LB (least Bound) e un UP (upper Bound). Se riguardiamo la funzione da minimizzare per gli alberi di decisione ottimali abbiamo $tree^*=\displaystyle\min_{tree}\sum_{i=1}^nL(x_i,y_i,tree)+\lambda(\#leave(tree))$:
	- LB
		Il meglio che possiamo fare per la minimizzazione è classificare correttamente tutti i salmple e quindi pagare 0 per la loss.
		In questo caso allora i samples vengono divisi perfettamente in un due nodi e quindi abbiamo esattamente due foglie.
		Allora in questo caso il least bound è $LB=0+2\lambda$.
	- UP
		È l'errore che abbiamo se ci fermiamo in quel nodo senza migliorare ulteriormente.
		In questo caso la loss è pari al numero $n$ di samples che appartengono alla classe meno rappresentata (la classificazione in un nodo sarà quella della classe più rappresenta all'interno del nodo stesso) e i samples finiranno tutti in una foglia (quel nodo su cui stiamo calcolando l'UB). Allora abbiamo $UP=n+1\lambda$.
##### Algoritmo
![GOSDT algoritmo](GOSDT%20algoritmo.jpeg)
Per spiegare l'algoritmo prendiamo questo [esempio](GOSDT%20computazione%20esempio.pdf) come riferimento.
- Si crea un grafo $G$, inizializzato con il problema master.
- Finché il problema master $p_0$ non è risolto si itera. Per risolto si intende che il suo LB e UB sono uguali.
- Si prende un problema $p$ dalla coda dei problemi $Q$. I problemi sono rappresentati nell'esempio dai pallini neri.
- Si cerca un problema associato a $p$, cioè un problema che abbia la stessa rappresentazione binaria; questo serve per non introdurre sotto alberi che sono già stati visitati. Se non si trova si crea un nuovo nodo nel grafo, che rappresenta $p$, e si calcola il suo LB e UB.
- Se il problema è risolto allora si arresta l'iterazione e si continua con il prossimo problema. Un problema è risolto se $UB\le LB$.
- Se non è risolto dobbiamo suddividerlo e osservare i LB e UB dei livelli inferiori. Lo split nell'esempio è rappresentato dai pallini bianchi. Gli split (uno per ogni features non ancora analizzata nela path dalla radice) si fanno sulla base delle soglie delle features stabilite quando abbiamo prodotto dati binari. Per ogni features $j$ allora produciamo lo split e osserviamo i due problemi che si sono creati: se questi già esistono allora nulla; altrimenti si creano due nuovi problemi ($p_l^j$ e $l_r^j$) e si calcolano i loro UB e LB.
- A questo punto dobbiamo aggiornare il LB e UB del problema da cui si sono generati gli split in base a quelli calcolati al livello inferiore. Per ogni coppia di problemi che deriva da uno split calcoliamo $p_l^j.LB+p_r^j.LB$ e $p_l^j.UB+p_r^j.UB$; calcoliamo ora i minimi di questi due valori rispetto a tutte le features $j$, producendo $LB'$ e $UB'$; a questo punto se $LB'$ è maggiore del $LB$ di $p$ (il padre che ha generato tutti gli split) allora $p.LB=LB'$ e se $UB'$ è minore dell'$UB$ di $p$ allora $p.UB=UB'$.
- Se dopo l'aggiornamento il problema $p$ è risolto si passa al prossimo problema; altrimenti dobbiamo inserire i problemi generati dagli split nella coda. Non si inseriscono (cioè si fa pruning dell'albero a quel livello e non si prosegue) tutti i figli ($p_l^j$ e $p_r^j$) generati per ogni features $j$, ma solo quelli che verificano delle condizioni:
	- Non si inserisce se il figlio è già risolso. In questo caso non si deve splittare ulteriormente e quindi non si proseque.
	- Non si inserisce se il figlio peggiora le cose, cioè se proseguendo in quella strada si raggiungono UP maggiore e LB inferiori.
# Librerie
- Abbiamo visto uno. È nella cartella relativa all'interpretability.