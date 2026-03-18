![Model-view controller](Model-view%20controller.png)
## Componenti
- Model
	È il responsabile dei dati e delle operazioni su di essi.
- View
	È responsabile della presentazione dei dati agli utenti.
- Controller
	Gestisce l'interazione tra sistema e utente. Riceve input dall'utente e li passa nel modo corretto agli altri componenti.
## Usabilità
- Si usa quando ci sono più modi o di presentare i dati e di eseguire richieste su di essi.
- Non sappiamo in che modo questi aspetti si possano evolvere.
## Conseguenze
- Il vantaggio è la possibilità di cambiare dati, modalità di rappresentazione e interazione sistema-utente l'uno indipendentemente dall'altro.
- Lo svantaggio è l'aggiunta di complessità all'architettura del sistema quando in realtà il sistema è semplice.
# Correlazioni
È spesso correlato con il pattern [observer](observer.md). Una volta che i dati sono cambiati il Model fa una $notify$ a tutti i suoi osservatori.