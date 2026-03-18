L'architettura di un sistema a memoria condivisa dove ci sono più processori (i.e., non un portatitile) si divide in due tipi: UMA e NUMA.
# UMA
![UMA](UMA.jpg)
Nell'architettura Uniform Memory Access i processori percepiscono tutti la stessa memoria: non c'è differenza per quanto riguarda posizione o tempi di accesso.
##### Limiti
I limiti principali di questa architettura sono:
- Scalabilità
- Bandwidth
- Latenza
# NUMA
![NUMA](NUMA.jpg)
Nell'architettura Non-Uniforma Memory Access (NUMA), quella utilizzata oggi, ogni processore può vedere tutta la memoria, ma tenderà a utilizzare quella più vicina.
L'idea è quella che una memoria più vicina è meno costosa da accedere: si guadagna in latenza e bandwidth.