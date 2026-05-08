Possiamo definite Docker come runner di [container](container.md) basato su sandbox, cioè un framework per lo sviluppo e il rilascio di applicazioni isolate l'una dall'altra.
Per la guida vedi la [documentazione](https://docs.docker.com).
# Comandi
- $\texttt{docker build}$
	Costruisce un container a partire da un [dockerFile](dockerFile.md). Il DockerFile deve essere nella stessa directory da cui si lancia il comando.
- $\texttt{docker run}$
	Esegue il container.
	Se gli viene passato un argomento allora questo viene eseguito al posto del comando in [Cmd](dockerFile.md#Cmd). Non è possibile invece sovrascrivere il comando [Entrypoint](dockerFile.md#Entrypoint).
-  $\texttt{docker compose build}$
	Costruisce un container a partire da un [docker compose](docker%20compose.md).
-  $\texttt{docker compose up}$
	Serve per lanciare il container precedentemente costruito.
- $\texttt{docker compose down}$
	Spegne il container.