Lo standard per il gestire il ciclo di vita dei [microservizi](microservizi.md) nel cloud è [kubernetes](microservizi/kubernetes/kubernetes.md). Il framework [quarkus](quarkus.md) offre una perfetta integrazione con esso, permettendo di generare automaticamente i manifesti per le varie applicazioni.
# Estensione
Per lavorare con Kubernetes in Quarkus basta installare la relativa estensione, $quarkus-kubernetes$.
Ci sono anche altre soluzioni:
- OpenShift
- KNative, per serverless.
# Funzionamento
Quando facciamo il [building](building.md) dell'applicazione con l'estensione di kubernetes installata, all'interno della cartella $target/kubernetes/$ viene generato il manifesto $kubernetes.yml$.
# Personalizzazione
Nelle applicazioni reali spesso dobbiamo personalizzare le risorse che vogliamo dedicare a ogni servizio. Per fare questo possiamo ancora una volta usare le [configurazioni](configurazioni.md) di Quarkus nel file $application.properties$.
Ci sono molte configurazione che possiamo specificare per generare il manifesto: numero di repliche, nome, label; per vedere tutte possiamo andare [qua](https://quarkus.io/guides/all-config) cercando kubernetes.