Lo sviluppo model-driven è basato su modelli. Questi non sono usati solo per generare codice, ma supportano lo sviluppo per tutto il suo ciclo di vita.
Il loro scopo principale è limitare lo sforzo cognitivo: dovrebbero semplificare il processo di sviluppo e quindi abbassare i costi.
Lo standard più usato è [UML](UML.md).
## LIVELLI
![[Model-driven engineering.png]]
- Il livello 0 è il nostro sistema, cioè quello effettivamente implementato.
- Il livello 1 è rappresnetato da modelli.
- Il livello 2 è rappresentato da meta-modelli.
- Il livello 3 è rappresentato da meta-meta modelli.
## Meta-modelli
Un esempio è [UML](UML.md).
- Un meta-modello è un modello di un modello, cioè un'astrazione di più alto livello.
- È un'ontologia, cioè contiene gli elementi e le possibili relazioni tra essi. Definisce la struttura, le relazioni e i vincoli delle entità di un particolare dominio.
- Definisce la sintassi che una sua istanza deve avere.
## Modello
Un modello è un'astrazione utile per uno scopo specifico e con un livello di dettaglio coerente con esso. È quindi un'astrazione (Cioè una semplificazione) di quello che vogliamo rappresentare; ogni livello di astrazione e ogni dominio ha il suo modello specifico, più o meno dettagliato in base alla situazione.
- Un modello è un'istanza di un meta-modello, cioè è costruito con elementi appartenenti al meta-modello. A seconda della nostra applicazione si usano gli elementi del meta-modello in un modo diverso.
- Il modello è conforme al meta-modello quando rispetta le regole e la sintassi in esso definite.
## Meta-meta modelli
- Si può passare da un modello a un altro con della perdita di informazione. Questo è fatto attraverso il mapping, cioè usando elementi che sono comuni a tutti i meta-modelli. Gli elementi comuni a tutti i modelli appartengono al meta-metamodello.
- Il meta-metamodelli sono conformi a se stessi e non hanno un ulteriore livello di astrazione. Questo perché a partire da un meta-metamodello si può, tramite la sua sintassi, costruire ricorsivamente tutti i modelli possibili.