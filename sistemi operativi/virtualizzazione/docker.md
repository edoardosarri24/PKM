Possiamo definite Docker come runner di [container](container.md) basato su sandbox, cioè un framework per lo sviluppo e il rilascio di applicazioni isolate l'una dall'altra.
Per la guida vedi la [documentazione](https://docs.docker.com).
# Volumi
Un container in Docker è per natura effimero: quando viene eliminato tutto quello che era al suo interno viene perso. Questo è un serio problema per le applicazioni reali e viene risolto tramite i volumi.
Quando un container viene montato possiamo dire a Docker di memorizzare tutti i file in un volume. Teoricamente questo volume è come se fosse un hard disk esterno, in pratica è una cartella nella macchina host. In questo modo il ciclo di vita del volume diventa indipendente da quello del relativo container.
##### Creazione
Ci sono vari modi per creare un volume:
- Esplicitamente
	Si crea a mano un volume con $\texttt{docker volume create nome-del-mio-volume}$ e quando si avvia il container con $\texttt{docker run}$ si attacca il volume.
	È il metodo migliore: sappiamo dove è il volume e come si chiama e possiamo attaccarlo a più container.
- Implicitamente
	Quando si avvia un container tramite run oppure tramite un docker compose si specifica nome e posizione del contaier tramite $\texttt{docker run -d --name come-container -v nome-volume:/app/data immagine}$. Il risultato è lo stesso del caso esplicito, è meno chiaro ma va bene lo stesso; si può anche specificare un volume esterno al contianer che è condiviso con un altro container.
- Anonimi
	Se creiamo un volume tramite run e non lo nomiano come ad esempio in $\texttt{docker run -d --name come-container -v /app/data immagine}$ allora quello che succede è che creiamo un volume senza sapere come si chiama. Questo non va fatto.
# Docker compose
Il problema che vogliamo risolvere è quello di voler fare un qualcosa di complesso mettendo insieme più container.
Una prima soluzione è eseguire più $\texttt{docker run}$. In questo modo il problema che si presenta sempre è gestire la rete e la connessione tra container e altre cose. Inoltre quando abbiamo finito pulire le cose è più complesso.
Tramite $\texttt{docker compose}$ possiamo semplificare questo processo tramite una singola configurazione in un file $.yml$. Se mettiamo il file di configurazione nel repository all'interno del registry, chi clona il nostro repo può avviare semplicemente l'insieme di container tramite il docker compose; se facciamo delle modifiche sui file può avviarle tramite $\texttt{docker compose up}$; quando vogliamo chiudere l'applicazione senza problemi quello che si può fare è $\texttt{docker compose down}$.
# Comandi
- $\texttt{docker compose watch}$
	Permette di osservare modifiche in live. Se ad esempio abbiamo un front end e modifichiamo i relativi fine allora vediamo direttamente le modifiche.