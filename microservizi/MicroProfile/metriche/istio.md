Un service mesh è un infrastructure layer che permette alle applicazioni a [microservizi](microservizi.md) di aggiungere funzionalità come stardard di sicurezza, [metriche](metrics.md), gestione del traffico e telemetrie, senza modifica al codice. Istio è il più popolare service mesh, fondato da Google.
# Funzionamento
Istio è definito da:
- data plane
	È un insieme di proxies (intermediario, componente che sta in mezzo) che controllano e gestiscono lo scambio di dati tra i vari micro servizi. Ci sono due alternative:
	- Sidecar
		Si tratta di un container che sta vicino a ogni pod del cluster. Istio utilizza [Envoy](https://www.envoyproxy.io) come proxy, che lavora a livello 5. Ha il vantaggio di una fine-grain sul controlla; ha gli svantaggi di aggiungere over head e della gestione dei proxy.
	- Ambient
		È la modalità più nuova e in fase di sviluppo. Si aggiunge un layer di rete per ogni pod del cluster, in cui possiamo mettere le funzionalità che ci servono. Ha il vantaggio di non dover far sidecar injection e inoltre non si ha over head.
- control plane
	Gestisce e configura i proxies del data plane.
# Funzionalità
Istio permette di implementare numerose funzionalità. Oltre a quelle approfondite abbiamo:
- Sicurezza
- Gestione del traffico
- Logs
##### Metriche
le metriche intese da istio sono diverse rispetto a quelle di business che possiamo definire con [MicroMeter](MicroMeter.md). Anche in questo caso le metriche solitamente vengono raccolte da [prometheus](prometheus.md) e mostrate su [grafana](grafana.md).
Le metriche si dividono in metriche proxy e metriche service. In generale quelle che si osservano con Istio sono metriche di rete e vengono raccolte osservando quello che entra e che esce da un pod.
- Latenza, cioè differenza tra tempo di arrivo e di risposta di una richiesta.
- Errori
- Saturazione di una replica di un pod.
- Volume di traffico.
##### Tracciabilità
Si tratta della sequenza di chiamate che avvengono in un'applicazione a micro servizi.
Le metriche vengono raccolte con il formato stardard [OTLP](OpenTelemetry.md#Stardard). In questo modo è possibile utilizzare tutti i backend che supportano questo standard, come [Jaeger](Jaeger).