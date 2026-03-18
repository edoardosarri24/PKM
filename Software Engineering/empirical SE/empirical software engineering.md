L'empirica SE è l'uso di dati quantitativi (es: metriche misurabili come LOC, tempi o bug) e qualitativi (es: esperienze e interviste) per capire e migliorare i prodotti e i processi di sviluppo software. In questo contesto sono importati i concetti di [empirismo e razionalismo](empirical%20vs%20rationalist.md).
Come nella scienza gli esperimento empirici si basano sull'osservazione, sulla formulazione di un'ipotesi e sulla sua verificazione tramite esperimenti.
##### Validità
Quando si fa uno studio empirico è fondamentale accertarsi della validità di ciò che stiamo facendo. Ci possono essere vari tipi di validità:
- Internal
	Se abbiamo misurato davvero ciò che ci interessa.
- External
	Se possiamo applicare i risultati solo al notro caso di studio o anche in altri contesti.
- Construct
	Se le metriche usate rappresentano davvero l'oggetto dello studio.
- Conclusion
	Se i risultati sono statistici risultati sono statisticamente affidabili.
##### Luoghi comuni
Valutare in modo quantitativo e qualitativo i prodotti e i processi permette di evitare l'uso di luoghi comuni inutili nel nostro contesto: se una cosa è sempre stata fatta così, non vuol dire che vada bene in una data situazione.
Le decisioni devono essere supportati da fatti coerenti con il contesto attuale.
# Metodi
Per svolgere esperimenti empirici ci sono vari metodi che possiamo combinare:
##### Lab vs wild
- Lab
	Vogliamo eseguire uno studio in un ambiente artificiale. Questo aiuta a eliminare del possibile rumore per porre maggiormente l'attenzione sulla relazione tra le varibili indipendenti e dipendenti. L'obiettivo è quello di controllare le variabili e ottenere replicabilità.
	Un esempio è osservare la produttività di uno svulippatore sullo stesso task svolto su due IDE diversi.
- Wild
	Vogliamo eseguire uno studio in un ambiente reale. L'obiettivo è quello di osservare comportamenti reali senza avere controllo su quello che stiamo studiando.
	Un esempio è osservare un team agile per un certo tempo e poi analizzare la qualità degli sprint e degli incotri giornalieri.
##### Qualitative vs quantitative
- Qualitative
	Negli studi si usano prevalentemente dati testuali o derivati. L'obiettivo è quello di comprendere il contesto e la qualità delle scelte.
	Un esempio è fare delle interview agli sviluppatori per capire come vivono il passaggio dal modello waterfall ad agile.
- Quantitative
	Negli studi si usano dati numerici e analizzabili statisticamente. L'obiettivo è quello di misurare e confrontare relazioni causali, cioè relazioni tra una causa-effetto.
	Un esempio è osservare il numero di bug prima e dopo aver fatto una code review.
##### Inductive vs deductive
- Inductive
	È l'approccio bottom-up: prima si osserva e poi si creano ipotesi. A partire da questi si creano modelli.
- Deductive
	È l'approccio top-down: si parte da una teoria e la si verifica con dati.
# Esperimento
Un esperimento empirico è un esperimento che manipola una o più variabili (dette anche fattori) indipendenti per osservarne gli effetti sulle variabili dipendenti. Si deve: identificare le variabili che influenzano l'oggetto di sutdio; capire le relazioni causa-effetto; fondare teorie e leggi.
Un esperimento solitamente è eseguito in laboratorio in modo da limitare le variabili che portano del rumore; questo permette di poter mettere maggiormente in relazione le variabili indipendenti con quelle dipendenti.
##### Definizioni
Un esperimento è deifnito dalle tre componenti sottostanti:
- Soggetto
	È colui o coloro che eseguono l'esperimento.
	Ad esempio uno sviluppatore un architetto o anche un utente.
- Oggetto
	È l'entità su cui eseguire l'esperimento. Devono essere rappresentativi del dominio di interesse, vari per rappresentare tutte le possibili condizioni reali. Se un oggetto è troppo accademico si rischia di non poter generalizzare l'esperimento alla vita reale.
	Ad esempio potrebbe essere un'applicazione software, oppure un insieme di repository di github.
- Trial
	È l'azione eseguita per eseguire l'esperimento, cioè è cosa fa il soggetto all'oggetto. L'esecuzione di un trial su un oggetto è detto experimental run.
	Si pala di full factorial design quando si applicano tutti i trial a tutti gli oggetti; in questo modo si minimizza il bias e si mettono in relazione tutte le possibilità. Se il numero di oggetti e trial è troppo grande invece se ne applica solo una parte a un sotto insieme di oggetti e allora si parla Randomized Block Design.
##### Flow
Ci sono vari livelli e astrazioni in cui possiamo vedere un espirimento:
- Conceptial
	Si deve definire l'obiettivo: si parte da un'idea di esperimento, si definisce lo scope, cioè il campo in cui l'esperimento deve essere valido e infine arriviamo all'obiettivo.
- Operational
	Si esegue l'esperimento
- Quantitative
	Si analizzano i dati raccolti per capire se questi sono significativi o casuali.
	Le tecniche più comuni di analisi sono:
	- Verifica della normalità (es: Shapiro-Wilk test)
		Si verifica che i dati siano distribuiti normalmente perché molti test statistici (es: t-test) partono da questa assunzione. In caso non sia verificata allora si deve svolgere test di altri tipi.
	- T-Test
		Si vuole confrontare le media dei due gruppi
		Ad esempio i tempi di compressione medi di due algoritmi di compressione
	- Spearman’s rank correlation
		Si vuole misurare la relazione monotona tra una variabile indipendente e una dipendente.
		Ad esempio la relazione tra la dimensione dell'immagine e il tempo di compressione.
##### Research question
Vediamo come formulare le domande della ricerca e come queste sono in relazione all'analisi e ai risultati di un esperimento.
L'idea è quella di suddividere l'obiettivo principale dell'sperimento in sotto problemi, cioè in domande più specifiche e gestibili. Ogni domanda avrà un esperimento mirato e specifico, in modo da ottenere risposte più specifiche e mirate che non confondono variabili indipendenti e dipendenti.
Ogni domanda dovrebbe avere due tipi di ipotesi:
- Ipotesi nulla (H0)
	Afferma che le differenze che saranno osservate con l'esperimento non sono significative, cioè che i vari oggetti degli esperimenti non differiscono secondo la domande in questione.
- Alternativa (Ha)
	Afferma che c'è una sostanziale differenza tra i vari oggetti sui si pone la domanda.