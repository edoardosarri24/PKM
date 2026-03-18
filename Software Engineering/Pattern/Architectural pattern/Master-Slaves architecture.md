Pattern architetturale usato nei [sistemi distribuiti](sistemi%20distribuiti.md) real-time (Situazione dove i task che devono essere eseguiti entro una dead-line. Non vuol dire che devono essere eseguiti velocemente) dove più processori devono acquisire dati dall'ambiente, elaborarli e attuarli.

- Master
	È responsabile della computazione dei dati e della coordinazione e comunicazione con gli slaves.
- Slaves
	Sono dedicati a specifiche azioni, come l'acquisizione di dati.