Gli [algoritmi con tabella](algoritmi%20con%20tabella.md) e [algoritmi senza tabella](algoritmi%20senza%20tabella.md) sono algoritmi di routing unicast, cioè con sorgente e destinazione unica.
Le applicazioni dell’instradamento multiplo possono essere l’accesso a database distribuiti, la diffusione delle informazioni e le teleconferenze.
# Broadcast
È la situazione in cui il pacchetto generato da una sorgente deve essere inviato a tutti i partecipanti della rete. In ordine crescente di efficienza si può:
- Si realizza effettuano tanti trasferimenti unicast quanti sono i nodi della rete. Si ha lo svantaggio di avere tante copie del pacchetto quanti sono i nodi.
- Il multidestination routing prevede l’invio di un solo pacchetto, il cui campo di indirizzo è una lista di indirizzi. Quando il pacchetto arriva primo router viene duplicato tante volte quante sono le porte di uscita; nel campo indirizzo di ogni pacchetto duplicato vengono inseriti solo gli indirizzi raggiungibili da quel percorso. In questo modo il campo di indirizzo si riduce sempre di più, fino ad arrivare ad essere composto solo dall’indirizzo del destinatario. Gli svantaggi sono: il nodo sorgente deve conoscere gli indirizzi di tutti i partecipanti alla rete; i tempi di elaborazione all’interno di ogni router sono elevati.
- Flooding.
- Nel RPF (Reverse Path Forwarding) il pacchetto viene inoltrato su tutte le uscite del router solo se non sono arrivate prima delle sue copie (Il router deve mantenere una lista di pacchetti arrivati).
- Con lo spanning tree, con radice il nodo sorgente, si riesce ad avere tutti percorsi ottimi. Il problema sta nella conoscenza dello spanning tree di tutti i nodi della rete; è infatti una soluzione percorribile solo se la rete implementa l’algoritmo Link State. Quando un nodo vuole essere aggiunto allo spanning tree lo si fa cercando il cammino più breve che vada dalla radice al nodo da aggiungere; l’operazione di rimozione di un nodo dalla rete è detta potatura.
# Multicast
È la situazione in cui il pacchetto generato da una sorgente deve essere inviato a più nodi della rete. Vediamo dire modi in cui si possono attuare collegamenti multicast:
- Multicast
	Il gruppo dei destinatari è identificato da un singolo indirizzo; quando arriva in un router questo si occuperà di duplicarlo e trasmetterlo sulle uscite corrette. In questo modo un collegamento non si può trovare più copie dello stesso pacchetto una dopo l’altra.
- Unicast multiplo
	La sorgente invia nella rete lo stesso pacchetto tante volte quante sono le destinazioni da raggiungere. In questo modo un collegamento si può trovare più copie dello stesso pacchetto una dopo l’altra.