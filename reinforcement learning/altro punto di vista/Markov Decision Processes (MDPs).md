Un MDP è alla base nel [Reinforcements learning](Reinforcements%20learning.md) e ci serve in quelle situazioni in cui abbiamo un agente e un ambiente che interagiscono in una sequenza (anche infinita) di passi $t=0,1,\cdots$.
Al tempo $t$ l'agente riceve un'osservazione $S_t$ ed esegue un'azione $A_t$; al tempo $t+1$ riceve la reward $R_{t+1}$ e si trova in un nuovo stato $S_{t+1}$. Questa interazione porta a una traiettoria del tipo $S_0,A_0,R_1,S_1,A_1,R_2,S_2,A_2,\cdots$.
##### Definizione
Un Markov Decision Process (MDP) formalemnte è una tupla $(S,A,p,R)$, dove:
- $S$ è l'insieme dei possibili stati dell'ambiente.
- $A$ è l'insieme delle possibili azioni dell'agente. Solitamente ci interessa l'insieme delle azioni dato uno stato, cioè $A(s)$.
- $p:S\times R\times S\times A\to[0,1]$, definita come $p(s',r|s,a)=Prob(S_t=s',R_t=r|S_{t-1}=s,A_{t-1}=a)$, è una distribuzione che caratterizza completamente le dinamiche dell'ambiente; in sostanza è la probabilità di ottenere $S_t$ e $R_t$ data la conoscenza dello stato e dell'azione all'istante di tempo precedente; non serve conoscere tutta la traiettoria.
	- A partire da questa possiamo calcolare cose interessanti come la probabilità di trovarci nello stato $s'$ dato s $e$ $a$ all'istante di tempo precedente come $p(s'|s,a)=Prob(S_t=s'|S_{t-1}=s,A_{t-1}=a)=\displaystyle\sum_{r\in R}p(s',r|s,a)$.
- $R\subseteq\mathbb{R}$ è l'insieme dei reward.
# Reward
Il nostro scopo è massimizzare l'expected reward lungo la traiettoria definita dall'agente, cioè i returns. È importante che i vari reward facciano capire all'agente cosa fare e non come farlo; il come farlo, detto reward reshaping, deve essere limitato il più possibile, altrimenti stiamo programmando il comportamento.
##### Expected return
La traiettoria dell'agente può essere finita, e allora si parla di episodio, o infinita.
- Nel caso finito possiamo definire l'expected return come $G_t=R_{t+1}+\cdots+R_{T}$.
- Nel caso infinito possiamo usare la ricorsione e usare degli sconti sulle reward: $G_t=R_{t+1}+\gamma R_{t+2}+\gamma^2R_{t+3}+\cdots=\sum_{k=0}^\infty\gamma^kR_{t+k-1}$ e questo si può vedere in modo ricorsivo come $G_t=R_{t+1}+\gamma(R_{t+2}+\gamma R_{t+3}+\cdots)=R_{t+1}+\gamma G_{t-1}$. Il fattore $\gamma$ definisce la lungimiranza dell'agente: se $\gamma=0$ allora l'unica reward che conta è quella immediata; se $\gamma=1$ allora tutte le reward contano nello stesso modo.
# Policy
Una policy (strategia) è definita come $\pi(a|s)$, ed è la distruzione delle azioni nello stato $s$, cioè indica la probabilità di scegliere un'azione quando siamo in un dato stato.
# Value function
##### State value function
È definita come $v_\pi(s)=\mathbb{E}_\pi[G_t|S_t=s]$ ed è il valore atteso della sequenza di return (l'expected return definito sopra) al tempo $t$ dato che siamo nello stato $s$ e stiamo seguendo la policy $\pi$.
In pratica indica la qualità di uno stato, cioè quanto conviene all'agente trovarsi in quello stato seguendo la policy $\pi$.
##### Action value function
È definita come $q_\pi(s,a)=\mathbb{E}_\pi[G_t|S_t=s, A_t=a]$ ed è il valore atteso della sequenza di reward (l'expected return definito sopra) al tempo $t$ dato che siamo nello stato $s$, abbiamo eseguito l'azione $a$ e stiamo seguendo la policy $\pi$.
In pratica indica la qualità di una coppia stato-azione, cioè quanto conviene all'agente eseguire quell'azione trovandosi in quello stato seguendo la policy $\pi$.
# Equazione di Bellman
L'equazione di Bellman è alla base del [reinforcement learning](Reinforcements%20learning.md). Non è altro che l'esplosione della state value function: $v_\pi(s)=\displaystyle\sum_a\pi(a|s)\sum_{s'}\sum_rp(s',r|s,a)[r+\gamma v_\pi(s')],\forall s\in S$.
Stabilisce una relazione ricorsiva in cui il valore in uno stato dipende dai valori degli stati successivi: la value function $v_\pi(s)$ non viene calcolata direttamente ma è espressa in termini degli stati futuri. 
##### Esempio
Immaginiamo che un robot possa muoversi tra più stanze e riceva +10 se arriva in una stanza con una batteria e 0 altrimenti.
Se il robot è nella stanza $s_1$ e può andare in $s_1$ (che ha la batteria), allora il valore di $v_\pi(s_1)$ dipende dalla probabilità di andare in $s_2$, dalla ricompensa immediata e dal valore di $s_2$.
# Ottimalità
Il nostro scopo è massimizzare l'expected return. Siccome questi sono ottenuti dopo aver scelto un'azione in un dato stato, ci serve una policy che permetta di scegliere una buona azione quando siamo in uno stato. In pratica vogliamo determinare $\pi_*$ che massimizza la sequenza di reward.
La maggior parte dei problemi di reinformcement learning si possono ricondurre nell'utilizzo delle due value function $v(s)$ e $q(s,a)$ per trovare $\pi_*$. I metodi per trovare $v_*$ e $q_*$ sono principalmente due:
- Conoscendo perfettamente l'ambiente. In questo caso si utilizzano tenciche di [dinamic programming](reinforcement%20learning/altro%20punto%20di%20vista/Dinamic%20programming.md).
- Tramite l'esperienza. In questo caso si usano tecniche come i metodi [Monte Carlo](Metodi%20Monte%20Carlo.md) e [temporal difference learning](Temporal%20Difference%20Learning.md).
##### State value function
Le state value function permettono di imporre un ordine di ottimalità tra le varie policy: per ogni stato $s$ si ha $\pi\ge\pi'\Leftrightarrow v_\pi(s)\ge v_{\pi'}(s)$.
Questo ci pemette di valutare due policy tramite la valutazione della state value function in un dato stato. Avremo allora $v_*(s)=\displaystyle\max_\pi v_\pi(s),\forall s\in S$.
##### Action value function
La stessa cosa vale per le action value function. Allora abbiamo che $q_*(s,a)=\displaystyle\max_\pi q_\pi(s,a),\forall s\in S,a\in A$.
##### Policy ottima
- Se consociamo $v_*$ allora è relativamente facile derivare $\pi_*$. Per ogni stato $s$, dobbiamo scegliere l’azione a che massimizza il ritorno futuro previsto tra tutte le azioni possibili. Questo vuol dir che dobbiamo assegnare una probabilità non nulla solo all'azione che massimizza l'equazione di Bellman (Bellman Optimality Eqaution), cioè all'azione $a^*=\displaystyle\max_a( \sum_{s{\prime}} \sum_r p(s',r|s,a)[r + \gamma v(s')])$.
- Stesso ragionamento lo possiamo fare se conosciamo $q_*$. Per ogni stato $s$, qualsiasi politica $\pi$ che assegni probabilità non nulla solo all'azione che massimizza $q_*(s, a)$ sarà ottimale.