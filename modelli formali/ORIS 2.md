Si tratta di un tool sviluppato dall'università di Firenze e [open source](https://www.oris-tool.org) che permette la modellazione grafica e l'analisi di sistemi concorrenti modellati tramite [TPN](Time%20Petri%20Net%20(TPN).md) e [PTPN](Preemptive%20Time%20Petri%20Nets%20(PTPN).md) e [GSPN](generalized%20stochastic%20petri%20nets%20(GSPN).md).
# Token game
Tramite il token game $T$ possiamo guidare l'esecuzione del modello: scegliere la prossima azione in modo indipendente dalla probabilità che questa sia eseguita veramente o fare un numero di step.
##### Uso
Non è molto utile per quanto riguarda l'analisi vera e propria, ma è molto informativa per quanto riguarda una prima simulazione: possiamo infatti capire se il modello si comporta come ci aspettiamo in linea di massima.
# Analisi
##### Tipi di analisi
Ci sono più tipi di analisi:
- Transient
	È l'analisi della probabilità transiente. Il risutlato è un grafico della reward rispetto al tempo.
- Steady state
	È l'analisi dello [steady state](continuous%20time%20markov%20chain%20(CTMC).md#Steady%20state), cioè quello che succede all'infinito (teorico); il risultato non è un grafico ma dei valori.
	Quando lavoriamo con le catene di Markov con tempo esponenziali allora è plausibile che il comportamento della catena non cambi una volta raggiunto il valore dello steady state; inoltre se la catena è irriducibile (cioè ergotica) allora siamo sicuri che lo steady state esiste che non dipende dalla marcatura iniziale.
	Se vogliamo calcolare dopo quanto arriviamo allora steady state possiamo: calcolare il valore allo stady state; valutare dopo quando raggiungiamo quela valore con una data tolleranza.
- Transient simulation
	La transient analysis e la steady state analysis possono essere eseguite solo se lo spazio degli stati è finito, visto che richiedono la loro enumerazione.
	Spesso questa cosa non è possibile e quindi si deve fare una simulazione.
##### Reward
Un reward definisce un [processo stocastico](stochastic%20process.md). Tramite un reward si possono calcolare le quantità di interesse per il problema modellato dalla rete.
È possibile definire più reward separandoli con un punto e virgola "$;$". Possiamo usare i cumulative rewards, che calcola in pratica l'integrale nel tempo della curva dei reward specificati.
Ci sono tre modi per definire un reward:
- Senza reward
	Semplificando plotta il valore atteso della probabilità di tutte le marcature possibili.
	Il valore atteso è inteso come la media della probabilità che al tempo $t$ ci si trovi in una data marcatura calcolata su tutte le marcature possibili al tempo $t$ (sono molte perché il processo è stocastico e quindi non deterministico).
 - Espressione aritmetica
	Il risultato è un plot che mette in relazione il valore atteso di quanto restituito dall'espressione rispetto al tempo.
- $If(cond,1,0)$
	È la reward che definisce una probabilità: al tempo $t$ la condizione può essere vera (1) o falsa (0); siccome siamo in un sistema concorrente al tempo $t$ la condizione può essere vera in alcune esecuzioni e falsa in altre. Quello che restituisce questa reward è un plot del valore atteso della probabilità che la condizione sia verificata dalla marcatura al tempo $t$.
	Può essere che non si restituisca 1, ma un certo valore; in questo modo abbiamo delle condizioni più complesse ma anche più potenti.