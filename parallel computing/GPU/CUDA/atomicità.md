Il linguaggio [CUDA](CUDA.md) ci mette a disposizione meccanismi hardwere per implementare istruzioni atomiche sulle [GPU](GPU.md).
# Contesto
Si possono eseguire itruzioni atomiche a due livelli:
- Shared memory
	Si sta sincronizzando thread a livello del blocco, cioè sulla memoria dello [streaming multiprocessor](architettura.md#Streaming%20multiprocessor) che è allocata al singolo blocco.
- Global memory
	È molto più costosa perché richiede di sincronizzare tutti i thread dei vari streaming multiprocessor.