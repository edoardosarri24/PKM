La complessità ciclomatica è una misura formulata nel 1976 per valutare quanto un codice fosse testabile e manutenibile. Il linguaggio di programmazione su cui si basa è Fortlan.
Si tratta di uno score che misura il numero di percorsi lineari indipendenti che esistono in un programma.
Il numero magico, trovato empiricamente, è più o meno 10/12.
- [a complexity measure](a%20complexity%20measure.pdf)
# Problematiche
##### Understandability
La complessità ciclomatica è un'ottima misura per quanto riguarda la complessità di testing, ma non è valida per quanto rigurda la manutenbilità e l'undestandability.
Infatti codici che hanno la stessa complessità ciclomatica non necessariamente sono ugualmente difficili da manutenere. Per questo secondo caso la metrica che ci serve è infatti la [cognitive complexity](cognitive%20complexity.md).
##### Linguaggi moderni
La complessità ciclomatica è stata definita nel 1976 e non tiene conto delle possibili strutture dei moderni linguaggi di programmazione.
# Funzionamento
La complessità ciclomatica si basa sul numero ciclomatico di un grafo. Dato un grafo $G$ il suo numero di clomatico è $V(G)=e-n+2p$, dove: $e$ è il numero di archi, $n$ è il numero di nodi e $p$ è il numero di componenti connesse.
##### Control flow graph
Capiamo che ci serve un grafo basato sul codice per capire la sua complessità ciclomatica. Il grafo che funge da astrazione matematica è il [control flow graph (CFG)](Testing.md#Control%20flow%20graph%20(CFG)).
# Testing
Possiamo usare la complessità ciclomatica per capire se abbiamo testato tutti i percorsi che il flusso può prendere.
Se infatti abbiamo una complessità ciclomatica $v=V(G)$ e attualmente abbiamo testato $ac$ (actual complexity), abbiamo più possibilità:
- Ci mancano da testare alcuni percorsi.
- Il grafo $G$ può essere ridotto in complessià di $v-ac$.
- Il codice può essere ridotto in complessità ciclomatica.