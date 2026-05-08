Si tratta di un file $\texttt{.yaml}$ usato per fare qualcosa di complesso mettendo insieme più [container](container.md) in un ambiente [docker](docker.md).
# Problema risolto
Il docker-compose si occupa di:
- Laciare e usare più container insieme.
- Gestire la rete tra container.
- Gestire la connessione con l'esterno.
- Pulire più container quando qualcosa finisce.
# Immagine vs compose
Il docker-compose non costruisce nuove [immagini](immagine.md), ma si occupa solo di lanciare quelli già definiti.
##### Variabili ambiente
Un caso particolare dove questo è utile sono le vairabili d'ambiente:
- Se esse sono necessarie e definiscono l'immagine allora devono essere inserite nel [dockerFile](dockerFile.md).
- Se sono parametri che possono essere passati anche dopo la costruzione a partire dall'immagine, oppure non definiscono il container (i.e., esso funziona anche senza), allora devono essere messi nel compose.