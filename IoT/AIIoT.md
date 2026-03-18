Lo scopo dell'AI nell'Iot deve essere quello di controllare un sistema e i vari dispositivi per renderli ancora più intelligenti e autonomi.
Si potrebbero controllare sensori e attuatore in modo da modificare loro il comportamento rispetto alla situazione che si viene a creare. Ad esempio si può modificare il tempo di campionamento di un sensore della temperatura al variare della situazione, come un incendio o una situazione di rischio.
### Relazione
Dobbiamo domandarci a cosa serve l'IoT nell'AI e a cosa serve l'AI nell'IoT:
- IoT nell'AI
	Il funzionamento del ML si basa sulla presenza di una grande quantità di dati. Il mondo dell'IoT, e in particolare i livelli più bassi dell'architettura a 7 livelli del [WFRM](Architettura%20IoT.md#WORLD%20FORUM%20REFERENCE%20MODEL), i dispositivi sono una fonte di dati incredibilmente grande.
- AI nell'IoT
	Nell'architettura a 7 livelli del [WFRM](Architettura%20IoT.md#WORLD%20FORUM%20REFERENCE%20MODEL) il ML possiamo metterlo al livello 6 e 7, quando le applicazioni sono basate su di esso.
	Può comunque essere collocato anche al livello 4 e 5 quando si parla di analisi dei dati (quindi il livello 5, data abstraction).
### Computing
Addestramento e previsioni del ML possono stare in posizioni diverse.
- Molto più vicino al cloud quanto più si ha bisogno dell'intero contesto.
- Il più vicino possibile al dispositivo (fog computing) per diminuire la latenza.
- Sia nel cloud che nel fog con il continuos computing: si tratta di elaborare in continuità tra cloud e fog un'informazione. Per il ML si potrebbe fare l'addestramento nel cloud e la previsione nel fog (il più vicino possibile all'edge).
### Federated learning
Si tratta di un modello di ML distribuito basato sul modello client-server. L'idea è quella che i vari client inviano a un cloud dei modelli, il cloud esegue delle esecuzioni e produce un meta modello sulla base del quale, dopo averlo ricevuto, i client eseguono le proprie previsioni; in questo modo il meta modello è il risultato dell'addestramento di più modelli.
Un esempio è quello l'idea di Google sulla previsione della parola inserita: i modelli sono le informazioni degli utenti (da dove stanno scrivendo, se sono mancini o destri, che tipi di errori fanno); il meta modello è l'elaborazione da parte del cloud Google necessaria poi ai dispositivi per eseguire una predizione.
- Osserviamo che in questo modo non si scambiano mai dati grezzi, ma solo modelli. Questo aumento da un lato la privacy dell'utente, che rimane sul dispositivo, dall'altro si riduce la quantità di dati scambiati.