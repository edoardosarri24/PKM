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
- [approximation of cumulative distribution functions by bernstein phase-type distributions](approximation%20of%20cumulative%20distribution%20functions%20by%20bernstein%20phase-type%20distributions.pdf)
# Domande
##### Scenario
Da quello che ho capito è [questo](timing%20analysis.md#Probabilistic%20time%20analysis).
##### Terminologia
Quando si parla delle tecniche per la [probabilistic responce time analysis](probabilistic%20responce%20time%20analysis.md) si parla di pWCRT (che il paper chiama WCRTEP) e WCDFP. Cosa cambia? Cioè la WCDFP si ha quando $R_{k}=D_{k}$, e quindi? Cosa esprime uno e cosa l'altro?
##### Quello che vogliamo fare noi
Noi vogliamo quindi inserirci [qua](probabilistic%20responce%20time%20analysis.md#Risoluzione) rappresentando i tempi di esecuzione tramite polinomi di berstain?
##### Dipendenza pWCET (3:10 in [questo](a%20survey%20of%20probabilistic%20timing%20analysis%20techniques%20for%20real-time%20systems.pdf) paper.)
Noi vogliamo il responce time del task a priorità più bassa: visto che gli altri avranno una probabilità minore di mancare la deadline, se quello a priorità più bassa ha una probabilità sotto la soglia che definiamo allora ok.
Per ottenere il responce time nel caso non probabilistico facciamo la [responce time analysis](responce%20time%20analysis.md) sommando i WCET dei task a priorità più alta. Nel caso probabilistico facciamo la stessa cosa con le distribuzioni pWCET che sono rappresentare con i polinomi di Bestain.
Sommando random variabile che sono dipendenti usando la convoluzione (ma forse noi non usiamo la convoluzione perché polinomi di Bernstein?) classica otteniamo una distribuzione ottimistica che non ci piace.
##### Execution time probabilistico e discreto
Loro usano solitamente un execution time probabilistico e discreto in modo da rendere trattabile il problema (cioè per questi motivi [qua](probabilistic%20responce%20time%20analysis.md#Risoluzione)) o per altri motivi?
In realtà forse è proprio l'utilizzo di questa discretizzazione che fa esplodere il problema?
se così fosse, come penso che sia, lo scenario in cui siamo direi che è [questo](probabilistic%20timing%20analysis.md#Difficoltà) per il contesto del probabilistico nel soft real-time.
##### AI
Le tecniche basate su AI per cercare di identificare pattern di violazione delle dealline (come quello fatto con il Lippi) servono per risovlere i problemi di [consevatività](timing%20analysis.md#Consevatività)?
I metodi simbolici che ho usato con il Lippi sono spiegabili ma poco potenti, mentre quelli basati su DNN (transformer magari) sono sicuramente più potenti però non ci danno nessuna spiegazione. Le tecniche basate su WCET e pWCET ci danno spiegabilità?
##### Interferenza
È giusto dire quello che ho scritto sulla quantità $\bar{S}_{t}$ [qua](probabilistic%20responce%20time%20analysis.md#Carry-in%20(sezione%20$IV.A$))? quello tra parentesi è la definizione del paper.
##### BE
- Non mi è chiaro come possiamo approssimare CDF con i polinomi di bernstein senza usare i BPH (Bernstein Exponential).
- Per l'approssimazione dobbiamo avere la vera CDF? come ricaviamo la CDF dai dati?