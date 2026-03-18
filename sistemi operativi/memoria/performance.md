Per misurare le performance della memoria ci sono due metriche fondamentali:
- Latenza
	È il tempo che trascorre tra la richiesta del processo e il momento del servizio da parte della memoria; ad esempio 100 cicli di clock.
- Bandwidth
	È il rate con cui la memoria fornisce dati al processore; ad esempio 10GB/s.
	Questo è il problema principale delle memorie e per questo motivo la cosa migliore che possiamo fare è avere meno [cache miss](cache%20miss.md) possibili in modo da ottimizzare al meglio la memoria.
# Upper bound
Oggi le performance delle memorie, come quelle delle CPU, sono arrivate a un upper bound; per mitigare questo si costruiscono memorie più grandi per evitare i cache miss e si aggiungono core per CPU in modo da parallelizzare.