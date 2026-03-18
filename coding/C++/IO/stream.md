Gli stream I/O sono un meccanismo [C++](C++.md) (e non solo) per spostare i data dal disco alla memoria con un alto livello di astrazione. Sono quindi facili da usare e portabili, ma non veloci (possiamo in questo caso usare [mmap](mmap.md)).
# Piepeline
Quando usiamo questo approccio solitamente facciamo qualcosa del tipo $\texttt{file >> value}$. Quello che succede è la sequente cosa:
- System call $\texttt{read()}$: il programma esce dalla user mode e invoca il kernel.
- DMA transfer: i dati vengono portati dal disco alla Kernel Page Cache, cioè la memoria gestita dal SO.
- I dati verngono copiati dalla RAM del kernel in un buffer dello users space.
# Conseguenze
I costi sono molto elevati a causa di:
- Molte system calls.
- Double copy
	Prima i dati vengono copiati dal disco alla memoria del kernel e poi dalla memoria del kernel al buffer dell'utente.
- Semplicità di utilizzo.
- Se deve essere fatto un parsing questo sarà gestito dallo stream.