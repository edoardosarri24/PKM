È un pattern architetturale usato nei [sistemi distribuiti](sistemi%20distribuiti.md).
È basato sul modello [Client-Server](Client-Server.md), ma in questo caso non si fa distinzione tra client e server: ogni componente offre e ricevi servizi dagli e agli altri.
## Middleware
Un ruolo fondamentale è giocato dal [middleware](sistemi%20distribuiti.md#Middleware%20e%20interazioni): fa comunicare tra loro i vari componenti; permette l'integrazione (Aggiunta), la rimozione e la modifica di un componente in modo trasparente per gli altri.
## Conseguenze
- Permette di posticipare la decisione della definizione dei ruoli.
- Permette di aggiungere nuove risorse al sistema in base alle necessità, cioè di riconfigurarlo. Questo vuol dire che il sistema è flessibile e scalabile.
- La complessità maggiore è la standardizzazione del [middleware](sistemi%20distribuiti.md#Middleware%20e%20interazioni). Microsoft e Oracle ad esempio sviluppano tipi incompatibili. Per questo motivo sono nati e hanno preso posto le architetture basate sui [servizi](Service%20Oriented%20Architecture%20(SOA).md).