I [parametri RAMS](attributi.md) (Reliability, Availability, Mantenaibility e Safety) descrivono qualunque sistema e servono per garantire il suo comportamento e ciclo di vita. Si parla di conformità per indicare l'adesione di un sistema a tali parametri, valutata teamite [misure sperimentali](dependability/misure.md#Sperimentale).
# Range in specifica
![conformità in specifica](conformità%20in%20specifica.png)
La conformità di un sistema non è un concetto binario, ma il valore di ogni parametro della specifica deve essere definito da un range:
- Il valore obiettivo è detto valore nominale.
- Al valore nominale si aggiunge una tolleranza definita da un limite superiore e un limite inferiore, all'interno della quale i valore ottenuti sono accettabili. Più questo intervallo è stretto più è costoso rispettarlo: di solito è più stretto per i componenti safety-critical.
- Al di fuori della tolleranza abbiamo prima valori fuori specifica e poi valori che porterebbero a guasto catastrofico.
# Range in verifica
![conformità in verifica](conformità%20in%20verifica.png)
Durante la verifica del sistema non si parla solo di valori ammissibili o non ammissibili, ma viene introdotto anche un range di ambiguità in un intorno dei punti in cui si passa da valore accettabile a valore non accettabile.