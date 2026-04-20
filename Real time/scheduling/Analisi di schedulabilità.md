# CONFRONTO
Vediamo un [resoconto](confronto%20analisi%20di%20schedulabilità%20alg%20periodici.png) dell'analisi di schedulabilità per lo [scheduling](scheduling.md) di task periodici.
- Fattore di utilizzo
	- LL bound. Condizione sufficiente (se negata non si può dire che il taskset non è schedulabile).
	- LL bound per task armonici. Condizione necessaria e sufficiente (se negata si può dire che il taskset non è schedulabile).
	- HB bound. Condizione sufficiente (se negata non si può dire che il taskset non è schedulabile).
	- EDF bound. Condizione necessaria e sufficiente (se negata si può dire che il taskset non è schedulabile).
	- Complessità lineare
- Response time analysis
	- Condizione necessaria e sufficiente
	- Complessità pseudo-polinomiale.
- Processor demand analysis
	- Condizione necessaria e sufficiente
	- Complessità pseudo-polinomiale.
# CON RISORSE CONDIVISE
Se il blocked time è unbounded allora di sicuro un taskset non è feasible, cioè non esiste un algoritmo che genera uno schedule feasible.
Se il blocked time è bounded allora non possiamo ancora dire se un taskset sia feasible, ma dobbiamo integrare questa informazione con l'analisi di schedulabilità fatta per i vari [scheduling](scheduling.md). Questa analisi non si fa più sull'intero taskset, ma si fa task per ogni task in modo che sia un'analisi pessimistica: considerando per ogni task il maggior tempo di blocco a cui può andare in contro siamo sicuri che se è schedulabile in questa situazione allora lo sarà sempre; questa è un analisi sufficiente, perchè se negata non possiamo dire che il task non sia schedulabile; inoltre non per forza tutti i task incorrono nel maggior tempo di blocco possibile.
Il razionale di queste [tecniche](schedulabilty%20test%20with%20resources.pdf) è quindi per ogni task considerare sia l'interferenza che un task subisce dai task a priorità più alta sia il tempo di blocco che subisce dai task a priorità più bassa.
##### Caratteristiche
- RM
	- Poca efficienza. Infatti il fattore di utilizzo tende al [69%](asintotic%20LL%20bound%20for%20RM.png) al crescere del numero dei task.
	- Semplicità di implementazione con un Real-Time OS.
	- Durante l'overload (arrivano più task del previsto) ha molta predicibilità a causa della priorità statica. Lo svantaggio è che i task a priorità più bassa saranno bloccati dai task a priorità più alta (che saranno più del dovuto), mentre quelli a priorità più alta saranno eseguiti al proprio tasso.
- EDF
	- Molto efficente. Infatti il fattore di utilizzo è del 100%.
	- Basso numero di preemption. Questo implica un basso overhead dovuto al context switch.
	- Durante l'overloads è molto flessibile grazie alla priorità dinamica. Lo svantaggio è che tutti i task saranno eseguiti a un tasso più basso del normale, perché anche i task che hanno bassa priorità diventeranno nel futuro quelli a priorità più alta.
	- Maggiore reattività nella gestione dei task aperiodici.
##### [Analisi di schedulabilità](Analisi%20di%20schedulabilità.md)