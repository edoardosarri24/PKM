# paper
- [a survey of probabilistic timing analysis techniques for real-time systems](a%20survey%20of%20probabilistic%20timing%20analysis%20techniques%20for%20real-time%20systems.pdf)
	- se serve approfondire la sezione $IV$ che parla dei MBPTA in modo dettagliato
- [critical instant for Probabilistic timing uarantees refuted and revisited.pdf](critical%20instant%20for%20Probabilistic%20timing%20uarantees%20refuted%20and%20revisited.pdf.pdf)
	- Nell'introduzione fa una carrelata di paper dove si fanno assunzioni sbagliate, tra cui alcuni lavori anche loro.
	- Nella sezione $VI$ vengono presentati gli esperimenti per valutare come i bound definiti dalle distribuzioni pWCRT funzionino rispetto agli approcci in cui si considerava la classica RTA con il classico critical instant.
- [analytical approximations in probabilistic analysis of real time systems](analytical%20approximations%20in%20probabilistic%20analysis%20of%20real%20time%20systems.pdf)
	- All'inzio ci sono un sacco di riferimenti a metodi analitici per trovare gli Upper Bound.
	- Non prende in considerazione che la distrivuzione del esecuzione $C_{i}$ sia continua: tutti i ragionamenti che fa sono sul discreto.
	- Non ho studiato alla fine gli esperimenti.
- Bernstein
	- [approximation of cumulative distribution functions by bernstein phase-type distributions](approximation%20of%20cumulative%20distribution%20functions%20by%20bernstein%20phase-type%20distributions.pdf)
	- [EPEW26_bernstein (1)](EPEW26_bernstein%20(1).pdf)
# Da vedere io
##### Interferenza
È giusto dire quello che ho scritto sulla quantità $\bar{S}_{t}$ [qua](probabilistic%20responce%20time%20analysis.md#Carry-in%20(sezione%20$IV.A$))? quello tra parentesi è la definizione del paper.
##### Interferenza soft real-time
Nella soft ci potrebbe far comodo non avere il caso pessimo di numero di job che fanno interferenza. vogliamo una distrubzione di questo numero di job. per trovarla facciamo le combinazioni e poi somma pesata: otteniamo una distribuzione del RT per ogni combinazione e poi le sommiamo in modo pesato.
# 20260402
abbiamo distribuzioni del tempo di esecvuzione del task e vogliamo fare convluzioni. questo fa rappiare un gradi e dobbiamo riportarli.
- noi non facciamo le conv in forma numerica ma in forma anlitica. se ho i pol di bern che rappresentano le distri e faccio conv cosa succede?
- la forma analitica è ad esempio un polinomio o un ex polinomio. il primo problema è che se ho una distr approssimata da un polinomio quale è il pol di bernstein che meglio approssima
	- non è detto che pol di bern costruito con la formula sia la miglior approssimazione di quella funzione (sicuramente è un'approssimazione). questo deriva dalla non interpolaza.
	- ci serve una libreria che permetta di fare esperimenti su questa cosa, cioè di trovare un modo per capire come passare nel modo migliore dal polinomio che rappresenta la ECDF in forma analitica al PB.
		- una prima sol è usare lo stesso grado per PB con quello del polinomio che approssima la ECDF.
		- una seconda cosa è usare un grado possibile.