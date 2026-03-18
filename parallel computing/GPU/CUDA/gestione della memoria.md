[CUDA](CUDA.md) fornisce delle funzioni per gestire la memoria delle [GPU](GPU.md).
# Architettura logica
![permessi memoria](permessi%20memoria.png)
A livello fisico conosciamo l'architettura della [memoria nelle GPU](architettura.md#Memoria). A livello logico, cioè considerando come poi andiamo a programmare le GPU, i permessi sono i seguenti:
- La memoria globale può essere letta e scritta dai thread dell'host e letta da tutti i thread della GPU.
- La memoria costante può essere letta e scritta dai thread host, ma solo letta dai thread device. Solitamente si usa per configurazioni.
- La shared memory fisicamente è condivisa a livello di [streaming multiprocessor](architettura.md#Streaming%20multiprocessor), ma a livello logico è suddivisa in base al blocco: i thread dello stesso blocco possono sincronizzarsi con questa memoria; non possono vedere la shared memory allocata per blocchi diversi.
- I registri possono essere letti e scritti solo dai propri thread proprietari.
# Funzioni
- $cudaMalloc$
	Alloca la global memory. Prende come parametri lo spazio da allocare e l'indirizzo del puntatore (quindi un doppio puntatore) dell'oggetto da allocare.
	Restituisce $cudaError\_t$: $cudaSuccess$ se tutto ok andato bene; $cudaErrorMemoryAllocation$ altimenti.
- $cudaFree$
	Serve per liberare la memoria globale.
- $cudaMemcpy$
	È la funzione per inizializzare la memoria allocata. Prende il parametro $cudaMemcpyKind$ che definisce la direzione in cui copiamo i dati: host2device, device2host o anche host2host e device2device.
##### Errori
In caso di errori possiamo stampare il messaggio in un formato human-readle con la funzione runtime $cudaGetErrorString$.
La prassi è definire una macro che usa questa fuznione.
# Qualificatori variabili
CUDA permette di allocare le variabili in memorie specifiche tramite dei qualificatori.
![variable qualifiers](variable%20qualifiers.png)