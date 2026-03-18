La definizione più generica di qualità del sw che possiamo dare è quando esso rispetta i requisiti.
## AZIENDE
![Qualità nelle aziende](QualityInCompanies.png)
All'interno di un'organizzazione chi è responsabile di certificare la qualità del software?
- Ente certificatore
	Gli enti certificatori non sono presenti in tutti i settori (Nel ferroviario si. Nell'aerospaziale no). Dove non c'è, il prodotto viene certificato dall'azienda stessa attraverso l'utilizzo di uno standard.
- Quality assurance team
	È interno all'azienda e ha il compito di aiutare il team a sviluppare il prodotto in modo che questo abbia un buon livello di qualità.
- Team di sviluppo
	All'interno del team di sviluppo c'è il RAMS (Relaiability, Avaibility, Maintainability e Safety) engineer, cioè l'addetto alla qualità. Spesso non possono dare certificazioni perché sono troppo coinvolti e non obiettivi.
- V&V team
	Servono per certificare la qualità di un prodotto. Possono essere interni all'azienda, ma in questo caso sono di un altro dipartimento, o esterni.
# Fondamenti
I requisiti SW definiscono gli attributi di qualità richiesta (ex. Safety, Security, Reliability, Relience, Portability, Efficiency, Testability, ...).
- Cultura e Etica del Software Engineering -> la qualità del software deve far parte della cultura dell'ingegnere del SW; l'etica deve essere presente nell'ingegneria del software per evitare ad esempio discriminazioni ed assicurare privacy ad esempio. 
- Safety -> l'aumento del livello di rischio implica la necessita di aumentare la qualità del software e delle tecniche di controllo. Sono tipicamente regolate da standard (ferrovia, automotive, ...).
# Modelli
I modelli che definiscono la qualità del software sono molti. L'ISO/IEC 9126-1 definisce tre misure qualitative ([grafico](SPQM%20graphics.png)):
- **Interna** ed **esterna**. La prima è la misura del prodotto Software; la seconda è la misura del sistema che contiene il software (Computer System). Si basano su:
	- Funzionalità
	- Affidabilità. Un esempio di misura è la maturità, intesa come la capacità di un prodotto di evitare il fallimento se il SW fallisce. Si può misurare, sia prima del rilascio sia dopo il rilascio, come la quantità di fallimenti in un determinato lasso di tempo.
	 (fault tolerance, recoverability).
	- Usabilità. Un esempio di metrica è comprensibilità, cioè la capacità di un software di rendere comprensibile l'esecuzione dei task.
	- Efficienza
	- Manutenibilità
	- Portabilità
- Dell'utente (Quality in use -> misura attributi di un sistema esterno, Business System, che contiene il Computer System)
	Feedback del cliente. Si basa su:
	- Efficacia
	- Efficienza (Nel senso di produttività)
	- Safety
	- Soddisfazione

Le varie misure sono collegate. È praticamente impossibile avere misure interne ed esterne ottime e pessimi riscontri dall'utente.
# Software quality manager process
- SW Quality Assurance
	Non è il testing. Un piano di SW quality definisce le attività e i task da completare per assicurare che il SW sviluppato soddisfi i requisiti stabiliti dal progetto.
- Verification & Validation
	Validation assicura che il prodotto sviluppato soddisfi lo scopo per cui è stato costruito; Verification assicura che il prodotto sia costruito correttamente, nel senso che gli output siano quelli che avevamo imposto nelle precedenti attività (abbiamo costruito il giuto sistema).
	- esempi: risk analysis, Simulation, **Dinamic** Analysis and Testing.
- Reviews & audits (revisioni e controlli) -> esame **statico** degli artifatti SW:
	- Walkthroughs (valutano un prodotto SW)
	 Sono dei feedback che hanno lo scopo di migliorare e insegnare qualcosa. Solitamente provengono dall'interno e non sono formali.
	- Inspections (identificano anomalie nel prodotto SW)
	 Hanno l'obiettivo di trovare degli errori nel SW. Provengono di solito dall'esterno e si basano su regole e standard. Serve una preparazione molto adeguata.