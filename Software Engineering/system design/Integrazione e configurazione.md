È un [processo](system%20design.md) per lo sviluppo di sw.
Si basa sul riuso di codice esistente; questo deve essere integrato e configurato nel nostro sistema.
È un paradigma molto utilizzato: la maggior parte dei sistemi sono sviluppati non implementando direttamente librerie ma usandone alcune di terze parti (Un esempio è Python).
##### Conseguenze
- Riduce i costi ([stima dei costi con COCOMO II](COCOMO.md#Riuso)).
- Aumenta la velocità di sviluppo.
- Le librerie esterne devono essere testate: non sappiamo come sono implementate e potrebbero essere fatte male; in futuro potrebbero essere modificate male e introdurre degli errori.
  Esempi di test che dovrebbero essere fatti sono: casi limite, come il passaggio di parametri di tipo non corretto, gestione dell'overflow; gestione dei thread e della memoria, visto che potrebbero non essere deallocati e potare a un overflow.
  Il problema è definire quanto sia giusto testare queste cose; è sempre bene dire che un sistema è sufficientemente testato, senza affermare che non ci siano errori.
- Le librerie esterne potrebbero esporci a problemi di security.
- Più cose si integrano più si perde il controllo di quello che stiamo facendo. Se qualcosa non funziona si deve capire da dove deriva il problema e correggerlo o cambiare approccio.