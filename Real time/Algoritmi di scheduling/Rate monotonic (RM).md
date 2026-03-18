Priorità assegnata a ogni task in modo inverso rispetto all'ampizza del periodo. [Esempio](rate%20monotonic%20example.png).
- Preemptive
- Statico
- Online
- Task puramente periodici
- Priority-driven
	Questi algoritmi assegnano una priorità a ogni task sulla base della quale viene implementato lo schedule.
# OTTIMALITÁ
##### Teorema
Il rate monotonic è ottimo rispetto alla proprietà di feasible tra tutti gli algoritmi basati su priorità fissa per lo scheduling di task puramente periodici.
Questo vuol dire valgono contemporaneamente:
- Se uno schedule a priorità fissa è feasible per un task set $\Gamma$ allora anche lo schedule di RM è feasible per $\Gamma$.
- Se lo schedule del RM non è feasible per un task set $\Gamma$ allora non esiste uno schedule feasible a priorità fissa per $\Gamma$.
##### Teorema
Se uno schedule a priorità fissa è feasible per un task set $\Gamma$ all'istante critico allora lo schedule di RM è feasible per $\Gamma$ all'istante critico.
- [Dimostrazione](prof%20optimality%20for%20RM.pdf). Siccome ogni task ottiene il suo pessimo response time quando viene attivato all'istante critico, basta verificare l'ottimalità all'istante critico, cioè quando tutti i task vengono attivati insieme.
# TEST DI SCHEDULABILITÁ
### Guarantee test (Lui & Layland, 1973) - LL bound
##### Teorema
Se $U\le U_{lub}=n(2^{1/n}-1)$ di un task set $\Gamma$ con $n$ task puramente periodici, allora $\Gamma$ è schedulabile da RM.
- Dimostrazione
	- Come fare:
		- Si assegna la priorità ai task secondo le regole di RM.
		- Si assume che i task arrivino contemporanemanete, cioè ci mettiamo nello scenario pessimo.
		- Si massimizzano tutti i computation time fino a utilizzare pienamente il processore.
		- Si calcola l'upper bound $U_{ub}$ di $U$.
		- Si minimizza $U_{ub}$ rispetto a tutti gli altri parametri per arrivare a $U_{lub}$.
	- [Con 2 task](RM%20guarantee%20test%20for%202%20task.pdf).
##### Osservazione
- Questo test è sufficiente: se non è verificato $\Gamma$ potrebbe comunque essere schedulable da RM; se è verificato allora è schedulable.
- Ha una complessità lineare ($O(n)$) rispetto al numero di task.
- Se abbiamo un taskset composto da più task dove questi hanno a due a due periodi armonici, cioè se $\tfrac{T_i}{T_j}\in N$ allora $U_{lub}=1$. In questo modo non solo usiamo la CPU al 100%, ma il test diventa condizione sufficiente e necessaria: se $U<1$ allora è $\Gamma$ è schedulable, se $U>1$ non lo è.
- Considerando $U_{lub}=n(2^{1/n}-1)$ si ha [questa](asintotic%20LL%20bound%20for%20RM.png) situazione al variare di $n$. Questo ci dice che usare RM quando il taskset $\Gamma$ cresce di dimensioni implica non avere un fattore di utilizzo molto elevato. Sopra questi valori ricordiamo che $\Gamma$ potrebbe essere schedulabile ma non lo sappiamo a priori.
### Hiperbolic bound (Bini, 2003)
##### Teorema ([articolo](https://www.iris.sssup.it/handle/11382/200358))
Se $\prod_{i=1}^n(U_i+1)\le2$ per un taskset $\Gamma$ con $n$ task puramente periodici allora $\Gamma$ è schedulable da RM.
- [Dimostrazione](proof%20hyperbolic%20bound.pdf).
##### Osservazione
- Questo test è sufficiente: se non è verificato $\Gamma$ potrebbe comunque essere scehdulabile da RM.
- Hyperbolic bound è [tight](tight%20di%20HB%20e%20LLB.png) (LL bound non lo è). Questo vuol dire che dato $\Gamma$ se l'hyperbolic bound non è soddisfatto allora non esiste nessun test che mi possa dire se $\Gamma$ è schedulable.