È il Message Queque Telemetry Transport (MQTT), un protocollo leggero, open source, basato su TCP e sul modello [publish/subscribe](publish-subscribe.md#PUBLISHER-SUBSCRIBER) per ambienti con vincoli (device/network constrained environment). Implementa l'application layer.
# TOPIC
I messaggi del publisher sono inviati al broker e sono filtrati tramite topic (una URI, cioè una risorsa).
I topic, inseriti nel campo $path$ delle URI, hanno una forma gerarchia separata dallo slash: $topic/subtopic/subsubtopic$. Un subscriber si può iscrivere a un topic specifico, iscrivendosi a tutta la gerarchia, oppure usare le wildcard:
- Il + indica che si vuole iscriversi a tutto quello che è presente in quella posizione, cioè a quel livello della gerarchia (es: $topic/+/subsubtopic$)
- Il # indica che ci si vuole iscrivere a tutto quello che viene dopo il cancelletto (es: topic/#).
# MESSAGGI
Il controllo degli elementi dell'header viene fatto tramite bit (in modo binario) e non confrontando stringhe; in questo modo risulta un protocollo leggero. Stesso discorso viene fatto per il payload: visto che il contenuto è specifico per un'applicazione il contenuto può essere binario.
![messaggi MQTT](messaggi%20MQTT.jpeg)
Un messaggio è composto da 2 byte di header, una parte di header variabile e opzionale e un payload opzionale.
- [Message type](MQTT%20message%20type.jpeg) (4 bit)
	Notiamo che il messaggio ACK non è generico, ma ogni tipo di messaggio ha il relativo ACK.
- DUP (1 bit)
	Indica se il messaggio è già stato inviato ed è nuovamente inviato perchè non si è ricevuto un ACK. Se posto a 0 indica che è il primo invio; altrimenti un insieme successivo.
- QoS (2 bit)
	Indica il livello di sicurezza per la consegna del messaggio su base E2E, cioè la consegna al subscriber. I valori sono:
	- 0
		Indica "al più una volta" e una soluzione best effort. Se il messaggio arriva bene, altrimenti non si invia nuovamente. Non richiede un messaggio ACK.
	- 1
		Indica "almeno una volta", ma volendo anche di più (quindi si possono avere duplicati).
		Necessità di due messaggi: il publisher manda un PUBLISH e aspetta un PUBACK. Se non riceve il PUBACK entro un determinato lasso di tempo allora invia nuovamente il PUBLISH con DUP a 1; altrimenti il publisher cancella il messaggio.
	- 2
		Indica "esattamente una volta" ed è la condizione più restrittiva. La catena di messaggi è [questa](QoS%20type%202%20MQTT.jpeg).
- Retain (1 bit)
	Indica se il broker deve salvare il mesaggio in modo permanente. 