A differenza della RTA classica, la pRTA (probabilistic RTA) gestisce task i cui tempi di esecuzione sono [random variable](random%20variable.md) indipendenti e identicamente distribuite (iid).
L'obiettivo non è più verificare se il WCET del task può superare la deadline usando un singolo valore, ma valutare il pWCRT (probabilistic worst-case responce time), cioè una distribuzione di probabilità del responce time in funzione della dead-line del task: mi dice quale è la probabilità che all'istante $t$ il task abbia finito la sua esecuzione.
# Istante critico
Uno dei problemi quando si affrontano questi problema è definire l'[istante critico](istante%20critico.md). In contesti [probabilistici](istante%20critico.md#Soft%20real-time) di soft real-time infatti non possiamo considerare quello definito dal teorema di Lui e Layland.
# Quantità
Le quantità che ci interessano sono:
- $R_{k}$ è il tempo di risposta probabilistico (i.e., la distribuzione del tempo di risposta) del task $\tau_{k}$. In pratica è l'ouput dell'analisi pRTA, cioè pWCRT.
- WCRTEP (Worst-Case Response Time Exceedance Probability)
	È la funzione $P(R_{k}>t)$ e ci dice quale è la probabilità che il responce time probabilistico superi un certo valore soglia $t$. Solitamente $t=D_{k}$, cioè si vuole sapere quale è la probabilità che il tempo di risposta superi la deadline.
- WCDFP (Worst-Case Deadline Failure Probability)
	Solitamente nel calcolare il WCRTEP si setta $t=D_{k}$, cioè si vuole sapere quale è la probabilità che il tempo di risposta superi la deadline. Questo valore qua è il valore WCDFP; si tratta quindi del valore di un probabilità, i.e., $WCDFP\in[0,1]$.
# Carry-in
In [questo articolo](critical%20instant%20for%20Probabilistic%20timing%20uarantees%20refuted%20and%20revisited.pdf.pdf) (sezione $IV$) suppongono che un job venga abortito quando e se supera la propria dead-line e dimostrano che il responce time del task $\tau_{k}$ si può ottenere includendo un ulteriore job per tutti i task $\tau_{i}\in hp(\tau_{k})$, dove $hp(\tau_{k})$ sono i task con priorità più alta di $\tau_{k}$.
Questo ulteriore job rappresenta l'execution time dei task a priorità più alta che ancora non è stato consumato totalmente al rilascio simultaneo dei task e che quindi ha scavallato (i.e. carry-in) l'inizio dell'analisi.
##### Teorema
Formalmente definiscono un teorema che afferma che il WCRTEP del task $\tau_{k}$ per un responce time target $R_{k}$, tale che $0\leq R_{k}\leq T_{k}$, è limitato da un valore, cioè $\displaystyle P(R_{k}>t)\le\inf_{0 \leq t\leq R_{k}}P(S_{t}>t)$, dove:
- $T_{k}$ è il periodo del task $k$-esimo.
- $S_{t}=C_{k,1}+\displaystyle \sum_{\tau_{i}\in hp(\tau_{k})} \sum_{q=1}^{\lceil \tfrac{t+D_{i}}{T_{i}} \rceil}C_{i,q}$, è quindi la somma del primo job del task in esame e dei job dei task a priorità più alta che rientrano nell'intervallo $[0,t]$ più uno (il carry-in). La quantità $S_{t}$ è una variabile casuale perché i tempi di esecuzione $C_{i,j}$ sono tutte variabili casuali.
	In [questo](analytical%20approximations%20in%20probabilistic%20analysis%20of%20real%20time%20systems.pdf) (sezione $IV$) articolo si definisce la quantità $\bar{S}_{t}=\displaystyle \sum_{\tau_{i}\in hp(\tau_{k})} \sum_{q=1}^{\lceil \tfrac{t+D_{i}}{T_{i}} \rceil}C_{i,q}$ come il limite superiore (del carico di lavoro probabilistico accumulato) all'interferenza che i task a priorità più alta esercitano, e si dimostra che essa è valida sia nel caso in cui i job che superano la propria deadline vengono abortiti sia che continuino.
	Inoltre si dimostra il comportamento asintotico di questa quantità grazie al [teorema del limite centrale di Lyapunov](teorema%20del%20limite%20centrale%20di%20Lyapunov.md), rendendo il calcolo dell'approssimazione della distribuzione di probabilità della deadline-miss trattabile tramite un approccio [analitico](probabilistic%20timing%20analysis.md#Analitiche).
- Il WCRTEP, i.e. $\displaystyle P(R_{k}>t)$ è definito come l'uppur bound della probabilità che il responce time di un job del task $\tau_{k}$ sia maggiore di un istante di tempo $t$. Questo vuol dire che $WCRTEP=\displaystyle\sup_{j\in\mathbb{N}}P(R_{k,j}>t)$).
# Inflation (sezione $IV.B$)
L'idea è quella di inflazionare (i.e., aumentare) la distribuzione dei tempi di esecuzione dei job già presenti nell'intervallo.
Si definisce la $SAI(\tau_{k},a,b)=\displaystyle\max\left[ \sum_{c\in S}c|S\subseteq\left[C_{i,1},\cdots, C_{i,b}\right],|S|=a\right]$: si considerano $b$ campionamenti dalla distribuzione $C_{i}$ del tempo di esecuzione del task $\tau_{i}$ (nota che questi tempi sono i.i.d); prendiamo gli $a$ valori maggiori; facciamo la somma di questi $a$ valori.
Siccome non sappiamo quale dei job del task causerà l'interferenza maggiore, dobbiamo considerare un insieme maggiore di job e prendere i casi peggiori.
Il teorema ci dice che la probabilità che il responce time di $\tau_{k}$ superi il valore target $R_{k}$ è limitata da $\displaystyle\inf_{0 \leq t\leq R_{k}}P\left(C_{k}+\sum_{\tau_{i}\in hp(\tau_{k})}SAI(\tau_{i},a,b)>t\right)$, cioè che $WCRTEP(R_{k})=\displaystyle\inf_{0 \leq t\leq R_{k}}P\left(C_{k}+\sum_{\tau_{i}\in hp(\tau_{k})}SAI(\tau_{i},a,b)>t\right)$ (WCDFP nel caso in cui $R_{k}=D_{k}$). Abbiamo che:
- $a=\left\lceil  \frac{t}{T_{i}}  \right\rceil$: È il numero di job che entrano in un intervallo di tempo $t$.
- $b=\lambda_{i}^t$: È il numero totale di job da cui campionare, definito con una formula precisa qui non riportata.
# Conseguenze
Osserviamo che:
- Con pochi task il metodo con il SAI è migliore, mentre il carry-in tende a essere troppo pessimistico. In alcuni casi, utilizzando il carry-in si ottiene una probabilità di rigettare il task di 1.
- Se abbiamo molti task allora il metodo del carry-in è esatto ed è migliore.