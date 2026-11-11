[Health](https://microprofile.io/specifications/microprofile-health/) è una specifica [MicroProfile](MicroProfile.md) che permette di esporre informazioni sullo stato della nostra applicazione tramite un [endpoint](Endpoint%20Java.md) usando [HTTP](HTTP.md).
Le informazioni in questione sono informazioni binarie, cioè ci dicono se l'applicazione sta bene o sta male; se vogliamo un valore allora la specifica MicroProfile che ci serve è [metrics](metrics.md).
# Kubernetes
L'utilizzo standard delle metriche di health è far consumare gli endpoint che espongono queste metriche da vari tool.
Il più classico è associare Health a Kubernetes. Periodicamente il [service](elementi.md#Service) di Kubernetes valuta le metriche di salute e prende una decisione su come comportarsi relativamente alla replica di un servizio: può decidere se intradare il traffico verso un container oppure un altro.
##### Visualizzazione Quarkus
In [quarkus](quarkus.md) ci sono due tecniche per vedere lo status del check nella dev mode:
- Dalla dev UI.
- Chiamando tramite HTTP $localhost:porta/q/health$.
# Check
SmallRye mette a disposizione vari tipi di check sulla salute dell'applicazione e questi in [quarkus](quarkus.md) sono resi disponibili nell'[estensione realtiva](estensioni.md#Health). Ogni tipo di check ha la sua omonima annotazione.
Questi controlli devono essere configurati: in [quarkus](quarkus.md) devono essere inseriti nel file di [configurazione](configurazioni.md) $application.properties$.
- Liveness
	Se la liveness fallisce vuol dire che l'applicazione è in uno stato di errore dal quale non può uscire. A esempio è entrata in un loop infinito, in uno stato di deadlock oppure è avvenuto un memory leak, oppure non riesce a stabile la connesione con altro pod per qualche motivo.
	Kubernetes valuta questa metrica periodicamente e se da un risultato negativo, di solito per più volte consecutivamente, allora fa ripartire il pod.
- Readiness
	Se la readiness fallisce vuol dire che l'applicazione non è momentaneamente pronta. A esmepio non è connessa con il database, oppure sta svolge un numero di task che supera un valore soglia.
	Kubernetes di solito valuta questa metrica per decidere se mandare le future richieste per quel servizio a un'altra replica.
- Startup
	È una metrica valutata solo quando il container parte: quando esso è pronto a ricevere il traffico allora la metrica ha successo.
- Wellness
	Valuta potenziali problemi dell'applicazione, ma che ancora non sono così gravi come nel caso della liveness o della readiness.
	Solitamente non è utilizzata da tool come Kubernetes, ma sono informazioni utili per gli ingegneri.
- Custom
	Si possono creare metriche binarie (di Health) personalizzate semplicemente creando una classe annotata con uno dei precedenti tipi, che implementa l'interfaccia $HalthCheck$ e facendo l'override del metodo $call$.