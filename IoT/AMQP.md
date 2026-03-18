L'AMQP (Advanced Message Queque Protocol) è uno stardard proposto dall'OASIS.
È un protocollo asincrono, basato su TCP e complementare ad HTTP. Vuole contrapporsi al [publish/subscribe](publish-subscribe.md#PUBLISHER-SUBSCRIBER), nel senso che è un'applicazione del concetto di [message queue](publish-subscribe.md#MESSAGING%20QUEUE).
# IDEA
Il publisher manda un messaggio al broker in una coda; questa non è un topic ma è una coda general purpose, senza quindi un preciso scopo. Questi messaggi sono tenuti nella coda finché non arriva un client (o consumer) che li preleva; a questo punto il broker può cancellarli.
Per ampliare questo modello i messaggi possono essere marcati con un routing key, cioè un topic; i messaggi comunque vengono mandati sempre ad una coda generica.
# CONSEGUENZE
- Il fatto che il messaggio, in un implementazione che rispetta quanto detto sopra, sia preso solo da un consumer è un aspetto molto vincolante.
- Non si dice niente sulla situazione in cui un subscriber arriva dopo la creazione della coda.
##### Somiglianza con [MQTT](MQTT.md)
- Sono entrambi protocolli asincroni e basati sul [message system](publish-subscribe.md).
- Si basano su TCP.
##### Differenze con [MQTT](MQTT.md)
- In MQTT un messaggio è mandato a una sola coda, che però è specifica per un topic; in AMQP i messaggi possono essere mandati a molti code, però queste sono tutte general purpose.