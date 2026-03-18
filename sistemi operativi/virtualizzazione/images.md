Un'immagine [Docker](docker.md) è un modello utilizzato solo in lettura (non modificabile) utilizzato per creare un container.
Un'immagine è il progetto del container dichiarato nel Dockerfile, come una classe; un [container](container.md) è l'istanza in esecuzione, come l'istanza di una classe. A partire dalla stessa immagine possiamo creare molte sue istanze, ognuna delle quali avrà vita propria.
# Caratteristiche
Un'immagine Docker è:
- Immutabile
	Una volta che viene creata non può essere modificata. Se dobbiamo fare delle modifiche dovremmo creare una nuova immagine, eventualmente a partire da quella originale.
- Stratificata
	Un'immagine è composta da più layer. Ogni layer è rappresentato da un'istruzione dell'immagine che modifica il file system: si aggiungono, rimuovono o modificano file o cartelle.
- Portabile
	Un'immagine si comporterà sempre nello stesso modo su qualunque macchina host.
# Layer
Dobbiamo pensare un'immagine come una serie di layer non modificabili. Ogni istruzione dell'immagine aggiunge un layer sopra quello precedente e contiene solo le modifiche apportate da quella istruzione.
##### Dockerfile
Il Dockerfile è il file (senza estensione) di configurazione della nostra immagine. Contiene una serie di istruzioni chiave che permettono di aggiungere layer all'immagine di base.
Possiamo infatti pensare a un'immagine docker come un qualcosa di modulare: il pirmo layer è un'immagine ottenuta da un altro container; le istruzione del nostro Dockerfile aggiungono o modificano la struttura del file system del container base.
Per passare dal Dockerfile all'immagine Docker si utilizza $\texttt{docker build}$.
##### File System
Vediamo come viene costruito il file system di un container.
Il Docker file abbiamo detto definisce i layer non modificabili. Ognuno di questo è una parte o un intero file system: l'immagine base costituisce un intero file system; se con l'istruzione $\texttt{RUN}$ eseguiamo un'installazione questa conterra i relativi binari. Nel momento della costruzione del container viene unito tutto e creato un file system unico; se un file esiste in più layer allora quella del layer più esterno (i.e., l'ultimo definito) nasconde le altre.
Nel momento della costruzione del container, prima dell'unificazione dei file system, viene aggiunto un ulteriore layer read-write. Quando un file che esiste in un layer inferiore deve essere modificato, viene copiato nel layer read-write, e viene modificato qua. A questo punto l'ultima versione nasconda l'originale per quello che abbiamo detto sopra. Questo meccanismo prende il nome di Copy-on-Write (CoW).
# Registry
Un registry è un iniseme di repository, dove ogni repository contiene tutte le versioni delle immagini di una data applicazione. All'interno di un team l'idea è che si prenda l'ultima versione dell'immagine di un container dal registry, si faccia le modifiche che si devono fare e si carichi la nuova versione.
Senza un posto dove condividere container non avrebbe senso l'idea stessa dei container, cioè distribuire software in modo indipendente dalla macchina.
[Docker hub](https://hub.docker.com) è il registry ufficiale di Docker. Ci sono anche altri registries, come quello di Amazon e Google; si può creare anche un proprio registry interno a un'azienda.