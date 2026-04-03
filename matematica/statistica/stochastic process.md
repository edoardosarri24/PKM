Un processo stocastico $\mathbb{M}=\{M(t), t\in T\}$ è una collezione di [variabili aleatorie](random%20variable.md) che descrivono come un sistema evolve nel tempo. È definito da:
- Uno spazio degli stati $M(t)$, che rappresenta l'insieme dei valori che le variabili aleatorie possono assumere.
- Il parametro $t$ che solitamente rappresenta il tempo. Questo parametro assume valori in un insieme che è totalmente ordinato.
##### Esempio
Se consideriamo i processi stocastici nell'ambito delle [GSPN](generalized%20stochastic%20petri%20nets%20(GSPN).md), allora l'insieme $M$ è l'insieme delle marcature.
# Spazio e tempo Continuo/Discreto
![spazio e tempo continuo e discreto](spazio%20e%20tempo%20continuo%20e%20discreto.png)
Un processo stocastico può essere definito in uno spazio continuo o discreto a seconda se $M$ è continuo o discreto e può essere definito a tempo continuo o discreto a seconda se T è continuo o discreto.
Siccome ueremo questi concetti nel campo delle GSPN, a noi interessa lo spazio discreto, visto che l'insieme delle marcature sono discrete. Per il tempo ci può invece interessare tutti e due i casi: un esempio di processo stocastico a tempo continuo è definire la marcatura al tempo $t$; un esempio di processo stocastico a tempo discreto è la marcatura dopo $n$ transizioni.
##### Right continous
Se il processo stocastico è continuo allora si suppone essere right continous.
Facendo l'esempio con le GSPN, questo vuol dire che se al tempo $t$ si cambia stato, allora lo stato al tempo $t$ (che potrebbe essere ambiguo) è quello uguale a tempo $t+\epsilon$ (limite destro).
# Astrazioni
Quando definiamo un model run in una GSPN (cioè quello che succede effettivamente in un run), possiamo astrarlo in due modi: usando il tempo continuo e il tempo discreto. È importante osservare che l'astrazione discreta del model run non è la stessa cosa dell'[embedded chain](stochastic%20process.md#Embedded%20chain), cioè della rappresentazione discreta dell'astrazione continua.
Sono entrambi fondamentali per descrivere una GSPN perché uno si può perdere informazioni che l'altro invece mostra ([esempio](astrazione%20model%20run.png)):
- Con il tempo discreto posso dire in quale marcatura ci troviamo dopo $n$ transizioni, ma non posso dire quanto tempo ci è voluto per raggiungere quella marcatura.
- Con il tempo continuo posso dire in quale marcatura ci troviamo dopo un tempo $t$, ma non posso osservare quelle marcature in cui sono stato un tempo nulla (posti in ingresso di transizioni immediate).
# Embedded chain
Un processo stocastico a tempo continuo può essere difficile da analizzare, perché il tempo tra due eventi significativi è appunto continuo. Le embedded chian sono una sequenza di istanti di tempo discreti in cui avvengono gli eventi importanti; è un modo per trasformare una [CTMC](continuous%20time%20markov%20chain%20(CTMC).md) in una [DTMC](disctete%20time%20markov%20chain%20(DTMC).md).
Formalmente se $\mathbb{X}=\{X(t),t\in\mathbb{R}_{\ge0}\}$ è un processo a tempo continuo, allora definita la embedded sequence $\{t_n\}_0^\infty$ dove $t_n\in\mathbb{r}_{\ge0}$ con $t_i\ge t_{i-1}$, si ha che la embedded chain è $\mathbb{X}^e=\{X(t_n), n\in\mathbb{n}\}$.
- Data una [CTMC](continuous%20time%20markov%20chain%20(CTMC).md), esistono infinite embedded chain e queste dipendono dalla scelata dalla seguenza dei tempi $\{t_n\}_0^\infty$.
##### GSPN
Nel nostro caso delle [GSPN](generalized%20stochastic%20petri%20nets%20(GSPN).md) è difficile analizzare un processo stocastico perché il tempo tra due transizioni è continuo. Possiamo quindi (come in questo [esempio](embedded%20chian.png)) prendere in considerazione solo gli istanti di tempo in cui avvengono le transizioni; in questo caso i tempi della embedded sequence sono quelli in cui avvengono le transizioni.
Si potrebbe anche campionati tempi equidistanti, ma in questo caso di potrebbero avere informazioni ridondanti (per più istanti di tempo discreto si ha la stessa marcatura) o perdere informazioni (tra due istanti di tempo discreti si cambiano più marcature).
# Filosofia
La scelta di rappresentare alcuni parametri in modo probabilistico non è dovuto al fatto che tali parametri abbiano nella realtà un comportamento casuale, ma che sono talmente complessi che è il modo più semplice di trattarli, cioè è il modo migliore che al momento abbiamo per rappresentare in modo matematico un sistema molto complesso.
Se consideriamo per esempio il lancio di una moneta, questo è molto deterministico: il risultato dipende da forse a attriti che possono essere ottenuti, ma il sistema è talmente complesso che ci affidiamo a una probabilità $p=0.5$ per descrivere l'evento.