# Definizione
Il repository mining è un processo di estrazione automatica di artefatti software contenuti in repository con l'obiettivo di capire staticamente (senza runnare il codice) come viene sviluppato e manutenuto il software.
##### Artefatti
Quando si parla di artefatti intende qualunque cosa su cui si possa svolgere delle analisi e non sono software.
Ci sono gli artefatti architetturali, cioè documentazione o modelli che descrivono come il sistema è organizzato.
##### Dettagli
- Il livello di dettaglio dell'estrazione può essere più fine o meno a seconda del compito che si deve svolgere. Può essere un singolo commit per uno studio (es: un dato refactoring) o l'intera conologia per capire l'evoluzione del progetto.
- Gli artefatti sono estratti automaticamente (es: analisi static del codice) oppure etichettati manualmente (es: categorizzare commit come fix o features).
# Viewpoint e view
Si parla di viewpoint per descrivere una rappresentazione astratta dell’architettura del software. Esempi: un viewpoint della sicurezza è una policy di accesso; del deployment sono le distribuzioni sul cloud; uno logico è un modulo.
Quando si parla di view si intende una rappresentazione concreta seconda uno viewpoint; in pratica è una view è un viewpoint con i dati.
# Obiettivi
Analizzare gli artefatti è fondamentale per strudiare l'eveluzione del sistema, studiare il costo della sua manutenzione e analizzare l'allineamento tra codice e architettura.
# Analisi
Un processo tipico per capire l'evoluzione di un repository segue dei passi.
- Selezionare un repository.
- Eseguire il checkout di ogni commit con $git\ commit <commit\_id>$. Questo e altre operazioni è possibile farlo anche senza clonare il repository grazei alle GitHub REST API.
- Analizzare metadati o codice del commit con un tool e salvare i risultati, solitamente in un json.
##### Conseguenze
- Abbiamo il pieno controllo sull'analisi, con la possibilitàdi eseguire regolo personalizzate.
- Si possono trovare dati che GitHub non fornisce.
- Richiede un ambiente ben configurato.
- A volte capire il codice può essere complicato.
# Esempi
Vediamo degli esempi di analisi:
- Studiare la produttività degli sviluppatori.
- Studiare l'effetto di pratiche diverse (es: [waterflow](Plan-Driven%20process.md) o [agile](Integrazione%20e%20configurazione.md)).
- Test coverage
	Se vogliamo analizzare la copertura del codice nel tempo possiamo, per ogni commit, analizzare la percentuale di copertura.
- Capire se i bug si concentrano da qualche parte.
# Tool
Vediamo degli esempi di tool:
- [PMD](https://pmd.github.io)
	Si tratta di un analizzatore di codice statico per linguaggi come Java, XML e Apex.
	Ci sono circa 120 regole pronte per definire best practies, code style, design, capire la [complessità](Cognitive%20complexity.pdf) di un metodo, performance, situazioni error prone. Supporta delle soglie personalizzate.