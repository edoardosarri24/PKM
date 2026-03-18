Si tratta di un [obiettivo](multi%20obiettivo%20in%20MAS.md) in un sistema multi agente in cui vogliamo monitorare un'area $X$ con un sistema di agenti. Si suppone che si abbia un numero di agenti $N$ tale che esista una qualche loro distribuzione nello spazio che permetta di coprirne la maggior parte (altrimenti ha più senso un task di [mapping](mapping.md)). L'obiettivo è disporre gli agenti in modo da minimizzare l'area non coperta $\displaystyle X-\bigcup_{i=1}^NFoV_i(x_i)$, dove il Field of View (FoV) è un sensore (e.g., una telecamera) che vede lo spazio.
# Voronoi partition
Supponendo che i sensori degli agenti siano tutti equivalenti e circolari, cioè siano tali che $FoV_i(x_i)=FoV(x_i)$ e $FoV(x_i)=\{p:||p-x_i||\le\rho\}$, allora possiamo minimizzare l'area non coperta grazie alle partizioni di Voronoi.
##### Definizione
In uno spazio euclideo $X$, l'$i$-esima partizione di Voronoi, definito il suo centro $x_i$, è l'insieme dei punti dello spazio $C=\{p\in X:||p-x_i||\le||p-x_j||,\forall j\ne i\}$, cioè è l'insieme dei punti dello spazio che sono più vicini al centro $x_i$ rispetto a tutti gli altri centri $x_j$.
Quando lo spazio $X$ è un poligono convesso, allora ogni partizione di Voronoi è un poligono convesso.
##### Soluzione
Nel caso di agenti che si devono disporre in modo da minimizzare l'area non coperta, possiamo suddividere lo spazio in $N$ partizioni di Voronio, con $N$ agenti nel sistema.
Una volta assegnata una partizione a un agente, facciamo muovere l'agente verso il centroide di tale partizione. Quando l'agente si muove la partizione si modifica; ricalcoliamo tale parzione, il suo centroide e iteriamo in questo modo.
Si può dimostrare che questo algoritmo converge in ambienti stabili.