Un sistema è definito sulla base di funzionalità, usability, performance, costi e affidabilità. La dependability (i.e., affidabilità) è il fatto riporre fiducia in un sistema in modo giustificato; più il sistema è critico più il livello di affidabilità deve essere elevato.
La dependability si basa su tre aspetti:
- [Attributi](attributi.md)
	Sono detti attributi RAMS e sono le proprietà che devono essere verificate per far si che il nostro sia affidabile. 
- Threats
	In opposizione agli attributi abbiamo i threats, cioè degli eventi indesiderati, ma non inattesi: vorremmo che i threats capitassero mai, ma dobbiamo implementare un sistema che sia affidabile se questi si verificano.
	Prima di arrivare a essere un problema, i threats devono attraversare la [fault-error-failure chain](fault%20error%20failure%20chain.md).
- Means
	Per arrivare a un sistema che verifica gli attributi e quindi a un sistema affidabile, dobbiamo usare contemporaneamente più tecniche.
	- Prevention
		È la tecnica con cui si vuole prevenire l'introduzione di fault durante la progettazione del sistema.
		Si basa su tecniche di [qualità](software%20quality.md): per il software abbiamo ad esempio la modularità dei componenti e mantenere il design semplice anche se il problema è complesso.
		Le proprietà che sono alla base delle scelte di design, comprese le librerie esterne usate, devono essere dimostrate formalmente e non solo testate.
	- [[fault tollerance]]
		Si occupa di mantenere il corretto funzionamento del componente in modo automatico anche in caso di [fault](fault%20error%20failure%20chain.md#Fault%20(guasto/difetto)) attivi.
		Una buona fault tollerance (e prevention) ci permette di avere poca fault removal manuale.
	- Removal
		È la rimozione dei fault.
		Se il sistema è in fase di progettazione allora la fault removal avviene tramite l'identificazione dei fault e una correzione di tali bug trovati, iterando queste due fasi per eliminare anche i fault che si sono introdotti per rimuovere quelli precedenti. La fase di identificazione dei fault può essere fatta in modo statico o dinamico: nel primo casi il sistema non viene eseguito e si usano static code analyzer, model checking o dimostrazioni formali; nel secondo caso si usano test o esecuzione simbolica (i.e., valutazione dei path tramite variabili e non valori reali).
		Se il sistema è già in funzione allora si parla di [manutenzione](attributi.md#Maintenability) e può avvenire in modo predittivo o correttivo, cioè prima o dopo che un guasto si trsaformi un errore. La manutenzione prende circa il 75% dei costi di un sistema.
	- Forecasting
		Vogliamo stimare il numero di guasti, quanto questi sono gravi e quanto sono pericolosi per il sistema.
		Ci sono due modi per fare questa stima:
		- Qualitativa
			Vogliamo identificare e classificare i fault, cercando anche di capire come chi interagisce con il sistema può provocarli.
			I metodi principali sono: FMEA (Failure Mode and Effects Analysis) che è un approccio sistematico per elencare i posisbili guasti e vederne l'effetto; Fault-Trees (Alberi dei Guasti) che è un approccio deduttivo che parte da un failure per arrivare ai fault che lo possono generare. 
		- Quantitativa
			Si valuta in termini di probabilità come gli attributi di dependability siano soddisfatti. Inoltre ci interessa il failure intensity, che solitamente segue la bath curve.
			Ci sono varie tecniche per ottenere questi dati:
			- Modeling
				Si crea un modello che simuli il nostro sistema, come [markov chain](markov%20chain.md) o [Petri Net](Petri%20Net%20(PN).md). In questo caso è fondamentale verificare che le assunzioni siano corrette.
			- Testing
				Si stressa il sistema o un prototipo per raccogliere dati reali. Per far accadere i fallimenti si usa solitamente la fault injection.
# Definizioni
- Servizio
	È il comportamento del sistema che l'utente osserva.
- Function
	È cosa il sistema vuole fare ed è espresso in termini di [specifiche funzionali](requirement%20engineering.md#Tipi).
- Specifiche
	Descrivono cosa il sistema fa (e cosa non va) in termini di servizio atteso e condizioni in cui questo servizio deve verificarsi. Se usciamo dal range coperto dalle specifiche allora non è detto che il sistema funzioni come le specifiche indicano.
- Fallimento
	È la transizione da un servizio corretto a un servizio non corretto, dove la correttezza è definita come il sequire le specifiche.
	Un sistema fallisce perché non rispetta le specifiche o perché le specifiche non rispettano il sistema, cioè perché sono scritte male.
- Rischio
	È la relazione tra la probabilità che un evento accada con la gravità che esso comporta.
# Requisiti
I [requisiti](requirement%20engineering.md) nell'ambito della dependability richiedono di analizzare il componente che stiamo definendo all'interno del sistema di cui fa parte. Ci dobbiamo chiedere infatti quali sono le conseguenze del fallimento del nostro componente all'interno del sistema di cui fa parte.