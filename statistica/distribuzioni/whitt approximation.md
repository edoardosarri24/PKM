Le approssimazioni di Whitt sono delle configurazioni di [PH](continuos%20Phase%20Type%20(PH)%20distribution.md) e rappresentano una tassonomia di distribuzioni utili quando la [distribuzione esponenziale](distribuzione%20esponenziale.md) non rappresenta fedelmente la realtà. Quest'ultima infatti ci semplifica i calcoli, ma spesso la sua proprietà di [memoryless](distribuzione%20esponenziale.md#Memoryless) non è adatta nella vita reale; ad esempio quando il tempo di soggiorno in uno stato dipende dal tempo trascorso negli stati precedenti.
##### Idea
Se partiamo dall'esponenziale, sappiamo che il suo [coefficiente di variazione](momento.md#Coefficiente%20di%20variazione) è $C_V=\tfrac{\sigma_X}{\mu_X}=1$; non tutti i fenomeni però possono portano a $C_V=1$. In questi casi, possiamo raccogliere i dati, calcolare il $C_V$ e definire i parametri del giusto approssimante in modo che fitti al meglio i dati.
Quello che stiamo facendo è fittare una distribuzione ai dati che manga il primo e secondo momento.
##### Catene di Markov
In una catena di [markov chain](markov%20chain.md) i tempi di soggiorno in uno stato sono rappresentati da una distribuzione esponenziale. Se i dati del workflow reali non hanno il coefficiente di variazione uguale a 1, allora perdiamo la proprietà di memory less.
Tramite le whitt approximation stiamo scomponendo ogni stato in più sotto stati, ognuno dei quali è rappresentato un'esponenziale, in modo da markovianizzare il workflow e poterla trattare come una markov chain, che dal punto di vista matematico è molto ben trattabile.
# HyperExp
È una distribuzione che modella tempi di arrivo con alta variabilità, cioè dove $C_V>1$.
Graficamente la sua [PDF](random%20variable.md#PDF) ha [questa](hyperExp.jpeg) forma: all'inizio decresce più velocemente, ma col tempo decresce meno velocemente dell'esponenziale.
Si tratta di uno XOR di due esponenziali, ognuna definita dal suo rate e dalla sua probabilità: $X = \begin{cases}Exp(\lambda_1) & \text{con prob } p \\Exp(\lambda_2) & \text{con prob } 1-p\end{cases}$. Si modella quindi tramite un branch e un merge con un modello di [questo](modello%20hyperExp.jpeg) tipo.
Osserviamo che se $\lambda_1=\lambda_2$ allora si torna ad avere una [distribuzione esponenziale](distribuzione%20esponenziale.md) semplice con $C_V=1$.
# HypoExp
È una distribuzione che modella tempi di arrivo con bassa variabilità ma non troppo, cioè con $C_V\in[\tfrac{1}{\sqrt{2}},1]$: più $C_V\to\tfrac{1}{\sqrt{2}}$ e più $\lambda_1\to\lambda_2$; più $C_V\to1$ e più $\lambda_1>>\lambda_2$ (o viceversa). 
Graficamente la sua [PDF](random%20variable.md#PDF) ha [questa](hypoExp.jpeg) forma: si ha una moda; all'inizio si comporta in modo lineare; alla fine si comporta come un'esponenziale.
Si modella quindi come sequenza di esponenziali, cioè $X=Exp(\lambda_1)+\cdots+Exp(\lambda_n)$, con $\lambda_i\ne\lambda_j$, cioè con tassi diversi. Si modella tramite un modello di [questo](modello%20HypoExp.jpeg) tipo.
Osserviamo che se $\lambda_i=\lambda_j$ allora si ha una [Erlang](whitt%20approximation.md#Erlang).
# Erlang
È una distribuzione modale che modella tempi di arrivo con bassa variabilità, con $C_V<\tfrac{1}{\sqrt{2}}$; in particolare si può rappresentare dati per cui $C_V=\tfrac{1}{\sqrt{n}}$, dove $n$ è il numero di fasi della distribuzione.
La sua [PDF](random%20variable.md#PDF) è definita come $f(x)=\lambda e^{-\lambda x}\cdot\tfrac{\lambda x^{n-1}}{(n-1)!}$ e graficamente ha [questa](Erlang.jpeg) forma: all'inizio si comporta come un polinomio di grado $n-1$ e alla fine come un'esponenziale; la media è $mean=\tfrac{n}{\lambda}$ e la moda (il punto massimo) è $\tfrac{n-1}{\lambda}$. Se $n=1$ allora abbiamo un'esponenziale; se $n$ cresce si sposta e diventa simile a una gaussiana. Il parametro $\lambda$ invece determina la rapidità.
In particolare $Erl(n,\lambda)=Exp(\lambda)+\cdots+Exp(\lambda)$, cioè è una sequenza di esponenziali con lo stesso tasso. Si modella tramite un modello di [questo](modello%20Erl.jpeg) tipo.
# Generalized Erlang
È una distribuzione che modella tempi di arrivo con un coefficiente $C_V<\tfrac{1}{\sqrt{2}}$ che non può essere fittato da una [Erlang](#Erlang) perché non è esattamente $C_V=\tfrac{1}{\sqrt{n}}$, ma abbiamo $\tfrac{1}{\sqrt{n+1}}<C_V<\tfrac{1}{\sqrt{n}}$. È composta da una Erlang a $n$ fasi con tasso $\lambda_1$ e una esponenziale con tasso $\lambda_2$.
Osserviamo che se $C_v\ge\tfrac{1}{\sqrt{2}}$ allora si può usare e torna ad esere una [HypoExp](#HypoExp).
# HyperErl
È una distribuzione multimodale a rami, ognuno dei quali è definito da una Erlang. Permette di rappresentare situazioni con coefficiente di variabilità o alto o basso.
Graficamente la sua [PDF](random%20variable.md#PDF) ha [questa](hyperErl.jpeg) forma: ogni massimo è una moda; prima di arrivare al massimo cresce come un polinomio e dopo il massimo decresce come un'esponenziale. Per alzare una moda si aumenta il parametro $\lambda$ corrispondente; per stringere una moda si aumenta il numero di fasi.
Si modella con un modello di [questo](modello%20hyperErl.jpeg) tipo.