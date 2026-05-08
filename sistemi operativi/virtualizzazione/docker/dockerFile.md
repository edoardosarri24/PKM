Il DockerFile è l'insieme di istruzioni che definisce [container](container.md) [docker](docker.md) e in particolare il file system (SF) di una sua [immagine](immagine.md).
# Costruzione
L'idea è quella di partire da un'immagine di base e modulare essa con layer agiguntivi che aggiungono reponsabilità al container. Dobbiamo sempre mettere nel container il meno possibile.
# Copy-on-Write policy
Il FS di un container è read-only: si aggiungono layer con il comando $\texttt{run}$ con l'idea che l'ultimo layer sovrascriva quelli sottostanti. Quando si costruisce un container da un'immagine, viene aggiunto in cima al FS un un ultimo layer read-write: questo non aggiunge nulla, ma qualunque modifica fatta ai file del SF agisce su questo layer e non su quelli sottostanti.
# Comandi
Per massimizzare la velocità, l'ordine con cui eseguire i comandi è:
- Immagine di base con $\texttt{FROM}$.
- Installazione di sistema (e.g., $\texttt{uv}$).
- Cartella applicazione.
- Dipendenze (e.g., $\texttt{requirements}$).
- Librerie (e.g., $\texttt{pip install}$).
- Codice con $\texttt{COPY}$.
- Configurazioni finali.
- Comandi di avvio.
##### From
Definisce l'immagine di base.
##### Workdir
È la directory di lavoro (come $cd$) all'interno del container.
##### Copy
Serve per copiare file o cartelle dal computer che lancia il comando al container.
##### Run
Serve per eseguire comandi durante la fase di build. Ad esempio serve per instalòare un pacchetto.
##### Env
Serve per impostare variabili d'ambiente.
##### Cmd
È il comando che viene eseguito quando il container viene lanciato.
Si usa solitamente per dare un avvio di deafult, che però può essere sovrascritto passando un agomento al comando [Cmd](dockerFile.md#Cmd).
##### Entrypoint
È il comando per eseguire uno script prima del $\texttt{main}$ principale lanciato con $\texttt{cmd}$. Di norma non è pissibile sovrascrivere questo comando quando il container parte.