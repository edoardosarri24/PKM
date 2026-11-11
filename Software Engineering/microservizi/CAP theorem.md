La condivisione dei dati è un aspetto fondamentale e motlo difficile all'interno dei [microservizi](microservizi.md). Ogni micro servizio dovrebbe avere il proprio database, ma un po' di condivisione, come all'interno dei micro servizi duplicati (utili per far fronte a grandi quantità di richieste), è consentita e necessaria.
# Teorema
Il CAP theorem ci dice che in un sistema basato su una rete soggetta a fallimenti di comunicazione, è impossibile realizzare operazioni atomiche di lettura e scrittura su una memoria condivisa che garantiscano la risposta a ogni richiesta.
##### Spiegazione
È un teorema che si basa sul compromesso tra safety e liveness. Per un sistema distribuito che condivide una memoria è impossibile soddisfare contemporaneamente queste tre proprietà:
- Consistency
	È una proprietà di safety: in una memoria condivisa e replicata i dati sono gli stessi in ogni momento.
	Nei micro servizi si parla di "evenutally consistent", cioè i due database prima o poi saranno consistenti (Spesso si risolve con il logging: quando la connessione è offline si logga gli accessi per poi ripristinare la consistenza).
- Availability
	È una proprietà liveness: ogni aggiornamento sulla memoria può essere portato a buon fine.
- Partiotioning
	Ogni parte isolata di un sistema deve funzionare anche dopo un guasto.