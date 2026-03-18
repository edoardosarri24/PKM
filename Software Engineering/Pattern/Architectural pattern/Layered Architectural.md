In un'architettura a livelli si suddivide il sistema in più livelli. Ognuno è indipendente dagli altri e fornisce servizi ai layer adiacenti tramite interfacce.
## Applicabilità
- Si devono costruire strutture sopra sistemi già esistenti.
- Quando si vuole implementare una sicurezza multi livello: per penetrare il sistema si deve superare la sicurezza di ogni livello.
## Conseguenze
- Vantaggi
	- Si possono modificare o sostituire interi livelli purché l'interfaccia resti la stessa.
	- Un servizio può essere ridondato in livelli diversi per aumentare l'affidabilità e la sicurezza.
	- Se si sviluppa in team si può separare bene il lavoro.
- Svantaggi
	- Definire i limiti di ogni layer può essere praticamente molto complesso. Spesso infatti ci sono parti comuni (In un PC ogni livello non è strettamente a se stante).
	- Le performance possono diminuire perché le richieste devono solitamente passare per ogni layer.
## Esempi
- Costruzione di un PC. Il layer più basso è HW, poi ci sono i drive, poi c'è il sistema operativo, poi il middleware e poi le applicazioni.
- Sistemi SW: il livello più basso è il sistema operativo o il database, poi la business logic, poi le autenticazioni o autorizzazioni e infine la UI.