In un'applicazione a [microservizi](microservizi.md) il tracciamento è un aspetto cruciale. Il compito viene fatto solitamente sequendo la specifica [Telemetry](MicroProfile.md#Specifiche) di MicroProfile. Qusta specifica è di fatto la specifica e l'implemetazione di [OpenTelemetry](OpenTelemetry.md), che è lo standard de facto per il tracing; un'altra soluzione molto usata e in divenire è [istio](istio.md).
# Motivazioni
In applicazioni con migliaia di micro servizi seguire la cascata di chiamate è molto complesso, così come capire dove si è interrotto il flusso di chimate all'interno di una cascata molto complessa.
Tramite il tracing non solo possiamo vedere la catena i chiamate, ma anche i tempi con cui ogni richiesta è stata servita, sia E2E che per la singola richiesta.
# Funzionamento
Il funzionamento solitamente è abbastanza semplice. Durante la cascata di chiamate viene passato un ID (univoco) della traccia. Una traccia è un insieme di spans, cioè un insieme di operazioni elementari che appartengono a una e una sola traccia. Gli spans sono messi poi insieme secondo l'ID della traccia.
Per trasportare l'ID lungo le chiamate si usare un protocollo: in HTTP viene usato l'header.
# Visualizzazione
Per visualizzare le tracce e le catene di chiamate si utilizza un back end. Lo standard è Jaeger.