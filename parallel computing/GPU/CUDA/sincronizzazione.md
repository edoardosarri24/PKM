I [thread](thread.md) in [CUDA](CUDA.md) possono sincronizzarsi, cioè aspettare che gli altri finiscano, a due livelli:
- Sistema
	Si deve aspettare che tutti i thread siano arrivati a quel punto, sia quelli host che quelli del device.
- Blocco
	All'interno della [GPU](GPU.md) è possibile solo far parlare thread che sono all'interno dello stesso blocco. Se dobbiamo attendere tutti i tread in un punto allora possiamo usare $\texttt{\_\_device\_\_ void \_\_syncthreads(void)}$, che definisce una [barriera](barrier.md) in quel punto.