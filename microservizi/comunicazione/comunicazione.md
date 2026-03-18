In un'applicazione a [microservizi](microservizi.md) spesso più microservizi interagiscono tra loro per arrivare al risultato di una singola richiesta di un client. Per questo motivo serve definire un meccanismo di IPC (Inter Process Comunication).
# Stili
Gli stili di comunicazione sono indipendenti dalla tecnologia che usiamo per implementare questo stile.
![stili](stili.png)
Ci sono due dimensioni in cui possiamo categorizzare i meccanismi di IPC. In base ai partecipanti si può avere one2one o one2many; in base al sincronismo può essere sincrono o [asincrono](asynchronous%20messaging.md).
- Request-Respose
	Il client fa una request al server e si blocca in attesa della risposta. Si usa quando i servizi sono molto accoppiati.
- Asynchronous Request-Respose
	Il client invia un request al server e non si blocca mentre aspetta una response asincrona.
- One-way notification
	Il client invia una notifica al server e non si aspetta una risposta.
- Publish/Subscribe
	Il client invia un messaggio che viene consumato da zero o molti servizi.
- Publish/async resposes
	Il client invia un messaggio e attente una risposta dai servizi che lo consumano per un certo intervallo di tempo.
# Formati messaggi
Siccome l'IPC è fondamentale un metodo per scambiare messaggi, è fondamentale definire il formato di questi messaggi. Questa scelta comporta l'efficienza dell'IPC, l'usabilità delle API che esponiamo e l'evolvability.
Ci sono solitamente due formati per i messaggi: text e binary.
##### Text
Gli esempi più usati sono JSON e XML.
- Sono formati human-readable e autodescrittivi. Chi riceve il messaggio può filtrarlo sulla base degli attributi che descrivono i valori.
- Introducano un overhead sia per la quantità di dati spediti che per il tempo di elaborazione a causa degli attributi che descrivono i valori.
##### Binary
Gli esempi più usati sono ProtoBuf e Avro.
Hanno le conseguenze opposte.




Tralasciando la tipologia SOAP, che non è più utilizzata, ci sono due famiglie principali di tipi di comunicazione:
- La più famosa nota e conosciuta è la classica sincrona request/response, implementata esponendo servizi [REST](REST.md) e utilizzando un qualche protocollo o framework (e.g. [graphQL](https://graphql.org)).
- La modalità più complessa è quella relativa all'event-based comunicazione. Si basa sul modello [publish-subscribe](publish-subscribe.md) e permette una sincronizzazione asincrona sia tra coppie di micro servizio che tra gruppi. In questo ambito c'è il framework [rRPC](https://grpc.io) di Google che permette di gestire anche stream di dati infiniti.