Un fallimento può essere di più tipi:
- Interni
	Il fallimento consiste nella risposta sbagliata.
- Esterni
	Qualcosa di esterno al servizio stesso compromette la sua disponibilità.
- Performance
	La velocità di risposta scende sotto un certo livello soglia.
##### Manifest failure
I problemi che sono visibili agli utenti, detti manifest failures, si possono risolvere parzialmente con un Watchdog times (che deve funzionare sempre).
È un times che parte quando la richiesta viene lanciata; se il tempo supera un valore limite si segnala che il servizio potrebbe non essere raggiungibile; dopo una serie di iterazioni si dichiara il servizio come irraggiungibile.
Questo porta al problema che la chiamata a un servizio raggiungibile ma non funzionante può causare un rallentamento del servizio chiamante, portando a una cascata di perdita di performance. Per questo motivo si usa un circuit breaker: è un servizio dedicato al rilevamento dei fallimenti basato su timer.
