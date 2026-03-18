Una Markov chain è un [processo stocastico](stochastic%20process.md) che verifica la [condizione di markov](condizione%20di%20markov.md). È un modello matematico che descrive un sistema che evolve tra vari stati in modo probabilistico e dove il futuro è definito solo dal presente, mentre il passato è irrilevante.
- [continuous time markov chain (CTMC)](continuous%20time%20markov%20chain%20(CTMC).md)
- [disctete time markov chain (DTMC)](disctete%20time%20markov%20chain%20(DTMC).md)
# Struttura
Il modello è definito da:
- Spazio degli stati $S$.
	È l'insieme delle possibili configurazioni del sistema.
- Grafo
	Una catena di Markov si può rappresentare come un grafo: i nodi sono gli stati e gli archi esprimono le transizioni possibili.
# Stati
- Ricorrente
	Uno stato è ricorrente se la probabilità di tornare (prima o poi) in quello stato è 1 dopo averlo lasciato.
- Transiente
	Uno stato è transiente se una volta lasciato la probabilità di non tornare in esso è maggiore di 0.
- Assorbente
	Uno stato si dice assorbente quando una volta che siamo entrati non si può uscirne.
# Proprietà
- Irriducibili
	Una catena è irriducibile se ogni stato è raggiungibile da ogni altro. Se il sistema ha un numero finito di stati, allora questo implica che tutti gli stati siano ricorrenti.
- BSCC
	Un Bottom Strongly Connected Component è un sottoinsieme di stati tali per cui una volta entrati in esso non è più possibile uscirne.
# Analisi
Possiamo eseguire vari tipi di analisi su una catena di Markov:
* Transient Probability
	È la probabilità che il sistema si trovi in uno stato $i$ dopo $n$ passi, cioè vogliamo trovare $\pi_i(n)=P\{X(n)=i\}$.
* Steady-State
	È lo studio del sistema per $t \to \infty$. La soluzione rappresenta la percentuale di tempo che il sistema trascorre in ogni stato a regime.
	Se la catena è irriducibile allora il sistema converge a una distribuzione $\pi$ che non dipende dallo stato iniziale; questo comportamento è detto ergotico.
* First Passage Probability
	È la probabilità di raggiungere un determinato stato (o una BSCC) partendo da uno stato iniziale, cioè $\eta_i(n)=P\{\exists m\le n: X(m)=i\}$. Si può vedere sia come "prima o poi" raggiungiamo uno stato oppure "entro un certo istante di tempo" raggiungiamo uno stato.
# Risoluzione
Le catene di Markov si risolvono tramite sistemi di equazioni lineari.
Per sistemi con un numero di stati molto elevato, si ricorre a metodi simbolici (come i BDD) per superare la complessità computazionale $\mathcal{O}(n^3)$.