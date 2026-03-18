Metrics è una specifica [MicroProfile](MicroProfile.md) che permette di esporre informazioni sullo stato della nostra applicazione tramite un [endpoint](Endpoint%20Java.md) usando [HTTP](HTTP.md). Le informazioni sono in questo caso valori, a differenza delle informazioni di [health](health.md) che sono binarie.
Un'implementazione di metriche, che non segue le specifiche di MicroProfile ma è più utilizzata, è [MicroMeter](MicroMeter.md).
# Utilizzo
Solitamente le metriche sono raccolte periodicamente in un qualche backend (e.g., [Prometheus](https://prometheus.io)).
Su questo backend è possibile fare query manualmente, oppure farle fare automaticamente a un altro backend che le grafica (e.g., [Grafana](https://grafana.com)).
# Tipi
Ci sono due tipi di metriche:
- Framework
	Sono le metriche generate automaticamente a runtime. Sono generiche e riguardano l'esecuzione dell'applicazione.
	Un esempio sono quelle raccolte da [prometheus](prometheus.md).
- Business
	Sono metriche relative al dominio dell'applicazione stessa.