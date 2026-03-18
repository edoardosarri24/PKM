Nelle [GPU CUDA](CUDA.md) più thread spesso usano le stesse infomazioni per eseguire i calcoli; in questa situazione è inefficiente che ogni thread carichi dalla [global memory](architettura.md#Globale) i dati che gli servono.
Per risolvere questo problema possiamo usare i tiling: si caricano i dati nella [shared memory](architettura.md#Local%20memory) una volta all'inizio; i thread li usano e non accedono alla meoria globale.
# Problemi
Dobbiamo prestare attenzione a due cose principalmente:
##### Fasi
Le shered memory sono memorie piccole (i.e., 16KB) e [logicamente condivise](gestione%20della%20memoria.md#Architettura%20logica) tra più blocchi di uno [streaming multiprocessor](architettura.md#Streaming%20multiprocessor): se un blocco prende troppa memoria gli altri rimangono senza.
Ogni blocco deve portare i dati che gli servono in fasi, sovrascrivendo ogni volta la sua porzione di memoria.
##### Trade-off
Più un blocco è grande e più thread condivideranno i dati; questo perché la shered memory è condivisa a livello di blocco.
Aumentare troppo la dimensione del blocco fa si che serva più memoria per salvare i dati e quindi gli altri blocchi non ne avrebbero a sufficienza; in ogni momento sarebbero in esecuzione sono i thread di un dato blocco.
# Pipeline
Il tiling ha una pipeline strutturata che dobbiamo seguire:
- Identificazione della forma e dimensione dei tile.
- Caricare il tile dalla global memory alla shered memory.
- Sincronizzare i thread, con una [barriera](sistemi%20operativi/thread/sincronizzazione.md) a livello di blocco. In questo modo si garantisce che tutti i thread siano pronti.
- Consumare i dati.
- Usare un'altra barriera per assicurarci che tutti i thread abbiano finito l'esecuzione.
- Caricare nello stesso spazio i dati della prossima fase.
# Dimensione tile
I tile devono avere una dimensione piccola rispetto alla dimensone dei dati per due motivi:
- Padding
	Nella realtà non riusciamo coprire perfettamente tutti i dati con i tile; alla fine un tile coprirà dei valori fuori dai dati e dovremmo aggiungere dei padding, cioè salvare in shared memory un dato che non è poi usato.
	Se i tile sono piccoli allora i tile dove dovremmo mettere il padding sarà piccola.
- Dimensione shared memory
	Solitamente una shared memory è di 16KB. Se abbiamo tile $16\times16$ (in float) abbiamo che un tile usa 2KB di memoria. Possiamo in questo modo avere 8 blocchi, dove ognuno elabora il suo tile.
	Se invece abbiamo tile di dimensione $32\times32$ abbiamo un tile di 8KB. Al più possiamo avere 2 blocchi.