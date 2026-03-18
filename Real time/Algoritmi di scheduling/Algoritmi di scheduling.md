# [Scheduling di task periodici](Scheduling%20di%20task%20periodici.md)
---
![scheduling algorithms hierarchty](scheduling%20algorithms%20hierarchty.png)
Si può categorizzare in:
- Offline
	Le decisioni sono prese prima dell'attivazione dei task. Si parla di table-driven scheduling.
- Online
	Le decisioni sono prese a runtime durante l'esecuzione dei task.
- Preemptive
	Quando sta computando sulla CPU, un task può essere interrotto da uno a priorità più alta.
- Non preemptive
- Statico
	Le decisioni sono prese sulla base di parametri che sono stati decisi prima dell'attivazione dei task.
- Dinamico
	I parametri dei task cambiano durante il tempo.
- Ottimo
	Genera uno schedule che minimizza una funzione di costo definita da un criterio di ottimalità.
- Euristico
	Genera uno schedule che prova a soddisfare un criterio di ottimalità senza garanzie di successo.
- Garantito
	Genera uno schedule feasible se esiste. Utile nei casi in cui si richiede un hard real-time.
- Best effort
	Non lo garantisce. Si usa nei contesti di soft real-time visto che ottimizza le prestazioni medie.
# SCHEDULER
Informalmente uno scheduler assegna al processore un task.
Formalmente, dato un insieme di task $\Gamma=\{\tau_1,\cdots,\tau_n\}$, uno scheduler è una funzione $\sigma:R^+\to N$ che $\forall t\in R^+,\exists t_1,t_2\in R^+$ tale che, $\sigma(t)=\sigma(t')$, con $t\in[t_1,t_2)$ e $\forall t'\in[t_1,t_2]$; se $\sigma(t)=0$ si dice che il processore è $idle$ e non vi è alcun task in esecuzione; altrimenti se $\sigma(t)=k\in\{1,\cdots, n\}$ allora sul processore al tempo $t$ è in esecuzione il task $\tau_k$.
Praticamente, per ogni istante d tempo, esiste un intervallo in cui la funzione $\sigma$ è costante, cioè il task in esecuzione è lo stesso.
- Ogni intervallo $[t_i,t_{i+1})$ si dice $time\ slice$. Alla fine e all'inizio di ogni time slice il processore esegue il context switch, cioè salva i descrittori del task che subisce la preemption e carica in memoria quelli del nuovo task.
- Il problema dello scheduling, dasto un insieme di task $\Gamma$, un inseme di processori $P$, un insieme di risorse $R$ e un insieme di vincoli $C$, consiste nel trovare un assegnamento a $P$ e $R$ per $\Gamma$ in modo da produrre uno schedule (soluzione di scheduling) $feasible$ che soddisfi i vincoli $C$. Questo problema è NP-completo (diventa polinomiale se rilassiamo alcuni vincoli).
### Feasible
Un insieme di task $\Gamma$ è $schedulable$ da un algoritmo $A$ se $A$ genera uno schedule $feasible$ per $\Gamma$.
Uno schedule è detto $feasible$ se tutti i vincoli dei task sono soddisfatti:
- Tempo
	Attivazione, periodo, deadline.
	Possono essere espliciti se sono inclusi nella specifica, altrimenti sono impliciti (se sono obbligati dal contesto).
- Precedenza
	Si impone un ordine all'esecuzione dei task. Si esprime tramite un Directed Acyclic Graph (DAG) chiamato task graph.
- Accesso alle risorse
	Si impone una sincronizzazione di accesso in mutua esclusione alle risorse condivise per risolvere problemi di accesso concorrente.
# TEOREMA (Graham, 1976)
Se un set task dove ogni task ha la sua priorità assegnata, è schedulato in maniera ottima su un dato numero di processori, un dato tempo di esecuzione e certi vincoli, allora aumentare il numero di processori, ridurre il tempo di esecuzione o indebolire i vincoli può aumentare la lunghezza dello schedule. [Esempio](esempio%20teorema%20di%20Graham.png) (processore 2 volete più veloce; in giallo è CS).
Questo ci dice che il problema dello scheduling è molto complesso.
# BUONE NORME
- In un time slice se possibile è cosa buona lasciare un istante di tempo libero per poter gestire operazioni di overhead.
- Se è possibile usare meno possibile il context switch: se un task ha tempo per eseguire allora non interromperlo spezzandolo in due o più pezzi.
- Quando cambia qualche requisito il progettista deve
	- Rifare il design dello scheduler.
	- Cercare di diminuire il WCET tramite migliorie al codice.
	- Rivedere i periodi dei task, allungandoli se necessario (e possibile).