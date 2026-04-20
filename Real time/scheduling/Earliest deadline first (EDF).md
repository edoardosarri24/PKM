Priorità assegnata a ogni task in modo inverso rispetto alla proria deadline assoluta. [Esempio](EDF%20example.png).
- Preemptive
- Dinamico
- Online
- Task periodici e aperiodici
	Noi lo vedremo per task periodici e puramente periodici, ma le sue proprietà valgono anche per task aperiodici.
- Priority-driven
	Questi algoritmi assegnano una priorità a ogni task sulla base della quale viene implementato lo schedule.
##### Protocollo di accesso alle risorse
Se abbiamo una priorità dinamica dei task come in questo caso allora il classico [Priority ceiling protocol](Priority%20ceiling%20protocol%20(PCP).md) diventa [Stack resource policy](https://en.wikipedia.org/wiki/Stack_resource_policy#:~:text=The%20Stack%20Resource%20Policy%20(SRP,fixed%20priority%20tasks%20(FP).).
# OTTIMALITÁ
Questi teoremi sono indipendenti dalla periodicità. Valgono sia per task periodici che per task non periodici.
##### Teorema
EDF è ottimo rispetto alla proprietà di feasible tra tutti gli algoritmi.
- Questo vuol dire valgono contemporaneamente:
	- Se uno schedule è feasible per un taskset $\Gamma$ allora anche lo schedule di DM è feasible per $\Gamma$.
	- Se lo schedule del EDF non è feasible per un task set $\Gamma$ allora non esiste uno schedule feasible per $\Gamma$.
- [Dimostrazione](proof%20of%20optimality%20for%20EDF.pdf) (tramite la trasformazione di Dertouzos).
##### Teorema
Dati $n$ task indipendenti, ogni algoritmo che ordina la priorità dei task in ordine non-decrescente rispetto alla deadline assoluta è ottimo rispetto alla minimizzazione della lateness massima.
- [Dimostrazione](proof%20of%20optimality%20minimum%20lateness%20for%20EDF.pdf)
- Osserviamo che se un algoritmo minimizza la lateness allora questo è ottimo rispetto alla proprietà di feasible.
# TEST DI SCHEDULABILITÁ
### Guarantee test (Lui & Layland, 1973)
##### Teorema
Un set di task puramente periodici è schedulable da EDF se e solo se $U\le U_{lub}=1$.
- Questo test è necessario e sufficiente: se non è verificato allora $\Gamma$ non è schedulable da nessun algoritmo; se verificato allora $\Gamma$ è schedulable da EDF.
- Ha una complessità lineare ($O(n)$) rispetto al numero di task.
- Dimostrazione
	- [Necessaria](proff%20of%20necessary%20EDF.pdf)
	- [Sufficiente](proof%20of%20sufficiency%20EDF.pdf)
# TASK PERIODICI - Processor demand analisys
È una analisi necessaria e sufficiente.
### Processor demand criterion (Baruah, 1990)
##### Teorema
Un set di $n$ task periodici, con $D_i\le T_i\ \forall\tau_i$, è schedulable da EDF se e solo se in ogni intervallo $[t_1,t_2]$ la domanda del processore $g(t_1,t_2)$ non eccede i tempo disponibile, cioè se $g(t_1,t_2)\le t_2-t_1,\ \forall t_1,t_2$ con $t_1<t_2$.
- Questo teorema è necessario e sufficiente.
- [Come calcolare la g](Processor%20demand%20criterion%20for%20EDF.pdf).
### Demand bound test
##### Test
Un set di n task periodici, con $D_i\le T_i\ \forall\tau_i$, rilasciati tutti nello stesso momento (caso pessimo) è schedulable da EDF se e solo se $U<1$ e $dbf(t)\le t,\ \forall t\in D$, con $D=\{d_i|d_i\le min\{d_{max},t^*\}\}$ e $t^*=\sum_{i=1}^n(T_i-D_i)U_i/(1-U)$.
##### Funzionamento
È il processor demand criterion nel caso pessimo, cioè quando tutti i task arrivano al tempo 0 (i.e. $\phi_i=0\ \forall \tau_i$). In questo caso si definisce $dbf(t):=g(0,t)=\sum_{i=1}^n\eta_i(0,t)C_i=\sum_{i=1}^n\lfloor\tfrac{t+T_i-D_i}{T_i}\rfloor C_i$.
Osserviamo che il taskset è schedulable se $dbf(t)\le t$, cioè se, [nel grafico](demand%20bound%20function%20for%20EDF.png), la funzione $dbf$ sta sotto la retta $y(t)=t$; siccome $dbf$ è una funzione a gradini che cresce allo scoccare di ogni deadline assoluta, dovrò controllarlo solo allo scoccare delle varie deadline.
Posso definire una nuova funzione che è sempre maggiore o uguale di $dbf$ e che ha pendenza minore di $y(t)$. Allora in [questo](controlo%20per%20dbf.pdf) modo posso controllare solo i valore di t minori di $t^*$.