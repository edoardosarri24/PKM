In questo contesto si intende il testing del comportamento funzionale; nella software engineering quando si parla di funzionale si intende la prospettiva dei requirement di sistema (che dicono cosa si fa); quando si parla di non funzionale si intende tutti gli altri aspetti implementativi.
Il testing è un metodo di verifica che vuole identificare difetti nell'Implementation Under Testing (IUT) tramite esperimenti disciplinati (che seguono un razionale) e dinamici (non statici, si valuta il sw in fase di esecuzione).
Tramite il testing si possono trovare difetti nel nostro codice, ma non si può definire la loro assenza. Ci impongono di scrivere codice in un determinato modo per non avere poi un testing troppo oneroso; se il codice non è scritto bene testarlo è molto difficile.
# ERROR-DEFECT-FAILURE
Questi aspetti possono essere visti a livelli diversi:
- Componente
	Si parla di error-to-failur. Nel senso che un errore porta al fallimento del componente.
- Sistema
	Si parla di failure-to-error. Nel senso che il fallimento di un componente può essere un error nel sistema, o in un componente di più alto livello.
##### Ontologia
Nella disciblina della dependability si parla di fault-erorr-failure chain.
- Error
	È un elemento nel processo di sviluppo che porta a un difetto.
- Defect
	È un elemento implementativo che porta a un fallimento.
- Failure
	È una deviazione dai requisiti funzionali.
# ATTIVITÁ
### Test case selection
Una volta individuati i fallimenti che vogliamo coprire, dobbiamo selezionare un insieme di test case che possano portare a quel fallimento.
La scelta dei test case si basa su una qualche astrazione funzionale (è una pratica funzionale, cioè che non osserva il codice) dell'IUT. L'astrazione funzionale non è comunque l'unica possibile:
- Funzionale
	È un'astrazione black box (so cosa l'implementazione fa ma non come lo fa) e si riferisce alle specifiche dell'IUT (es: usa case diagram, SRS).
- Strutturale
	È un'astrazione white box (so cosa l'implementazione fa e anche come lo fa) e si riferisce all'implementazione dell'IUT (es: class diagram, SDD).
