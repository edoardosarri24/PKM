Nella nostra analisi andremo a considerare l'[istante critico](istante%20critico.md) di un task.
### Assunzioni
Avremo delle assunzioni per semplificare il lavoro:
- Il job si attiva ogni periodo $T$.
- Ogni job del task $\tau_i$ ha lo stesso WCET $C_i$.
- I task sono puramente periodici, per cui $D_i=T_i$.
- Tutti i task sono indipendenti, cioè non ci sono vincoli di risorse o relazioni di precedenza.
- I task non si possono sospendere (ad esempio per operazioni di I/O).
##### Parametri
Definiamo altri parametri, oltre a quelli già visti nei [task](Real%20time/task.md#Definizioni):
- Fase
	È il momento del rilascio del primo job del task.
- Iperperiodo
	È il minimo intervallo dopo il quale lo schedule si ripete, cioè $H=mcm(T_1,\cdots,T_n)$. Questo nel caso non ci siano deadline mancate.
- Job response time
	$R_{ik}=f_{ik}-a_{ik}$.
- Task response time
	$R_i=max\{R_{ik}\}$.
# FATTORE DI UTILIZZO
È il rapporto $U=\sum_{i=1}^n\tfrac{C_i}{T_i}$. [Esempio](processor%20utilizzation%20factor.png). Osserviamo che: non dipende dall'algoritmo di scheduling; può essere incrementato aumentando il computational time o diminuendo il periodo dei task.
- L'upper bound del fattore di utilizzo $U_{ub}(\Gamma, A)$, dato il task set $\Gamma$ (definito da numero e periodo dei task) e l'algoritmo di scheduling $A$, è il valore massimo di $U$ sotto il quale $\Gamma$ è feasible e sopra il quale non lo è più. Se $U=U_{ub}$ allora si dice che $\Gamma$ utilizza interamente il processore.
- Il least upper bound $U_{lub}(A)$ di $U$ dato l'algoritmo di scheduling $A$ è $U_{lub}(A)=min_{\Gamma}U_{ub}(\Gamma,A)$. Attraverso il least uppur bound si può rispondere alla domanda se un task set è schedulabile dall'algoritmo $A$: se $U\le U_{lub}$ allora si; se $U>1$ allora no; se $U_{lub}<U<1$ non si può sapere a priori (si deve provare).
##### Teorema
Se il fattore di utilizzo di un task set $\Gamma$ è maggiore di 1 allora $\Gamma$ non è feasible.
- [Dimostrazione](fattore%20di%20utilizzo%20>1.pdf).
# ALGORITMI
- [Cyclic executive scheduling](Cyclic%20executive%20scheduling.md)
- [Rate monotonic (RM)](Rate%20monotonic%20(RM).md)
- [deadline monotonic](deadline%20monotonic.md)
- [Earliest deadline first (EDF)](Earliest%20deadline%20first%20(EDF).md)
### Confronto
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