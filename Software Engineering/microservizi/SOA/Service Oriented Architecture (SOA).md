Negli anni '90 le informazioni che le società pubblicavano sul web erano accessibili sono da browser e non potevano essere utilizzate da altre applicazioni. Sono quindi nati i servizi web, che permettono alle applicazioni di accedere e aggiornare le [risorse](REST.md#Risorse) presenti sul web.
##### Servizio web
Un servizio web può essere definito come un componente sw riutilizzabile, debolmente accoppiato, che può essere distribuito e che viene acceduto da applicazioni.
Un servizio web espone al mondo un'interfaccia che specifica come questo possa essere utilizzato; non espone nessuna interfaccia in ingresso, cioè non è definito sulla base dei servizi richiesti (come succede per i componenti sw).
Quando i servizi diventano molto piccoli, con singole responsabilità allora si parla di [microservizi](microservizi.md).
# Conseguenze
Un [sistema distribuito](sistemi%20distribuiti.md) basato sui servizi ha numero vantaggi:
- I servizi che lo compongono possono essere implementati internamente oppure da aziende esterne.
- Se si utilizzano servizi esterni si paga solo quello che si utilizza. Non si deve più comprare tutto un sw complesso, ma solo il servizio necessario.
- Se ci sono calcoli complessi questi sono delegati al servizio, mentre l'applicazione può restare piccola e snella.
- Si possono utilizzare più fornitori di servizi, cambiando le chiamate ai vari servizi anche a run time.
# Implementazioni
Le prime implementazioni dei servizi si basavano su uni scambio di messaggi tra servizi in formato XML. Questa era una tecnica molto pesante in quanto richiedeva un'elaborazione per creare, leggere e modificare i file XML trasmessi.
L'alternativa studiata fu quella di utilizzare uno stile architetturale diversio, basato su REST. Un'applicazione dove ogni cos è una risorsa REST è detta [RESTful](REST.md#RESTful).
# Service process engineering
Implementare un servizio web è abbastanza complesso: si deve astrarre dal caso d'uso attuale e creare un servizio che possa essere riutilizzato in più scenari e in applicazioni diverse. Il servizio deve essere infine molto ben documentato in modo che chi lo vuole utilizzare sappia come fare.
Per creare dei servizi ci sono dei passi da seguire.
##### Identificazione
L'output di questa prima fase è un elenco dei servizi indipendenti e riutilizzabili, con i relativi requisiti funzionali e non funzionali: i primi indicano cosa il servizio deve fare; i secondi riguardano ad esempio la sicurezza e le prestazioni.
Per identificare i servizi di cui potremmo aver bisogno ci sono varie categorie in cui possiamo definire un servizio:
- Utilità
	Implementano funzionalità generali, che possono essere utilizzate a supporto di altre.
- Aziendali
	Implementano funzionalità specifiche per l'azienda.
- Coordinamento o processo
	Implementano funzionalità che supportano un processo aziendale più complesso.
- Orientati ai compiti o alle entità
	I primi sono relativi a una qualche attività.
	I secondi sono relativi a una qualche risorsa dell'azienda.
##### Interfaccia
Per ogni servizio individuato si deve adesso progettare l'interfaccia.
L'output di questa fase sarà una documentazione o delle API che permettono di capire:
- Le operazioni supportate
- Gli input
- Gli output
- Il tipo di interfaccia
	Si deve infatti chiarire se si vuole utilizzare [SOAP e WSDL](SOAP%20e%20WSDL.md) oppure [REST](REST.md).
- Le eccezioni che possono essere sollevate
	Questo è un cosa molto importante. Si dovrebbe supporre che gli utenti del nostro servizio possano sbagliare a chiamrlo in qualunque modo possibile.
##### Implementazione
Adesso i servizi devono essere implementati in un qualunque linguaggio, volendo anche tutti linguaggi diversi. Dopo aver creato il nostro servizio questo va testato con ogni combinazione possibile degli input.
In alternativa un servizio può essere composto da altri servizi già implementati da altri. In questo caso si lavora tramite un workflow, cioè un flusso (disegnato con [UML](UML.md)) ordinato di attività: per ogni attività si cerca di capire quali servizi già esistano e come possono essere impiegati nel nostro flusso; se per una qualche attività non esiste un servizio già definito, o si cambia il workflow oppure si implementa in autonomia.