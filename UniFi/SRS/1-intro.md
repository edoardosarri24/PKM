La [dependability](dependability.md) (da non sapere come funziona il tollerance, removal e forcasting) si può definire (IEC 60300-1) come il fatto riporre fiducia in un sistema in modo giustificato.
I parametri RAMS (Reliability, Availability, Mantenaibility e Safety) descrivono qualunque sistema e servono per garantire il suo comportamento e ciclo di vita.
# Conformità
È l'adesione dei parametri funzionali di un prodotto alle sue specifiche.
##### Range in specifica
![conformità in specifica](conformità%20in%20specifica.png)
Non si tratta di un concetto binario, ma il valore di ogni componente della specifica deve avere un range:
- Il valore obiettivo della caratteristica che vogliamo raggiungere è detto valore nominale.
- Al valore nominale si aggiunge una tolleranza definita da un limite superiore e un limite inferiore, all'interno della quale i valore ottenuti sono accettabili. Più questo intervallo è stretto più è costoso rispettarlo: di solito si stringe per i componenti safety-critical.
- Al di fuori della tolleranza abbiamo prima valori fuori specifica e poi valori che porterebbero a guasto catastrofico.
##### Range in verifica
![conformità in verifica](conformità%20in%20verifica.png)
Durante la verifica del prodotto non si parla solo di valori entro la tolleranza e fuori tolleranza, ma si introduce anche un intervallo di ambiguità intorno al limite inferiore e superiore della tolleranza.
# Gerarchia IEC/CT56
È lo standard per gestire la dependability di un sistema.
Tale standard è costituito da varie norme (regole), le quali si citano costruendo una gerarchia con l'idea che quelle inferiori servano ad applicare in modo pratico quelle superiori.
- Principali
	Le norme di gestione e vocabolario stabiliscono il linguaggio comunque a tutte le altre leggi.
- Processo
	Definiscono cosa fare durante le varie fasi del ciclo di vita del prodotto.
- Supporto
	Definiscono i dettagli tecnici e le metodologie per calcolare i vari parametri RAMS.