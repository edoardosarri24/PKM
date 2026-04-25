Onos (Open Network Operating System) è un controller open source che implementa la versione [distribuita](control%20plane%20distribuito.md) del control plane.
# Architettura
Per garantire che il [control plane](control%20plane.md) sia resiliente e scalabile utilizza i sequenti concetti:
- Storage distribuito
	La memoria è distribuita usando un database come Cassandra.
- Grafo distribuito
	La topologia della rete è gestita usando un grafo distribuito tramite servizi come Titan.
- Notifica eventi
	Le notifiche tra nodi sono gestite tramite servizi come HazelCast.
- Coordinamento
	I nodi si devono coordinare e ad esempio implementare l'algoritmo del consenso.
- Comunicazione
	L'[application plane](application%20plane.md) deve vedere la rete in modo centralizzato. Questo è possibile tramite le Network View API.