- Architetturale
	È un'astrazione grey box (so cosa l'implementazione fa e anche come lo fa) e si riferisce all'architettura dell'IUT (es: IRS; è l'architettura eseguibile).
### Input
Si devono scegliere quali input fa eseguire all'IUT in base alla probabilità che questi portano al fallimento scelto.
Non sempre tutti gli input possono essere controllati: se il test case è scelto basandosi sui un'astrazione allora non tutti i punti dell'astrazione potrebbero essere raggiunti.
### Esecuzione
Nell'esecuzione alcuni componenti saranno emulati. Questo porta a due vantaggi: poter testare il codice senza aver a disposizione tutto il sistema; poter testare i componenti in isolamento, cioè senza che possano essere influenzati dal comportamento di altri componenti.
### Oracolo
È quell'entità che ci dice se il comportamento del test è corretto o no, cioè se il test ha verificato i requisiti funzionali.
Può succedere che l'oracolo non sia conclusivo a causa del fatto che il programmatore non ha il pieno controllo su tutto. Questo è il motivo per cui è l'attività più difficilmente controllabile.
### Debugging
È l'attività che parte dal fallimento e analizza il percorso (possibili error/fault) all'indietro che hanno portato a quella situazione.
Un fallimento può essere causato da più error/fault e quindi corretto da più modifiche che possono portare a nuovi error/fault.
### Copertura
Come nel caso del test case selection la fase di copertura si basa su una qualche astrazione strutturale (è una pratica che osserva il codice e quindi strutturale) dell'IUT. Nel determinare il grado di copertura dell'IUT dobbiamo basarci su un'astrazione di tipo strutturale, cioè dobbiamo osservare l'implementazione.
# CONTROL FLOW TESTING
Fornisce un'astrazione dell'IUT, detta control flow graph.
È applicabile sia per la scelta dei test case che per l'analisi di copertura.
È applicabile sia in contesti strutturali sia in contesti funzionali.
### Control flow graph (CFG)
Il control flow graph rimuove la dipendenza dai dati, facendoci focalizzare sulla struttura del codice: non si controllano le variabili, ma ci interessa solo come prendiamo le decisioni. Rappresenta per questo motivo un'astrazione utile per valutare gli errori sulle variabili di controllo del flusso.
- I vertici possono essere linee di codice (LOC) o blocchi di base (ma si può ampliare la granularità e considerare anche le chiamate alle funzioni). I blocchi di base sono l'insieme massimale di istruzioni $\langle S_1,\cdots,S_N\rangle$ che possono eseguire inseme, cioè tali che $S_i$ è l'unica istruzione precedente a $S_{i+1}$ e l'unica successiva a $S_{i-1}$ (cioè senza istruzioni condizionali o salti e $return$).
- Gli archi sono relazioni tra esecuzioni di blocchi successive.
### Criteri di copertura
##### All nodes
- Si visita ogni nodo del grafo in modo da garantire che ogni linea di codice sia stata eseguita.
- È sufficiente per molte situazioni di safety critical system.
- Se $n$ è il numero di nodi ha una complessità di $\mathcal{O}{(n)}$.
- Le sue limitazioni sono:
	- Non garantisce la copertura di tutti gli archi.
	- Non distingue come un ciclo è stato lasciato, se con la guardia, se con un $break$, un $return$.
	- Non considera quante volte un ciclo è stato eseguito.
##### All edge
- Si visita ogni arco del grafo in modo da garantire che tutte le decisioni siano state coperte.
- Comprende all nodes.
- Se $n$ è il numero di nodi e c è il massimo grado di uscita da un'istruzione (numero di $switch-case$) ha una complessità di $\mathcal{O}{(cn)}$.
- Le sue limitazioni sono:
	- Non garantisce che tutte le guardie siano analizzate con tutti i possibili valori, anche se questi portano poi alla stessa decisione. Questo è rilevante quando una guardia può produrre un side effect (variazione di stato quando un'istruzione viene eseguita, come un cambio di valore di una variabile).
##### Multiple condition
- Estende all edge coprendo tutte le possibili combinazioni delle condizioni (espressione elementare, senza connettivi logici) della guardia, cioè valutando tutte le possibili configurazioni delle varie condizioni che compongono la guardia.
- È utile quando una qualche combinazione produce un side effect che poi porta a un fallimento.
- Se $n$ è il numero di nodi, c è il massimo grado di uscita da un'istruzione (numero di $switch-case$) e $k$ è il massimo numero di condizioni di una guardia allora ha una complessità di $\mathcal{O}{(cn2^k)}$.
##### Modified Condition Decision Coverage (MCDC)
- Estende all edge coprendo tutte le possibili combinazioni delle condizioni determinanti. Una condizione è determinate quando: se essa è vera allora la guardia ha un certo valore; se essa cambia allora la guardia cambia valore.
- Se $n$ è il numero di nodi, c è il massimo grado di uscita da un'istruzione (numero di $case$) e $k$ è il massimo numero di condizioni di una guardia allora ha una complessità di $\mathcal{O}{(ckn)}$.
##### All path
- Si visita ogni percorso del grafo in modo da garantire che tutti i predicati siano stati coperti.
- Comprende all edge (e quindi all nodes).
- Se $n$ è il numero di nodi allora ha una complessità di $\mathcal{O}{(2^n)}$; inoltre è unbounded se i cicli possono essere eseguiti un numero illimitato di volte. Avendo una complessità esponenziale non praticabile nella teoria. Quello che si può fare è usarlo solo in contesti dove ci sono pochi nodi.
# DATA FLOW TESTING
Alla base del data flow testing c'è il concetto che un error/defect non causa necessariamente un fault finché il side effect che lo ha generato non viene effettivamente eseguito. Questo significa che se un error/defect coinvolge una variabile, allora tale error/defect si propaga finché la variabile interessata non viene utilizzata nel programma.
È applicabile sia per la scelta dei test case che per l'analisi di copertura.
È applicabile prima in contesti strutturali e poi in contesti funzionali.
### Data flow graph (DFG)
L'astrazione usata è il data flow graph. Lo scopo è quello di eseguire alcuni percorsi da dove si è verificato il side effect fino a dove la variabile in questione è stata utilizzata.
##### Annotazioni
Estende il CFG con le annotazioni $def-x$ e $use-x$ per ogni variabile rilevante (non tutte le variabili quindi sono prese in considerazione).
- $def$ indica quando la variabile $x$ è assegnata (quando si è creato il side effected).
- $use$ indica quando la variabile $x$ è usata. Ci sono due possibili utilizzi della variabile che riguardano le conseguenze:
	- $c-use-x$
		Indica che $x$ è fuori da una guardia. Porta a una propagazione del fault.
	- $p-use-x$
		Indica che $x$ è usata all'interno di una guardia. Porta a prendere un flusso di esecuzione sbagliato.
### Criteri di copertura
Se $h$ è il massimo grado di uscita da un'istruzione (numero di $switch-case$) e $n$ è il numero di nodi allora il costo per ognuno dei criteri è $\mathcal{O}{(hn^2)}$; questo è un criterio sia sufficiente che necessario.
Si vogliono considerare i $def-clear-path-x$, cioè quei percorsi che iniziano con una $def$, finiscono con una $use$ e in mezzo non hanno altre $def$; non ci devono essere altre $def$ ma ci possono essere altre $use$.
##### All def
- Per ogni $def$ viene coperto almeno un percorso fino a un qualche $use$.
- Non è comparabile con all node o all edge del control flow graph. Non ci garantisce che tutti i nodi o gli archi siano coperti.
##### All uses
- Per ogni $def$ viene coperto almeno un percorso fino a tutti gli $use$ coperti da un commino $def-clear-path-x$.
- Comprende all edge (e quindi al nodes) del control flow graph.
##### All-DU-paths
Per ogni $def$ vengono coperti tutti i percorsi fino a tutti gli $use$ coperti da un commino $def-clear-path-x$.
- Comprende all path e quindi all edge (e all nodes) del control flow graph.
# FINITE STATE TESTING
I sistemi reattivi sono sistemi che mantengono un’interazione continua con l’ambiente che li circonda, il quale spesso non è prevedibile. Esempi sono i sistemi operativi, i sistemi di controllo oppure i componenti sw embedded.
##### Macchina a stati finiti
Per definire il comportamento di un sistema si usa una macchina a stati finiti. È definita da $\langle S,I,O,E\rangle$ dove:
- $S$ è l'insieme degli stati; lo stato iniziale si indica con $S_0$.
- $I$ è l'insieme degli input.
- $O$ è l'insieme degli output.
- $E\subset S\times I\times O\times S$ è l'insieme delle transizioni.
### Testing
Quando si parla di testing dei sistemi reattivi si vuole focalizzare l'attenzione sul comportamento del sistema, più che sul processo di trasformazione dei dati (come abbiamo visto per [control flow testing](Testing.md#CONTROL%20FLOW%20TESTING) e [data flow testing](Testing.md#DATA%20FLOW%20TESTING)).
Quando si usa una macchina a stati finiti nella specifica dei requisiti, allora questa sarà la base su cui costruire la nostra implementazione; quando arriveremo ai test dovremo far si che la nostra implementazione si comporti come la FSM che descrive il sistema.
##### Conformance testing
Testare un sistema sulla base di una FSM, che descrive formalmente il sistema e i suoi requisiti, vuol dire cercare i fault sulla base del comportamento del sistema. In questo caso si parla di test di conformità.
Equivalenza tra una specifica S e un IUT I:
- Uno stato $s\in S$ e un'implementazione $i\in I$ sono $V-equivalent$ se e solo partendo da $s$ e $i$, la stessa sequenza di input di lunghezza $V$ genera la setssa sequenza di output.
- Uno stato $s\in S$ e un'implementazione $i\in I$ sono equivalenti se sono $V-equivalent$ per ogni $V$.
- Due FSM sono equivalenti se i loro stati iniziali sono equivalenti.
##### Fault model
Nel testing di una macchina a stati finiti i fault di cui andiamo a caccia sono ortogonali ispetto a quelli del control/data flow testing. Possono quindi essere:
- Output fault
	Il prossimo stato è corretto ma l'output è sbagliato.
- Transfer fault
	Il prossimo stato è sbagliato ma l'output è corretto.
### WP-method
Si tratta di un metodo di testing black-box: si applicano degli input e si osservano degli output; costruendo bene le sequenze di input si può determinare se l’implementazione è conforme alla specifica.
- La specifica, che sarà una FSM, è soggetto a delle assunzioni:
	- Completely specified
		In ogni stato tutti gli input sono accettati. Nel caso questo non sia rispettato basta aggiungere un arco che fa restare in quello stato.
	- Observable
		In ogni stato tutti gli input producono un qualche output. Nel caso non sia rispettato si può usare una parola nulla, cioè un'input fittizio che produca un qualche output.
	- Deterministic
		Dato stato e input si ha sempre una transizione e un output.
- L'IUT, che sarà l'implementazione della FSM della specifica, è soggetta a delle assunzioni:
	- È completamente specificata, osservabile e deterministica.
	- L'operazione di reset è corretta. Questa operazione ci porta sempre nello stato iniziale del sistema. Può essere onerosa a seconda del sistema implementato.
	- Ha lo stesso numero di stati della FSM della specifica. Questa è l'assunzione più difficile da garantire, perché nell'implementazione ci possono essere più stati.
##### Identificazione
Tramite l'identificazione si può capire in quale stato del sistema ci troviamo; questo è utile perché dopo il reset, l'IUT si trova in un qualche stato, ma non sappiamo in quale.
Avviene tramite l'applicazione di un input o una sequenza di input; una serie di input daranno una sequenza di output e quindi, siccome la FSM è deterministica, si può trovare lo stato attuale.
##### Step del metodo
- Characterization test
	Si deve selezionare un characterization test $W$, cioè un insieme di sequenze di input che permettono di scoprire lo stato iniziale.
	È importante trovare un compromesso tra la lunghezza e il numero delle sequenze: a volte una sola sequenza non è sufficiente per trovare lo stato iniziale; spesso dipende dal costo dell'operazione di reset.
- State cover
	Si seleziona un test suite $Q$ con un test case in ogni stato della specifica. Si ottiene lo state cover con la suite $Q\times W$ (cioè dato da ogni combinazione degli elementi di $Q$ e $W$).
	Se l'IUT passa il test cover allora ogni suo stato è identificato correttamente e in modo univoco.
	Aggiungere un suffisso $W$ garantisce che lo stato dell'IUT corrisponda a quello della specifica.
- Test suite
	Selezionare una test suite $P$ con un test case che attraversa ogni arco e che termina.
##### Osservazioni
Il WP-method garantisce la copertura completa e il rilevamento dei fault; ovviamente questo (che in generale non sarebbe possibile) vale solo con delle assunzioni:
- La funzione reset è corretta, cioè porta sempre nello stato inziale.
- L'IUT è determinisco, full specified e osservabile.
- L'IUT ha lo stesso numero di stati della specifica.

La complessità è pari a $|P|\cdot|W|$, cioè $\mathcal{O}{(N^3)}$.