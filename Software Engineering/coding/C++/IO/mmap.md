La funzione di basso livello $\texttt{mmap}$ (Memory Mapped I/O) usa la memroia virtuale per portare i dati dal disco alla memoria dell'utente in modo efficiente, limitando al massimo la doppia copia che fanno ad esempio gli [stream](stream.md).
# Pipeline
Il processo è il sequente:
- Mapping
	Il kernel restituisce un indirizzo al processo, che fa finta che il file sia già totalmente presente in memoria a partire da quell'indirizzo virtuale.
- Page fault
	Inizialmente non viene caricato nulla. Quando il processo va a prendere un pezzo del file dalla sua memoria si verifica un page fault: il kernel cattura tale eccezione, legge il blocco dal disco e lo mette direttamente nella cache del processo, saltano la copia nella Kernel Page Cache.
# Ottimizzazioni
Se il file viene letto in modo sequenziale possiamo dirlo al kernel per minimizzare i page fault. Con $\texttt{MADV\_SEQUENTIAL}$:
- Il kernel carica in memoria le pagina prima che queste siano richieste.
- Una volta passati gli indici di quella pagina il kernel può rimuoverla perch suppone che non sarà più utile.
# Conseguenze
- Quando i file sono piccoli $\texttt{mmap}$ ha lo svantaggio di avere un oberhead fisso. Se il file è grande si hanno vantaggi.
- Se dobbiamo leggere pezzi di file sparsi, allora a ogni accesso si lancia un'eccezione kernel page fault e questo ha un costo. Se invece l'accesso è sequenziale si hanno quandi vantaggi.
- Se dobbiamo scrivere in appena a un file mappato è molto costoso perché dovremmo allungare il file mappato e questo ha un impatto anche sul disco.
- In architetture a 32 bit abbiamo solo 2-3GB di spazio virtuale indirizzabile: se il file è molto grande abbiamo problemi. Nei sistemi a 64 bit non abbiamo invece praticamente limitazioni.
- Se deve essere fatto un aprsing sul file questo deve essere fatto a mano, mentre gli [stream](stream.md) facilitano il compito.