Un disco di memoria è composto principalmente da un disco magnetico, una testina che legge i dati e un braccio che sposta la testina in modo radiale.
# Struttura
![disco](disco.png)
I dati su un disco sono organizzati in modo gerarchico:
- L'unità più piccola di memorizzazione è il settore. Di solito la dimensione è da 512kB a 4KB.
- Una traccia contiene più settori. Una traccia è praticamente un giro sul disco.
- Se consideriamo più dischi, un cilindro è un inseme di più tracce una sopra l'altra.
# Accesso
Quando si accede a un disco non conviene leggere solo un byte del settore, ma tutto il settore.
Quindi i dati portati in memoria saranno tutti quelli contenuti in un settore.
# RAM
I dischi moderni hanno una ram più o meno grande. Una volta che siamo in un settore, può convenire leggere tutta la traccia e salvarla in ram, nel caso servisse.
Questo segue il concetto di località spaziale e temporale: se un dato è stato accedo ora probabilmente tra un po' servirà ancora; se è stato letto un dato da una certa posizione probabilmente si accederà alle posizioni vicine.