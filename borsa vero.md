- Capire cosa vogliamo pubblicare
	- Quale è il contributo dell'articolo: cosa ti da rispetto a quello che c'è già.
- Parallelo al punto precedente capire cosa già è presente in letturatura.
- Confrontare alibaba con altri dataset.
# Intro
Non esistono benchmark realistici e di dimensioni considerevoli.
# Contributo
Voler fornire delle topologie di depency graph realistiche e che abbiano in corpo dei singoli task che hanno un ET stocastico realistiche. Inoltre vogliamo anche esprimere la correlazione tra due microservizi: non solo attaccare a ogni microservizio la distrubzone del tempo di esecuzione, ma anche che questa sia basata su cosa ha fatto il microservizio precedente.
Si fornisce un tool basato su kubernetes, le traces di alibaba cluster e l'algoritmo di alibaba che tira fuori un deployment kuberntes generato nel modo che abbiamo detto.
# Implementazione
Deployment con kubernes.
Capire cosa intendono per callgraph, i.e. come usare la topologia per definire and o xor.