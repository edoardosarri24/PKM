Un'immagine [Docker](docker.md) è un modello utilizzato solo in lettura (non modificabile) utilizzato per creare un container.
Un'immagine è il progetto del container dichiarato nel [Dockerfile](#Dockerfile), come una classe; un [container](container.md) è l'istanza in esecuzione, come l'istanza di una classe; a partire dalla stessa immagine possiamo creare molte sue istanze, ognuna delle quali avrà vita propria.
# Caratteristiche
Un'immagine Docker è:
- Immutabile
	Una volta che viene creata non può essere modificata. Se dobbiamo fare delle modifiche dovremmo creare una nuova immagine, eventualmente a partire da quella originale.
- Stratificata
	Un'immagine è composta da più layer. Ogni layer è rappresentato da un'istruzione dell'immagine che modifica il file system: si aggiungono, rimuovono o modificano file o cartelle.
- Portabile
	Un'immagine si comporterà sempre nello stesso modo su qualunque macchina host.
# File system
Dobbiamo pensare un'immagine come una serie di layer non modificabili e indipendenti. Chi si occupa di definire il FS di un container è il [dockerFile](dockerFile.md).