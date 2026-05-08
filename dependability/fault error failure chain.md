La fault-error-failure chain è quella pipeline che i threats nella [dependability](dependability.md) devono attraversare per diventare un problema.
Se il nostro sistema è visto come una sequenza di layer gerarchici, allora un fault di un componente può portare al suo failur che può essere la causa di un fault in un componente di più alto livello.
# Fault (guasto/difetto)
Un fault è un guasto o un difetto, ed è definito come la causa potenziale di un error: se il guasto non si è verificato allora si parla di dormiente, altrimenti di attivo.
Ci sono vari tipi di guasti:
- Involontario o accidentale.
- Interno o esterno
	Rispettivamente se è causato da un componente del sistema o da un input errato.
- HW o SW
	In caso di SW si parla di bug. SOno circa il 65% dei guasti dei sistemi; solo il 10% sono hw.
- Permanente  o transitori
	Rispettivamente se ogni volta che il sistema si trova in uno stato allora fallisce o se l'errore è causato da eventi che non si presentano nuovamente.
# Error
L'errore in un componente è dato dall'attivazione di un fault che lo ha portato in uno stato errato; se lo guardiamo da un punto di vista sw è quando una variabile memorizza un valore non corretto.
Se questo errore non esce all'esterno del componente allora non abbiamo un fallimento.
# Failure
Si presenta quando un errore viene trasmetto all'interfaccia e altera il sistema, cioè quando si ha una deviazione delle funzionalità corrette che il sistema deve fornire. È quindi il problema che vediamo all'esterno del programma se osserviamo questo come una black box.
##### Classificazione
I fallimenti si suddividono in base a:
- Dominio
	- Content
		Risposte sbagliate.
	- Timing
		Il sistema risponde prima o dopo di quanto avrebbe dovuto.
	- Halt
		Il sistema non risponde. È il fallimento più facile da rilevare.
	- Erratic
		Risposte a volte sbagliate a volte giuste.
- Detectability
	Il componente che fallisce potrebbe sollevare il motivo per cui è fallito oppure no. Nel primo caso è più facile agire su di esso e rimuovere il problema e inoltre possiamo fidarci maggiormente del sistema.
- Consistenza
	Il fallimento può essere sempre lo stesso oppure può essere diverso ogni volta. Se è sempre lo stesso è più facile da rilevare.
- Conseguenza
	Può portare da conseguenze minore (e.g., app che si blocca) fino a conseguenze catastrofiche (e.g., aereo che casca).