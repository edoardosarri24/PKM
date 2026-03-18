È la versione asincrona dell'[[observer]].
Tra publisher e subscriber c'è un intermediario, detto event channel o event bus. Il publisher invia tipi di messaggi al broker; i subscriber si iscrivono ai tipi di messaggi di interesse; il broker dovrà inviare i messaggi ai subscriber in modo appropriato.
## CONSEGUENZE
- Il publisher e i subscriber sono completamente scorrelati.
- Il pattern è asincrono: il publisher una volta inviati i messaggi può continuare a svolgere i propri compiti, senza aspettare l'aggiornamento di ogni subscriber.