La gestione delle [memorie](memoria.md) avviene sulla base del modello con cui questa è implementata.
# RAM model
Il Random Access Machine è stato il primo modello, utile per i computer sequenziali. Oggi questo modello è ampiamente superato.
- Una sola CPU esegue con memoria illimitata che contiene il programma (i.e., le istruzioni) e i dati.
- Tempo di accesso unitario per ogni istruzione.
# PRAM model
Il Parallel Random Access Machine è il modello nato per il parallelismo, dove più CPU accedono alla stessa memoria illimitata.
- Non si può rappresentare il concetto per cui le istruzioni accedono alla memoria in tempo costante perché la possono trovare occupata oppure una parte può essere più vicina rispetto a un'altra.
- È un modello che porta a una non corretta valutazione della complessità degli algoritmi.
# CTA model
Il Candidate Type Architecture è il modello usato per la computazione parallela.
La memoria è suddivisa in livelli: quella più vicina è più veloce, mentre quella più lontana più lenta, fino ad arrivare alla memoria centrale.