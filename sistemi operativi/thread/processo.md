Un processo è definito come un programma in esecuzione.
Un processo durante la sua esecuzione è composto da uno o più flussi di lavoro, detti più [thread](thread.md).
# Componenti
Un processo è rappresentato da più cose:
##### Program counter
Contiene l'indirizzo in memoria della prossima istruzione da eseguire.
##### Codice
È caricato in memoria quando il programma diventa processo. Tale spazio di memoria può essere condiviso da più processi che eseguono lo stesso codice.
Solitamente è read-only così che il programma non possa modificare se stesso.
##### Dati globali
Sono dati che possono essere letti da tutto il processo.
##### Heap
È l'area di memoria [allocata dinamicamente](allocazione.md#Heap) durante l'esecuzione. Aumenta in modo crescente dal basso verso l'alto.
Porta a memory leak se non viene liberata durante l'esecuzione.
È molto grande, ma molto più lenta dello stack.
##### Stack
È l'area di memoria che viene allocata quando si chiama una funzione e che viene liberata in automatico quando la funzione ritorna.
Contiene le variabili locali (i.e., quelle che quando si scrive codice sappiamo già definire), i parametri e l'indirizzo di ritorno.
Aumenta in modo decrescente dal basso verso l'alto.
Porta allo stack overflow quando si ha una ricorsione troppo elevata. Si ridimensiona automaticamente.
È piccola, circa 8MB nel sistemi moderni, ma più veloce dell'heap.
# Stati
Il ciclo di vita di un processo percorre vari stati possibili. La stessa cosa vale per i [thread](thread.md).
![stati del task](stati%20del%20task.png)
##### Active
È un task che è pronto per essere eseguito sulla CPU.
Si divide in:
- Ready
	È un task presente nella coda (ordinata secondo l'algoritmo di scheduling) dei task pronti per l'esecuzione.
	- L'operazione di dispatching, che assegna al processore il primo task nella coda, viene fatta dallo scheduler.
- Running
	È un task in esecuzione sulla CPU.
	- L'operazione di preemption, che sospende un task in esecuzione in favore di uno con priorità maggiore, è un meccanismo del kernel.
		- Aumenta la concorrenza tra task, ma se questa diventa troppa allora si rende il meccanismo del context switch non trascurabile in termini di tempo (solitamente lo è).
		- Può essere disabilitato per assicurare l'esecuzione di task critici.
##### Blocked
È un task in attesa di qualche risorsa.
- L'operazione di $wait$, chiamata prima di usare una risorsa condivisa (che deve essere usata in mutua esclusione) gestita da un semaforo, è eseguita dal task stesso.
- L'operazione di $signal$ è eseguita dal task che libera la risorsa, e quindi il semaforo.
##### Idle
È un task che ha finito il suo compito (job) ma deve attendere un timer per riniziare il suo compito (altro job).
Non è blocked perché non sta attendendo che una risorsa venga liberata. Non è active perché lo scheduler non può assegnarlo al processore.
Assumeremo che un task può essere in questo stato solo in questa situazione, cioè che non possa essere forzato in questo stato.