Quando si parla di generare carico un applicazione di tipo [SOA](Service%20Oriented%20Architecture%20(SOA).md) si intende semplicemente avere un qualche tool (e.g., K6) che permette di fare un ciclo $for$ chiamare delle API che il servizio espone.
In questo modo si può testare la nostra applicazione in uno scenario reale simulando molti VU (Virtual Users).
# K6
Il funzionamento di [K6](https://k6.io) è il sequente.
- Si crea uno script JS che definisce le richeste HTTP da fare.
- Si configura il numero di VU e la durata del test.
- SI ottiene un report di metriche, come tempi di risposta, errori, throughput.