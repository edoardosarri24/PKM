La progettazione di un sistema non è automatizzabile e possiamo solo dare delle linee guida che forniscono al designer una metodologia, dei tool e una check list di quello che è da fare.
# Fasi
Il processo di system designsegue vari step.
##### Requisiti
Stilare i [requisiti](requirement%20engineering.md) è la parte che si fa con il cliente. Si deve definire il sistema da sviluppare e definire quali sono le priorità.
##### Design
È forse la parte più complessa e infatti è guidata da [standard](open%20distributed%20processing.md): più specialisti lavora insieme per documentare tutto quello che deve essere fatto. Sia lavora sulla parte di business, cioè quella che distingue l'applicazione e sulla parte non funzionale.
È importante cercare di documentare tutto in questa fase, in modo da prevedere tutti i possibili errori, visto che risolverli qua è molto meno costoso.
##### Development
Si implementa quanto definito.
##### Testing
Si passa ai test, cioè si deve verificare che il sistema rispetti i requisiti definiti.
- Unit testing
	Si testa la singola funzionalità.
- Integration testing
	Si testa il modulo quando viene chiamato dall'esteno, cioè l'API del modulo.
- System testing
	Si testa il systema completo.
- User acceptance
	Si verifica che l'utente sia soddisfatto.
##### Validation and Verification
- Validation
	È un processo esterno, cioè si cerca l'approvazione del cliente. Ci chiediamo se abbiamo costruito il prodotto giusto.
- Verification
	È un processo interno, cioè si eseguono i test di sistema, controllando che tutto il SW, ed eventualmente l'HW, messo insieme funzioni correttamente. Ci chiediamo se abbiamo costruito il prodotto giustamente.
##### Deployment and maintenaince
Il SW viene pubblicato e migliorato via via; si implementano nuove funzionalità in base alle richieste del cliente o del mercato.
È la parte in cui si passa il 70% del tempo.
# Meta modelli
- [[Plan-Driven process]]
- [[Sviluppo incrementale]]
- [[Integrazione e configurazione]]