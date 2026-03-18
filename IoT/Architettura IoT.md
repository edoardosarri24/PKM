L'architettura di un sistema sono le caratteristiche e le proprietà fondamentali di un sistema incorporate nei suoi elementi, relazioni e nei suoi principi di design ed evoluzione (definizione ISO).
# WORLD FORUM REFERENCE MODEL
L'architettura si basa su un [modello a 7 livelli](world%20forum%20reference%20model.png). L'obiettivo è quello di guidare o sviluppo e la standardizzazione del dominio IoT.
Vedi [esercizio](wfrm%20exercise.pdf) per collocare i livelli.
### L1-Edge (Physical device and controller)
Appartengono a questo livello tutti i dispositivi fisici per acquisire dati e attuare azioni. Ci possono essere dispositivi semplici come camere e sensori e attuatori e dispositivi più complessi come droni e robot.
### L2-Connectivity
Si occupa delle tecnologie di comunicazione (es: Wi-Fi, BLE, LoRa)
A questo livello appartiene la pila protocollare di internet (TCP/IP), che permette ai dispositivi di collaborare, scambiare informazioni e realizzare le policy di security.
Le informazioni possono passare dai dispositivi che appartengono a L1 alla rete, viceversa, e tra una rete IoT e un'altra.
Siccome le risorse dei dispositivi sono scarse, in mancanza o scarsità di una di queste i dispositivi devono poter utilizzare quelle degli altri. Più che un sistema rete client-server si ha una struttura p2p.
### L3-Edge computing
Dovrebbe essere chiamato fog computing perché è un luogo abbastanza vicino ai dispositivi per limitare la latenza.
È dove avviene l'elaborazione più semplice dei dati, per prepararli per il livello superiore.
Esempi possono essere la decodifica di dati criptati o la valutazione dei dati sulla base di criteri molto specifici.
### L4-Data accumulation
In questo livello i dati ottenuti dai dispositivi (tramite il sistema a eventi) vengono archiviati, trasformati in dati query-based e resi disponibili per le applicazioni quando necessario.
### L5-Data abstraction
I dati vengono elaborati, analizzati e trasformati in informazioni utili.
Alcuni esempi possono essere: l'aggregazione nel caso più dispositivi abbiano generato lo stesso dato; creazione di viste per il filtraggio e la selezione; normalizzazione e indicizzazione.
### L6-Application
È il livello che mette in relazione l'utente finale con il mondo IoT tramite funzionalità utili e applicazioni. 
### L7-Collaboration and processing
Include la gestione della sicurezza e di tutti gli aspetti organizzativi del mondo IoT.
Si può dire che è comporto dalle applicazioni e dalle persone che utilizzano o gestiscono.
# MIDDLEWARE
Si tratta di un componente software distribuito che colma le mancanze sw dei dispositivi IoT (vedi dispositivi di [classe 0](Hardware%20IoT.md#Classe%200) che hanno poco o nulla), che si posiziona tra il livello 4 (trasporto) e 5 (applicazione).
Le sue funzioni sono:
- Astrarre le applicazioni dal dispositivo.
- Garantire l'interoperabilità dei dispositivi eterogenei.
- Serve per la sicurezza, in quanto i dispositvii IoT nascono non sicuri perché sono limitati, e non gli possiamo chiedere di implementare aspetti di sicurezza.
- Fungere da orchestratore nel ciclo di vita dell'informazione (che va dal livello 1 al livello 7); un'operazione del midlleware è un'operaione che coordina e sincronizza i nodi edge (cioè i dispositivi).
### Architettura
Il [middleware](middleware%20architecture.png) si divide in due elementi: application che complementa il livello application, e device che complementa il livello fisico. L'elemento application è più complesso (l'elemento device è uguale ma i sottolivelli hanno meno funzioni) e si divide nei seguenti sotto livelli:
- network
	Gestisce la rete e ordina i task. Per ordinare i task si intende la loro sincronizzazione (es: concorrenza tra lettura e scrittura), che in un contesto distribuito è necessaria.
- service
	Esegue lo scheduling dei task e ottimizza le risorse degli elementi dei device. Inoltre orchestra i vari elementi che compongono l'applciazione.
- semantic
	Fornisce la descrizione dei servizi e delle policy (regole).
##### Security
È un [livello trasversale](middlware%20security.png) a tutti gli altri tre definiti sopra. Si divide in:
- Deployment
	È la sicurezza del livello fisico, cioè dei device e dei loro elementi e delle risorse.
- Comunication
	E la sicurezza del livello 2 della scala [wfrm](Architettura%20IoT.md#L2-Connectivity). Protegge dalle minacce la comunicazione tra dispositivi, soffermandosi sulla sicurezza del canale di comunicazione e sulla gestione dello scambio di chiavi. In questo scenario i gateway sono fondamentali per mettere in connesione due reti che hanno vincoli diversi.
- Service
	Garantisce l'autenticazione, l'autorizzazione e la gestione delle identità.
# WIRELESS (AD-HOC) SENSOR NETWORK
Sono reti ad-hoc, cioè personalizzate per scenari e applicazioni specifiche. 
- Wireless
	È sinonimo di flessibilità. Sono reti basate sulla mobilità e sulla facilità di distribuire un sistema su di esse.
- Sensor
	Capacità di percepire e interagire con il mondo.
- Network
	Si vuole costruire un sistema le cui funzionalità sono basate sulla comunicazione di dispositivi in un'area ampia.
##### Aspetti chiave
- Senza infrastruttura, cioè non sono retei centralizzate. I dispositivi di auto configurano e si auto organizzano.
- Basate sulla cooperazione di dispositivi eterogenei.
- Opportunistiche, cioè dove i collegamenti non sono sempre disponibili e i dispositivi comunicano quando ce n'è la possibilità.
- Auto riparazione che portano a una robustezza e affidabilità.
##### Paradigma data-centric
Si passa da un paradigma addres-centric a uno data-centric: il primo è basato sugli indirizzi della sorgente e della destinazione; il secondo è basato su una corretta diffusione dei dati senza considerare gli indirizzi.
In questo scenario i dati sono inviati in modo time-driven (periodico), event-driven (aperiodico) o tramite request-response (aperiodico).
Un esempio di questo sono le quattro frecce di un auto che non hanno un destinatario ma sono indirizzate a tutti i veicoli retrostanti e hanno un preciso significato.