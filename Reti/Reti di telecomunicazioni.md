---
Professore: Romano Fantacci
Esame: 2023-07-12
Voto: 30
---
# PRINCIPALI
[Telefonia](Telefonia.md)
[[ISO OSI]]
[TCP IP](TCP%20IP.md)
[[IEEE 802 - LAN]]
[Reti geografiche](Reti%20geografiche.md)
[[Routing]]
[[Congestione reti]]
## DEFINIZIONI
##### Telecomunicazioni
Indica la capacità di più individui o più dispositivi remoti (Non vicini) di scambiarsi informazioni attraverso collegamenti cablati o non.
##### Rete di telecomunicazioni
È l’insieme di collegamenti che consentono lo scambio di informazioni.
##### Dial up
Una tecnica è definita dial-up se questa usa un mezzo fisico che è nato per uno scopo diverso.
##### Commutazione
Modalità con cui i dati vengono inviati nella rete. [Link](Trasmissione.md)
##### Store&Forward
Un dispositivo che deve trasmettere dei dati prima li memorizza tutti e poi li propaga in un'unica soluzione. È un tipo di inoltro più lento ma più affidabile. La soluzione opposta è il [Cut-through](Reti%20di%20telecomunicazioni.md#Cut-through).
##### Cut-through
Un dispositivo che deve trasmettere dei dati inoltra bit a bit appena questi vengono ricevuti. È un tipo di inoltro più veloce ma più soggetto ad errori. La soluzione opposta è il [Store&Forward](Reti%20di%20telecomunicazioni.md#Store&Forward).
##### Broadcast storm
È l’eccessivo numero di pacchetti uguali che circola nella rete. Porta di solito al suo congestionamento.
##### Full duplex
Collegamento sorgente-destinazione bidirezionale
## TEOREMI
##### Teorema del campionamento
Se ho un segnale con banda limita nell'intervallo \[0; B]KHz, posso dire di conoscere esattamente il segnale se lo considero formato dai valori che esso assume in istanti di tempo non più distanti di $T\le\tfrac{1}{2B}$.