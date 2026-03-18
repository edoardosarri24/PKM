Una famiglia di funzioni hash è in generale un insieme di funzioni hash che identifica in modo efficiente le coppie di elementi che sono probabilmente simili.
##### Caratteristiche
Una famiglia di funzioni hash deve rispettare alcune proprietà:
- Deve essere più probabile che la funzione hash di due elementi sia uguale più questi elementi sono vicini.
- Le funzioni hash devono essere indipendenti.
- Deve identificare le coppie simili in modo efficiente. Un esempio nell'[LSH](Locality%20sensitive%20hashing.md#Locality%20Sensitive%20Hashing) è mettere nello stessa cella del vettore relativo a una banda due coppie simili.
- Deve permettere la combinazione di funzioni hash in modo efficiente. Un esempio nell'[LSH](Locality%20sensitive%20hashing.md#Locality%20Sensitive%20Hashing) è l'utilizzo dell'hash generale.
# Sensibilità
Una famiglia di funzioni hash è detta ($d_1$, $d_2$, $p_1$, $p_2$)-sensitive se $\forall h\in H$, $x,y\in S$ si ha che:
- Se $d_1(x,y)<d_1 \Rightarrow Pr[h(x)=h(y)]>p_1$.
- Se $d_2(x,y)>d_2\Rightarrow Pr[h(x)=h(y)]<p_2$.
Un esempio è usando la distanza di [Jaccard](Distanza.md#Jaccard) con cui si ha che ($d_1$, $d_2$, $(1-d_1)$, $(1-d_2)$)-sensitive
##### Ampliamento
Possiamo ampliare il concetto della sensibilità mettendo in AND e in OR più funzioni hash. In questo modo la nuova funzione hash darà come risultato $h(x)=h(y)$ se abbiamo $h_i(x)=h_i(y),\forall i\in\{1,\cdots,r\}$.
- Con la combinazione in $AND$, scegliendo in modo opportuno $r$ si può portare $p_2$ a essere molto vicino a 0.
- Con la combinazione in $OR$, scegliendo in modo opportuno $r$ si può portare $p_1$ a essere molto vicino a 1.



- **Sensibilità (d1, d2, p1, p2):** Una famiglia H di funzioni hash è definita **(d1, d2, p1, p2)-sensibile** in relazione a uno spazio S di punti con una misura di distanza `d`. Questo significa che per due punti qualsiasi `x` e `y` in S:
    - Se la distanza `d(x, y)` è **inferiore a d1**, allora la probabilità che `h(x) = h(y)` (ovvero che la funzione hash li consideri uguali) è **almeno p1**.
    - Se la distanza `d(x, y)` è **superiore a d2**, allora la probabilità che `h(x) = h(y)` è **al massimo p2**.
    - Non ci sono garanzie per le coppie con distanza tra d1 e d2, il che potrebbe portare a una frazione sconosciuta di falsi positivi in quell'intervallo.
- **Esempi di famiglie di funzioni hash sensibili alla località:**
    - **Minhash per la somiglianza di Jaccard:** La famiglia di funzioni minhash è un esempio specifico di una famiglia (d1, d2, (1 − d1), (1 − d2))-sensibile per la somiglianza di Jaccard. Ogni funzione minhash è basata su una delle possibili permutazioni delle righe di una matrice caratteristica. Min-Hashing serve a convertire grandi insiemi in "firme" corte, mantenendo la somiglianza, e utilizza l'hashing per generare queste firme con la proprietà `Pr[hω(C1) = hω(C2)] = sim(C1, C2)`.
    - **Iperpiani casuali per la distanza del coseno:** Per la distanza del coseno, la tecnica degli iperpiani casuali genera una famiglia (d1, d2, (1 − d1/180), (1 − d2/180))-sensibile. Una funzione hash `hv` è determinata da un vettore casuale `v` e restituisce `+1` se `v · x > 0` e `-1` se `v · x < 0`. La probabilità che `h(x) = h(y)` è `1 - (angolo tra x e y diviso 180)`. Questi risultati formano una "firma" (o sketch) di +1 e -1 che può essere usata per LSH in modo simile alle firme minhash per la distanza di Jaccard.
- **Amplificazione di una famiglia LS (AND e OR):** È possibile combinare le funzioni all'interno di una famiglia per migliorarne le proprietà:
    - **Costruzione AND:** Data una famiglia H (d1, d2, p1, p2)-sensibile, si può costruire una nuova famiglia H' combinando `r` funzioni indipendenti di H (`h1, ..., hr`). La nuova funzione `h(x) = h(y)` solo se `hi(x) = hi(y)` per tutte le `i` da 1 a `r`. Questo è simile all'effetto di `r` righe in una singola banda. La famiglia risultante H' sarà (d1, d2, (p1)r, (p2)r)-sensibile, il che **abbassa tutte le probabilità**. Scegliendo H e `r` in modo opportuno, `p2` può essere reso prossimo a 0.
    - **Costruzione OR:** Similmente, si può costruire H' combinando `b` funzioni indipendenti di H (`h1, ..., hb`). La nuova funzione `h(x) = h(y)` se `hi(x) = hi(y)` per una o più `i`. Questo è come combinare diverse bande. La famiglia risultante H' sarà (d1, d2, 1 − (1 − p1)b, 1 − (1 − p2)b)-sensibile, il che **aumenta tutte le probabilità**. Scegliendo H e `b` in modo opportuno, `p1` può essere reso prossimo a 1.
    - **Cascata AND- e OR-:** È possibile combinare in cascata le costruzioni AND e OR per rendere la probabilità bassa vicina a 0 e la probabilità alta vicina a 1, ottenendo così una discriminazione più netta tra coppie simili e non simili. Tuttavia, l'uso di più costruzioni e valori più alti per `r` e `b` **aumenta il costo computazionale**.

In sintesi, le famiglie di funzioni hash sono insiemi di funzioni progettate per preservare la somiglianza tra elementi, permettendo di identificare efficientemente coppie simili attraverso tecniche come l'hashing sensibile alla località (LSH).