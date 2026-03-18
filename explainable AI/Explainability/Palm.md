È una tecnica del 1996 di [Global model explanation](Metodi.md#Global%20model%20explanation) per il campo dell'[Explainability](Explainability.md).
# Caratteristiche
- Come explanator usa un albero.
- È un modello agnostico, cioè valido per tutti i tipi di black box.
- Va bene qualunque tipo di dato.
# Algortimo
Si basa sul k-means clustering.
- Si ottengono dei dati dalla balck box facendo inferenza oppure si usano i dati di test della black box.
- Si partizionano i dati in $k$ gruppi.
- Per ogni gruppo di dati si addestra un modello.
	L'idea è sempre di avere un'alta fidelity con la black box. Questi $k$ modelli addestrati possono essere qualunque cosa, anche altre mini black box.
- Ogni elemento di training viene riassegnato al sotto modello che lo predice meglio, cioè che fa un errore più piccolo rispetto alla black box.
- Si itera facendo retraining si questi sotto modelli sui nuovi dati che ora appartengono al gruppo.
- L'output dell'algoritmo è qualcosa del tipo sotto: in base alle features si decide quale sotto modello interrogare. Questo output è molto utile per fare debug di errori nelle predizioni: se un esempio è classificato male so quale sotto modello è responsabile.
	- if ($features_7>\tau$ and $features_3=yes$)
		then $submodel_1$
	- else if ($features_9<\theta$ and $features_4=no)$
		then $submodel_2$
	- $\cdots$