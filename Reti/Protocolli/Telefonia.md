È la rete che consente la trasmissione di segnali fonici tra due utenti remoti. Vista la grandezza della rete per scalare nelle dimensioni si usa una gerarchia:
- Centro urbano. Gestisce un' area geografica limitata e non sono previsti collegamenti diretti tra pari livello.
- Centro di settore. Gestisce una città e non sono previsti collegamenti diretti tra pari livello.
- Centro di distretto. Sono previsti collegamenti tra pari livello se l'area è molto densa.
- Centro di compartimento. Gestisce una regione e sono previsti collegamenti diretti e completi tra pari livello.
- Centrali nazionali. Gestisce una nazione e sono previsti collegamenti diretti e completi tra pari livello.
## TELEFONIA ANALOGICA
- È stata la prima soluzione proposta.
- La condivisione del canale si realizza con la tecnica [FDM](Multiplexing.md#FDM).
- Il problema deriva dalla condivisione dello stesso canale fisico da più utenti: le interferenze su un canale FDM provengono dai canali FDM vicini.
## TELEFONIA NUMERICA/DIGITALE
- Applicando il [Teorema del campionamento](Reti%20di%20telecomunicazioni.md#Teorema%20del%20campionamento) ad un segnale fonico continuo si ottiene un insieme discreto di valori reali; usando poi la quantizzazione si traduce questo insieme un intero. Con la telefonia digitale si trasmette il segnale fonico rappresentato da 1 byte.
- La condivisione del canale si realizza con la tecnica [TDM](Multiplexing.md#TDM). Il tempo di trama nella telefonia digitale è di 125$\micro$s.
- I vantaggi sono:
	- Universalità. Qualunque sia il tipo dell'informazione si trasmette sempre 1 byte.
	- Codifica. Si può codificare direttamente i byte rasmessi, senza trasformare il segnale.
	- Interferenza. Durante la sua trasmissione il segnale viene aplificato dai dispositivi di rete. Nella telefonia analogica si amplifica sia il rumore sia l’informazione; nella telefonia digitale si può, scegliendo bene la distanza tra una tappa e l’altra, eliminare il rumore, in quanto si deve solo discriminare un valore di polarità (Capire se ho ricevuto uno 0 o un 1).
## DATI
La rete telefonica è usata anche per la trasmissione di dati. Per questo scopo si usano le tecniche [dial up](Reti%20di%20telecomunicazioni.md#Dial%20up) definite come xDSL (Digital Subscriber Line).
Le più diffuse sono:
- ADLS (DSL asimmetrica). L’utente ha una diversa capacità di accesso, cioè una diversa quantità banda, per il download e per l’upload; questo avviene perché ci si aspetta che la domanda sia meno pesante della risposta.
	- Il dispositivo principale, presente sia nei router di casa che nelle centrali, è lo splitter, composto un filtro passa basso e uno passa alto: il primo serve per eliminare le alte frequenze, che tipicamente trasmettono i dati; il secondo elimina le basse frequenze, che trasportano inveve il segnale fonico.
	- Nella centrale lo splitter divide il segnale fonico da quello dati: il primo viene mandato normalmente alla rete telefonica; il secondo, prima di essere mandato al provider internet, raggiunge un altro dispositivo detto DSLAM, che implementa la multiplazione aggregando insieme più flussi.
- SDSL. È la versione simmetrica dell'ADSL. Ha le sue stesse caratterische, con la differenza di allocare la stessa quantità di banda sia per il download che per l'upload.
- HDSL. Esiste sia la versione simmetrica che asimmetrica. Ha la caratteristica di avere un collegamento dedicato per ogni utente; in questo modo si raggiongono velocità di trasferimento molto elvate.