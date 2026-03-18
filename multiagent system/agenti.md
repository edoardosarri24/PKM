Un agente, che può essere sia fisico che virtuale, è qualunque cosa possa interagire con l'ambiente: ricevere informazioni da esso (e.g., sensori); agire per modificarlo (e.g, attuatori).
# Intelligenti
Un agente è definito essere intelligente quando ha le seguenti proprietà:
- Autonomo
	Deve poter agire senza un intervento esterno.
- Goal-oriented
	Deve avere un obiettivo specifico sulla base del quale prendere decisioni.
- Adattivo
	Deve essere capaci di adattarsi anche a cambiamenti dell'ambiente imprevisti.
- Robusto
	Deve mantenere le sue funzionalità anche in caso di perturbazioni o incertezza (e.g., l'ambiente non sempre è totalmente noto).
- Capace di learning
	Deve imparare dell'esperienza passata.
# Obiettivo
L'obiettivo di un agente intelligente è in realtà un multi obiettivo; si deve trovare un trade-off. Gli obiettivi in generale hanno queste caratteristiche:
- Un orizzonte temporale $T$ che può essere finito o infinito.
- Un insieme $X_G\subset\mathbb{X}$ di configurazioni desiderate; definisce una specifica di reachability. Queste sono le configurazioni goal, cioè quelle che l'agente deve raggiungere: $x(t)\in X_G(t),\exists t\in[0,T)$. Il punto di partenza $x(0)$ dell'agente è detto invece start.
- Un insieme $X_A\subset\mathbb{X}$ di [configurazioni ammissibili](spazi.md#Free%20and%20obstacle%20space); definisce una specifica di safety. Queste sono le configurazioni in cui l'agente si può trovare in ogni momento: deve essere $x(t)\in X_A,\forall t\in[0,T)$. Il complementare $\mathbb{X}-X_a$ è detto insieme di configurazioni indesiderate o proibite.
- Una funzione obiettivo, cioè un [return](modello.md#Reward%20e%20return), da ottimizzare.
# Multi agente
Un sistema multi agente è un sistema nel quale sono presenti più agenti intelligenti.
Ogni agente in generale è individualista, cioè sono implementati per ottimizzare il loro obiettivo. In base a come l'obiettivo è definito possiamo avere scenari:
- Competitivi
	Ogni agente massimizzare la propria reward, ma questa è progettata per essere in conflitto con quella degli altri.
	Questo vuol dire che, ad esempio per $N=2$, il [modello](modello.md) che descrive gli agenti sarà dato da $X_{g,1}\cap X_{G,2}=\emptyset$ e $r_1=-r_2$.
- Collaborativi
	Gli agenti massimizzano una reward comune a tutti.
	Questo vuol dire il modello sarà che $X_{G,i}=x_G$, $X_{A,i}=X_A$ e $r_i=r$.
##### Modello di osservazione
Il [modello di osservazioni](modello.md#Observation%20model) nel caso multiagente è più complesso e permette la costruzione di una rete o un [grafo](grafi.md).
Gli agenti prendono le decisioni individualmente sulla base delle proprie informazioni, che spesso non sono globali, cioè $u_i=\pi_i(t,I_i(t))$, dove $\pi_i$ è la [policy](multiagent%20system/robot%20motion%20planning/policy.md) dell'$i$-esimo agente e $I_i(t)$ è l'osservazione dell'agente $i$-esimo al tempo $t$. Ogni agente ha il proprio information vector $I_i$, che include le sue osservazioni e azioni passate. Ogni agente può avere dei vicini, cioè degli agenti di cui può conoscere lo stato (i.e., con cui può comunicare); questo stato può essere incluso nell'information vector, cioè $I_i=\begin{bmatrix}h(x_i(t))\\x_j(t),\forall j\in N_i\end{bmatrix}$, dove $N_i$ è l'insieme dei vicini dell'agente $i$-esimo.