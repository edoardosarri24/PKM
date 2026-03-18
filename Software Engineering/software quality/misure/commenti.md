Ci sono molti studi che dimostrano come commenti migliori aiutino il rilevamento di bug, la comprensione del codice e in generale la manutenibilità del programma; in sostanza quella he è l'undestandability del codice. Il rpoblema principale è che questo concetto è molto correlato con le capacità di chi legge codice.
Per valutare la qualità dei commenti ci sono molti [attributi](Software%20Engineering/software%20quality/misure/misure.md#Attributi), confusionari e con pochi tool a supporto.
# Studi
Si trovano abbastanza lavori che trattano della qualità dei commenti. Non c'è ancora uno standard per questo tipo di valutazione, ma molte misure in overlapping con pochi tool.
La maggior parte dei paper trattano codice Java. La maggior parte del codice analizzato tratta code comment; gli altri due tipi di commenti più valutati sono i commenti a metodi e quelli alle API.
- [a decade of code comment quality assessment - a systematic literature review](a%20decade%20of%20code%20comment%20quality%20assessment%20-%20a%20systematic%20literature%20review.pdf) - base per torvare aricoli
- [quality analysis of source code comments](quality%20analysis%20of%20source%20code%20comments.pdf)
	Mette in relazione pertinenza del javadoc con il codice tramite tecniche di machine learning.
- Ci molti lavori che descrivono l'idea di qulche tool, ma nulla è open source.
# Attributi
Gli attributi più studiati sono:
- Consistency/Coherence/up-to-dateness
	Valuta la sovrapposizione tra i termini del commento e codice relativo.
	In [questo lavoro](improving%20code%20readability%20models%20with%20textual%20features.pdf) vengono proposte 7 metriche per valutare la consistenza e la readability tra codice e commenti. Sarebbe interessante vedere come implementarne qualcuna forse?
	In [questo lavoro](a%20topic%20modeling%20approach%20to%20evaluate%20the%20comments%20consistency%20to%20source%20code.pdf) invece si propone un metodo complesso basato sull'estrazione dei topic e sull'analisi della loro coerenza tra metodi e commenti.
- Completeness
	Indica il grado di copertura del codice relativamente ai commenti.
	[Questo lavoro](CIDRe%20-%20A%20Reference-Free%20Multi-Aspect%20Criterion%20for%20Code%20Comment%20Quality%20Measurement.pdf) definisce la copertura in un modo interessante. Da capire se implementabile.
- Readability
	Gli sviluppatori passano molto tempo a leggere codice e documentazione; avere commenti leggibili aiuta a capire più velocemente cioè che stiamo leggendo.
	La metrica più utilizzata legata a questo attributo è Flesch-Kincaid Grade Level.
- Commented-Out (CO) code
	Si tratta di codice commentato. I problemi sono molteplici: il codice commentato non viene manutenuto; per archiviare codice si dovrebbero usare tool di versioning del codice e non i commenti; chi legge il codice potrebbe avere dubbi sulla sua utilità.
	In [questo lavoro](the%20secret%20life%20of%20Commented-Out%20source%20code.pdf) sono stati valutati i risultati con tool che rilevano CO code (anche in base alla versione del codice), ma non ci sono tool online disponibili. In [questo lavoro](Minerando%20Codigo%20Comentado.pdf) viene presentato un parser per il rilevamento di CC code in Java, ma anche qua non ci sono implementazioni.
- Complessità spaziale
	Si tratta in pratica di verificando la distanza tra la definizione di un metodo e la sua chiamata.
	È definita in [questo paper](Code%20and%20data%20spatial%20complexity%20-%20two%20important%20software%20understandability%20measures.pdf), ma direi che è poco interessante.
# Valutazione
Le valutazioni degli attributi sono fatte con [tecniche](evaluation%20techniques.png) diverse:
- Manualmente
	È la tecnica più usata. Gli sviluppatori o chi per loro si mettono li e correlano una misura agli attributi.
- ML/DL
	Un approccio molto utilizzato è relativo alle tecniche di machine learning o deep learning.
- Heuristic-based
	Sono le tecniche basate su regole. Il problema è che spesso le regole sono definite per un dato linguaggio o una data situazione e quindi sono difficili da generalizzare.