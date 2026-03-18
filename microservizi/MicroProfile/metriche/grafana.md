Grafana è un backend per la visualizzione di metriche, solitamente raccolte da [prometheus](prometheus.md), usato per applicazioni a [microservizi](microservizi.md).
# Funzionamento
![prom graf flow](prom%20graf%20flow.png)
Grafana interroga Promethues (di deafult ogni 30s) e grafica quello che riceve.