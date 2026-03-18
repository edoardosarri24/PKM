L'architettura fisica di una [GPU](GPU.md) è simile a quella della CPU, ma presente delle differenze costruttive. La terminologia adottata è quella di NVIDIA, ma i componenti sono gli stessi per tutte.
![architecture](architecture.png)
# Streaming multiprocessor
Una GPU consiste in un array di streaming multiprocessor (SM), che in generale sono detti Compute Unit (CU).
È composta da più parti:
- Threaed scheduler
	Decide quale insieme di [warp](gerarchia%20thread.md#Warp) deve eseguire in ogni ciclo di clock: in ogni momento ci sono più warp in esecuzione contemporaneamente; molti altri warp sono attivi e pronti per eseguire.
- Memoria
	È la [memoria locale](architettura.md#Local%20memory) al singolo SM.
- Register file
	I [registri](#Private%20memory) in una GPU non sono interni al core, ma interni allo SM.
- Core
	Abbiamo molti core. Ad esempio 64 core in uno SM sono pochi.
# Core
I core in una GPU sono poco più complessi di un'ALU e molto meno di un core di una CPU: non sanno fare cose complesse (e.g., branch prediction) visto che hanno poca logica e non hanno registri al loro interno, visto che questi sono nel register file dello SM.
##### SFU
Sono core particolari, detti Special Function Unit, che permettono di eseguire operazioni matematiche complesse (e.g., $sin$, $cos$, $exp$ e $log$) più velocemente (ma non in un ciclo di clock) rispetto alle ALU dei normali core.
# Memoria
In una GPU, come nelle CPU, abbiamo più livelli di memoria. Ognuna di questa memoria ha un aspetto [logico](gestione%20della%20memoria.md#Architettura%20logica) da considerare.
##### Globale
È la RAM condivisa tra tutti gli SM. Viene letta e scritta dalla CPU per mettere i dati da eseguire al suo interno.
Rispetto alle CPU abbiamo meno livelli di memoria perché la [bandwidth](bandwidth.md) è molto maggiore: si possono portare più dati dalla memoria globale ai registri dei core (o alla cache).
Si divide in due livelli:
- Global memory
	Tutti i thread hanno permessi di scrittura e lettura.
- Constant memory
	È una specie di cache, quindi molto veloce, dove tutti i thread hanno solo permessi di lettura. Solitamente viene usata per dati condivisi da tutti i thread.
##### Local memory
È la memoria interna a un singolo SM e si divide in:
- Shared memory
	È una cache L1 programmabile, cioè che i thread possono usare per scrivere dati. A livello [logico](gestione%20della%20memoria.md#Architettura%20logica) è condivisa solo dai threda che appartengono allo stesso blocco.
- Cache
	Sono cache non scrivibili dai thread.
##### Private memory
Sono i registri, cioè la parte che i thread usano per le computazioni.
Non sono a livello di core, ma contenuti nello [SM](#Streaming%20multiprocessor) e questo permette a un thread di poter eseguire in uno qualunque dei core dello SM.
A differenza delle CPU nelle GPU i registri sono moltissimi (e.g., 264K) e questo permette di avere un costo nullo nel [context switch](parallel%20computing/GPU/concorrenza.md#Context%20switch) all'interno delle GPU perché non si devono ricaricare i dati del singolo thred visto ma questi rimangono all'interno dei registri.