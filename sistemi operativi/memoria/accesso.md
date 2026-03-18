Quando un [processo](processo.md) ha bisogno di un dato nei registri per farci delle operazioni, lo va a cercare prima nella cache più vicina. Se non si trova e si verifica un [cache miss](cache%20miss.md) nella cache allora viene portato dalla [memoria](memoria.md) (RAM o da altri livelli di cache) nella cache più vicina e poi usato.
# Burst
Quando un dato viene portato dalla memoria alla cache non viene trasportato da solo, ma si riempie un'intera linea di cache seguendo la località spaziale: si trasporta un burst, cioè un gruppo di dati che sono vicini in ram.
# Cerenza
Se più core hanno bisogno dello stesso dato, allora avremmo la stessa vista della RAM su più cache di core diversi; per mantenere poi la stessa vista della cache si utilizza un protocollo di coerenza nelle cache.
Se una cache L1 viene modificata allora si invalidano tutte le linee di cache degli altri core.
# Coalescing
L'accesso per coalescing vuole sfruttare il caricamento in burst della memoria per aumentare le prestazioni, soprattutto in un sistema [parallelo](parallel%20pragramming.md) e soprattutto nelle [GPU](GPU.md).
Se più thread (e.g., nelle GPU, più thread di uno stesso blocco) hanno bisogno di dati vicini allora si può guadagnare tempo caricando tutti questi dati vicini con un unico caricamento, cioè quello eseguito dal primo thread che richiede un dato; se hanno bisogno di dati lontani invece si devono caricare in più caricamenti.