Un sistema ILP è il programma che implementa i concetti della [ILP](Inductive%20Logic%20Programming%20(ILP).md). È ccaratterizzato da:
- Il tipo di learning: [LFE](Inductive%20Logic%20Programming%20(ILP).md#Learning%20from%20entailtment%20(LFE)), [LFI](Inductive%20Logic%20Programming%20(ILP).md#Learning%20from%20interpretation%20(LFI)) o un altro.
- Representation languege
	Definisce che linguaggio posso usare per rappresnetare la background knowledge: se ho dei vincoli (e.g. non si può usare il negato), se posso usare il reasoning numerico, se posso usare tutta la logica del primo ordine (e.g. quantificatore universale o esistenziale).
- Language bias
	Definisce lo spazio delle ipotesi $\mathbb{H}$.
	Per esempio una regola non può essere del tipo $happy(X)\vdash happy(Y)$ perché non avrebbe senso e manca qualcosa.
- Metodo di ricerca
	Come si cerca nello spazio delle ipotesi. È quindi l'algoritmo che il sistema utilizza per imparare le regole.
# Aleph system
È un ILP system.
##### Algoritmo
Vediamo i passi dell'algoritmo, cioè del metodo di ricerca:
- Si scegli un esempio positivo, cioè qualcosa che appartiene a $E^+$. L'obiettivo da qui in avanti è di trovare una regola che copra questo esempio il più generica possibile.
- Si ripete finchè $E^+$ non contiene più esempi:
	- Si costruisce la bottom clause, cioè clausola più specifica possibile che soddisfa l'esempio e che è consistente con il language bias.
	- Si cerca di generalizzare la clausola, cioè si cerca una clausola più generica e che massimizzi uno score.
	- Si aggiunge la clausola all'ipotesi $H$ e si rimuovono gli esempi positivi che sono coperti da tale regola.
##### Score
In Aleph lo score è $P-N$, $dove$ P sono gli esempi positivi e $N$ sono quelli negativi coperti dalla clausola.
# Tool
- Versione originale di Oxford - [link](https://www.cs.ox.ac.uk/activities/programinduction/Aleph/aleph.html).
- SWISH con aleph - [link](http://cplint.ml.unife.it/example/aleph/aleph_examples.swinb).
- Libreria python - w.
- popper - [link](https://github.com/logic-and-learning-lab/ilp-experiments).
	È un ILP system costruito su Aleph, ma più potente.
- [NUMSYNTH](NUMSYNTH.md)