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
# Domande
##### Interferenza
È giusto dire quello che ho scritto sulla quantità $\bar{S}_{t}$ [qua](probabilistic%20responce%20time%20analysis.md#Carry-in%20(sezione%20$IV.A$))? quello tra parentesi è la definizione del paper.



# Interferenza soft real-time
Nella soft ci potrebbe far comodo non avere il caso pessimo di numero di job che fanno interferenza. vogliamo una distrubzione di questo numero di job. per trovarla facciamo le combinazioni e poi somma pesata: otteniamo una distribuzione del RT per ogni combinazione e poi le sommiamo in modo pesato.
# Polinomi Bernten
- classe base
- classe polinomio
- classe di funzioni statiche che fa cose sui polinomi
	- calcolare derivata (articolo)
	- convoluzione di polinomi
		precalcolare i polinomi di bernstein, metterli in una matrice e poi usarli. in generale ottimizzare cose.
	- visualizzare
##### apporssimazione
- come scegliere i punti
- come riprtare il grado $n$ dopo aver fatto la convoluzione su polinomi.