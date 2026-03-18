È un linguaggio indipendente dal tipo di database SQL utilizzato, che permette di utilizzare una sintassi più vicina a Java che rispetto a SQL.
Si riferisce all'entità Java e non alle relazioni di SQL.
- [esempio codice JPQL](esempio%20codice%20JPQL.pdf).
# Join
L'operazione di join in JPQL è logicamente la stessa dell'algebra relazionale. JPQL mette a disposizione tre modi:
- Associazione implicita.
	Dopo la keyword $WHERE$ si usa un $=$. Non richiede nessuna ottimizazione: è la soluzione più semplice ma anche quella meno efficace. Un esempio è [questo](associazione%20implicità%20JPQL.png).
- Associazione implicita
	Si usa esplicitamente il join. Si possono usare anche più condizioni. Un esempio è [questo](associazione%20esplicita%20JPQL.png).
# Inheritance
Siccome [JPA](JPA.md) permette l'ineritance allora anche le query devono farlo.
Solitamente questo si fa direttamente. Si specifica la sotto classe in cui si vuole ricercare per una ricerca più spicifica. Altrimenti si specifica la supercalsse per ricercare in tutte le sottoclassi.