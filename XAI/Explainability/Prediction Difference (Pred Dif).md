Prediction Difference (Pred Dif) è una tecnica del 2007 per l'[outcam explaination](Metodi.md#Outcam%20explaination) nel campo dell'[Explainability](Explainability.md).
- In questo caso non si usa un modello interpretabile come spiegazione. Si valuta l'importanza della features in base alla variazione dell'output della black box.
- È model agnostic, cioè valido per tutti i tipi di black box.
- Va bene qualunque tipo di dato. La difficoltà per dati non tabulari (quelli per cui lo vedremo noi) è capire come eliminare una feature.
# Idea
Per capire la black box si valuta l'importanza delle varie features.
Per capire l'importanza della feature $A_i$ nell'output dell'esempio $x$, cioè quanto $A_i$ contribuisce all'output, si deve rimuovere $A_i$ e valutare il cambiamento della predizione.
# Algoritmo
L'algoritmo si basa su $predDiff_i(x)=\displaystyle f(x)-f(x-A_i)$, dove $x-A_i$ è l'istanza $x$ a cui abbiamo eliminato la feature $A_i$, $f$ è la black box e $x$ è l'istanza da spiegare.
Per valutare la variazione dell'output del modello dove aver eliminato una features possiamo usare una probabilità: confrontiamo le due probabilità $p(y=c|x)$ e $p(y=c|x-A_i)$, dove $c$ è una delle possibili classi (supponiamo che la black box sia un classificatore). In modo alternativo possiamo anche considerare i logaritmi di queste probabilità, ma il risultato non cambia.
##### Eliminare features
Il problema è capire come eliminare la feature $A_i$ dall'esempio $x$ in modo che la black box $f$ sia sempre la stessa: non vogliamo infatti dover rimuovere una feature e poi riaddestrare il modello perché non sarebbe più lo stesso.
PredDiff sostituisce la feature $A_i$ con una specie di valore medio, in modo da forzare il modello a comportarsi come se non la conoscesse. Questo viene fatta tramite $p(y=c|x-A_i)=\displaystyle\sum_{s=1}^{m_i}p(A_i=a_s)\cdot p(y|x\leftarrow A_i=a_s)$, dove:
- $m_i$ è il numero di possibili valori assunti dalla features $A_i$ all'interno del train set.
-  $P(A_i=a_s)$ è la probabilità marginale di $a_s$ e rappresenta la frequenza con cui compare il valore $a_s$ nel train set.
- $x\leftarrow A_i=a_s$ indica l'esempio $x$ modificato in modo che la feature $A_i$ assuma valore $a_s$.
- $p(y|x\leftarrow A_i=a_s)$ è la predizione del modello per il nuovo esempio $x$ modificato.
- $p(y|x-A_i)$ quindi rappresenta la media delle prestazioni del modello per l'esempio $x$ nel caso in cui $A_i$ assuma a turno tutti i valori possibili pesati per la loro frequenza nel train set.
# Conseguenze
- Vantaggio
	È model agnostic.
- Svantaggi:
	- Non si considera l'interazione tra le features.
	- La perturbazione delle features potrebbe portare all'[OOD](Incremental%20feature%20deletion.md#Out%20of%20distribution), creando dati non reali.
	- Ci serve il train set.