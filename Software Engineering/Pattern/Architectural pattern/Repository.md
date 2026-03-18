![Repository](Repository.png)
Lo scambio di informazioni da parte di più sottosistemi può essere fatto in due modi: condividere i dati in un database centrale che viene acceduto da tutti; ogni sottosistema ha il suo database e le informazioni vengono scambiate tra i sottosistemi, che restano indipendenti.
- Il pattern repository si basa sul primo concetto. C'è una struttura centrale che mette a disposizioni operazioni CRUD (Create, Read, Update, Delete). Questa struttura non ha molta logica; un eventuale business logic deve essere implementata nei sottosistemi.
- Nel pattern repository non c'è un orchestratore, cioè colui che permette una sincronizzazione tra i sotto sistemi. Lo si può aggiungere, ma si deve violare l'indipendenza totale dei sottosistemi; l'orchestratore è l'unico che la deve violare.
## Applicabilità
- Ci sono grandi quantità di dati che devono essere condivise tra più sottosistemi.
- Si vuole implementare un sistema data-driven.
## Conseguenze
- Vantaggi
	- I sottosistemi sono indipendenti e non devono essere a conoscenza l'uno dell'altro.
	- Cambiamenti a un sottosistema non influiscono sugli atri.
	- I dati sono consistenti in ogni momento.
- Svantaggi
	- Il repository è un single point of failure.
	- Inefficiente organizzare la comunicazione tra i sottosistemi a causa dell'orchestratore.