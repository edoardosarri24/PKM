È una famiglia di algoritmi in stile [reattivo](paradigmi%20di%20agenti.md#Reattivo) per risolvere il problema del [robot motion planning](robot%20motion%20planning.md).
##### Supposizioni
Si richiede:
- Conoscenza globale del goal.
- Conoscenza locale dell'ambiente acquisita tramite sensori tattili: un ostacolo viene identificato solo quando ci si sbatte contro.
##### Comportamenti
I behavior che si utilizzano sono:
- Muoviti verso il goal.
- Segui l'ostacolo.
	La scelta del senso (orario o antiorario) è fatta in fase di progettazione; per questo motivo questo è un algoritmo deterministico. Ci sono versioni stocastiche che decido in modo randomico il senso in cui seguire l'ostacolo.
# Bug 0
I comportamenti sono alternati in questo modo:
- Segui la dizione del goal finché non incontri un ostacolo o il goal stesso.
- Segui l'ostacolo finchè non si può andare nuovamente verso il goal.
### Conseguenze
- Completezza
	Non è un algoritmo completo.
	Sopratutto nei percorsi labirintici è possibile che non termini mai.
# Bug 1
Si deve poter misurare la distanza dl goal. I comportamenti sono alternati in questo modo:
- Segui la dizione del goal finché non incontri un ostacolo o il goal stesso.
- Segui tutto l'ostacolo fino a tornare nel punto in cui lo si è incontrato, memorizzando per in ogni istante la distanza dal goal.
- Ritorna al punto dove la distanza dal goal era minima e vai verso il goal.
### Conseguenze
- Completezza
	È completo perché la distanza dal goal ogni volta che si lascia l'ostacolo diminuisce e quindi è una successione monotona decrescente rispetto alla distanza dal gol.
- Esaustivo
	È una ricerca esaustiva: valuta tutte le possibilità prima di proseguire.
- Bound
	Data $D$ la distanza start-goal, allora:
	- $D$ è il least bound sulla distanza che si percorre.
	- $D+\tfrac{3}{2}\sum_kP_k$ è l'upper bound sulla distanza che si percorre, dove: $k$ è il numero di ostacoli, $P_k$ è il perimetro dell'ostacolo $k$-esimo e $\tfrac{3}{2}$ viene dal fatto che si fa un giro completo di ogni ostacolo e al massimo si deve tornare nel punto di mezzo.
- Efficienza
	È un buon algoritmo quando siamo in percorsi labirintici.
# Bug 2
Si deve poter tracciare la goal line, cioè la retta che passa per lo start e il goal. I comportamenti sono alternati in questo modo:
- Segui la dizione del goal finché non incontri un ostacolo o il goal.
- Segui tutto l'ostacolo finché non incontri la goal line in un punto più vicino al gol rispetto a quello in cui avevi trovato l'ostacolo. Senza quest'ultima condizione l'algoritmo è molto simile a [bug 0](#Bug%200) e quindi non completo.
### Conseguenze
- Completezza
	È completo perché la distanza dal goal ogni volta che si lascia l'ostacolo diminuisce e quindi è una successione monotona decrescente rispetto alla distanza dal gol.
- Greedy
	È un algortimo greedy dove si prende la miglior decisione in quel momento. Si incontrano solo gli ostacoli che vanno dallo start al goal.
- Bound
	Data $D$ la distanza start-goal, allora:
	- $D$ è il least bound sulla distanza che si percorre.
	- $D+\tfrac{n_k}{2}\sum_kP_k$ è l'upper bound sulla distanza che si percorre: $k$ è il numero di ostacoli, $P_k$ è il perimetro dell'ostacolo $k$-esimo e $n_k$ è il numero di crossing point tra il perimetro dell'ostacolo e la goal line.
- Efficienza
	Spesso è migliore di [Bug 1](bug%20algorithms.md#Bug%201) tranne che nei percorsi labirintici.