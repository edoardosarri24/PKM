OpenTelemetry, noto anche come OTel, è un insieme di tool e API, usate anche da [MicroProfile](MicroProfile.md), che permette di collezionare ed esportare telemetrie da un'applicazione cloud native. Oltre alle telemetrie è usabile anche per metriche e logs, ma in questo non è stabile e si preferisce usare le specifiche di [MicroMeter](MicroMeter.md).
# Stardard
Il vantaggio di avere OpenTelemtry non è tanto la cattura delle tracce (cosa che viene fatta anche da [istio](istio.md) ad esempio), quanto la definizione di uno standard per esportare tali metriche raccolte: OTLP.
# Pipeline
Dopo aver raccolto le metriche esse possono essere gestite in due modi:
- Mandate a un Collector. In questo modo sono memorizzate e possono essere accessibili a più backend  (e.g., [Jaeger](Jaeger)). In grandi applicazioni che devono scalare questo è fondamentale.
- Mandate direttamente al backend. In questo caso sono memorizzate sulla ram del backend del backend stesso. In piccole applicazioni questo è forse consigliato per la facilità di configurazione e gestione.
# Quarkus
Per utilizzare OTel nelle nostre applicazioni [quarkus](quarkus.md) si deve fare una singola cosa: mettere l'estensione $quarkus-opentelemetry$ nel pom.
In questo modo Quarkus inizia a collezionare telemetrie in automatico.