Il problema del robot motion planning è uno dei problemi fondamentali in ambito di [agenti intelligenti](agenti.md#Intelligenti).
Dato un [configuration space](spazi.md#Configuration%20space) $C$, un insieme di [configurazioni ammissibili e inammissibili](spazi.md#Free%20and%20obstacle%20space) $C_{free}$ e $C_{obs}$, uno start $q_I$ e un goal $q_G$, il problema del robot motion planning sta nel trova il path minimo da $q_I$ a $q_G$ (oppure un insieme di goal $C_G$) in modo automatico.
# Planning-based solution
La soluzione più semplice è basata sul paradigma [deliberato](paradigmi%20di%20agenti.md#Deliberato), cioè dove abbiamo il modello del mondo completo(i.e., conosciamo interamente $C_{obs}$ e $C_{free}$).
##### Pipeline
Si basa su tre step:
- Costruire una road map, cioè costruire un grafo all'interno di $C_{free}$, dove un nodo rappresenta una configurazione ammissibile e un arco un percorso tra due configurazioni ammissibili che non entra in conflitto con un ostacolo.
	- [visibility graph](visibility%20graph.md)
	- [Discrettizzazione](modello.md#Discretizzazione) del configuration space
		- [exact cell decomposition](exact%20cell%20decomposition.md)
		- [approximate cell decomposition](approximate%20cell%20decomposition.md)
	- Approcci probabilistici
		Si utilizzano tecniche probabilistiche perché sono computazionalmente fattibili, rispetto alle altre che sono complesse.
		- [probabilistic road map](probabilistic%20road%20map.md)
		- [rapidly-exploring random trees](rapidly-exploring%20random%20trees.md)
- Trovare un percorso ottimo nella road map. I nodi della roadmap sono detti hotspot. Per fare questo ci sono degli algoritmi che non ci interessano qua.
- Trasformare il path discreto trovato in una traiettoria smooth. Questo permette di far percorrere al robot un path che al suo interno non ha punti (i.e., gli hotspot) in cui la curva non è derivabile. Per fare questo ci sono degli algoritmi che non ci interessano qua.
##### Limiti
I limiti di questo approccio sono:
- Si deve avere una conoscenza totale dell'ambiente.
- L'ambiente deve essere statico.
- Si deve poter rappresentare l'ambiente in modo strutturato. Ad esempio si devono definire geometricamente tutti gli ostacoli.
- Si deve lavorare nel [configuration space](spazi.md#Configuration%20space). Se questo ha una dimensionalità molto alta allora si ha un costo computazione troppo alto.
# Unstructure environment
La soluzione che si basa su paradigma [reattivo](paradigmi%20di%20agenti.md#Reattivo) si usa in ambienti cartesiani in due o tre dimensioni, dove l'agente è rappresentato come un puntiforme che utilizza sensori per definire la rotta.
##### Conseguenze
Si risolvono alcuni limiti dell'approccio [planning-based solution](robot%20motion%20planning.md#Planning-based%20solution):
- Non è richiesta una mappa, ma si devono avere dei sensori.
- Se lo spazio delle configurazioni ha un'alta dimensionalità non è un problema (fino a un certo punto). In alcuni casi è necessario combinare i due approcci.
- I sensori non sono perfetti e si devono considerare possibili errori.
##### Algoritmi
- [bug algorithms](bug%20algorithms.md)
- [tangent bug](tangent%20bug.md)
- [artificial potential field](artificial%20potential%20field.md)
##### Combinazioni
In ambienti non strutturati in realtà quello che si fa è quindi usare un approccio sia deliberativo che reattivo.