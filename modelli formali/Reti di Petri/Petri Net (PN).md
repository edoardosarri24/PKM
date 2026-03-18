Una rete di petri (PN) supporta la rappresentazione di un sistema concorrente, cioè un sistema dove più task potrebbero andare in esecuzione nello stesso momento (cioè dove nella coda dei processi ready ci sono più task).
È un modello non determinismo: a partire dallo stesso stato ci possono essere più esecuzioni diverse, a seconda delle scelte prese dal modello. Non è un modello stocastico come le [GSPN](generalized%20stochastic%20petri%20nets%20(GSPN).md), visto che non ci sono informazioni di probabilità.
# DEFINZIONE
### Sintassi
Si riferisce alla descrizione struttura del sistema, cioè alle regole formali che definiscono come il modello è costruito. È statica e definisce quindi gli elementi e le loro relazioni.
##### Petri Net
Data la tupla $\langle P,T,A^-,A^+\rangle$, una PN è un grafo diretto bipartito dove l'insieme dei vertici è $P\cup T$ e l'insieme degli archi è $A^-\cup A^+$.
- $P$ è l'insieme dei posti (cerchi) e $T$ è l'insieme delle transizioni (barre). Si ha che $P\cap T=\emptyset$ (perché il grafo è bipartito).
	- Un posto $p$ è detto posto di ingresso per una transizione $t$ se $(p,t)\in A^-$, cioè se c'è un arco da $p$ a $t$.
	- Un posto $p$ è detto posto di uscita per una transizione $t$ se $(t,p)\in A^+$, cioè se c'è un arco da $t$ a $p$.
	- [Esempio](posto%20di%20ingresso%20e%20di%20uscita%20PN.png).
- $A^-\subseteq P\times T$ è l'insieme delle precondizioni. In questo caso le frecce sono diretta dal cerchio alla barra.
- $A^+\subseteq T\times P$ è l'insieme delle postcondizioni. In questo caso le frecce sono diretta dalla barra al cerchio.
### Semantica
Si riferisce al comportamento del sistema, ovvero a cosa gli elementi evelvono nel tempo. È dinamica e descrive le regole che definisocono l'esecuzione del modello.
##### Marcatura
È una funzione $m:P\to\mathbb{N}$ che assegna un numero naturale di token (gettoni) a ogni posto.
- Si dice che i gettoni non hanno identità, cioè non hanno caratteristiche.
##### Transizione abilitata
Una transizione $t$ è abilitata da una marcatura $m$ se e solo se $m(p)>0, \forall p\in P|(p,t)\in A^-$, cioè se $m$ assegna almeno un gettone a ogni posto in ingesso di $t$.
- Si dice che più transizioni abilitate dalla stessa marcatura sono concorrenti.
##### Stato
Lo stato di una PN è un sigleton $s=\langle m\rangle$ dove $m$ è una marcatura.
- Una transizione è abilitata in uno stato $s$ se e solo se è abilitato dalla marcatura $m$.
- Una transizione abilitata viene attivata o disabilitata.
### Esecuzione
Un'esecuzione di una PN è il percorso $w=s_0\xrightarrow{\gamma_1}s_1\xrightarrow{\gamma_2}\cdots$, tale che:
- $s_0=\langle m_0\rangle$ è lo stato iniziale e $m_0$ è la marcatura iniziale.
- $\gamma_i$ è l'$i-esima$ transizione attivata. È selezionata tra le transizione abilitate nello stato $s_{i-1}=\langle m_{i-1}\rangle$. Dopo la sua attivazione la nuova marcatura $m_i$ è derivata da $m_{i-1}$ tramite due passi:
	- La rimozione di un token da ogni posto di ingresso di $\gamma_i$. Formalmente $m_{temp}=m_{i-1}(p)-1,\forall p|(p,\gamma_i)\in A^-$, dove $m_{temp}$ è la marcatura temporanea.
	- L'aggiunta di un token in ogni posto di uscita di $\gamma_i$. Formalmente $m_i=m_{temp}(p)+1,\forall p|(\gamma_i,p)\in A^+$, dove $m_i$ è la nuova marcatura.
- $s_i=\langle m_i\rangle$ è lo stato raggiunto dopo l'attivazione di $\gamma_i$.

Una PN può permettere esecuzioni finite o infinite. [Esempio](finite%20and%20infinite%20execution%20of%20PN%20example.pdf).
### Grafo di raggiungibilità
Definiamo prima l'insieme $\mathcal{R}(m_0)$ delle marcature che possono essere raggiunte dalla marcatura iniziale $m_0$ come il minimo insieme di marcature tale che: $m_0$ in $\mathcal{R}(m_0)$; se $m\in \mathcal{R}(m_0)\land \langle m\rangle\xrightarrow{t}\langle m'\rangle$ con $t\in T$ allora $m'\in\mathcal{R}(m_0)$.
Il grafo di raggiungibilità ([esempio](reachbility%20graph%20example.png) - [algoritmo per ottenerlo](reachbility%20graph%20algorithm.png)) è un grafo diretto tale che:
- L'insieme dei vertici è $\mathcal{R}(m_0)$.
- L'insieme degli archi include un arco da $m_i$ a $m_j$ se e solo se $\exists t\in T|\langle m_i\rangle\xrightarrow{t}\langle m_j\rangle$.

Osserviamo che ogni percorso nel grafo di raggiungibilità indica un'esecuzione.
# ESEMPIO PRODUTTORE E CONSUMATORE
Un produttore produce oggetti e li inserisce in un buffer con capacità limitata. Un consumatore consuma oggetti e li rimuove dal buffer.
##### Proprietà
- Safety
	Produttore e consumatore devono essere eseguiti in mutua esclusione.
- Liveness
	Produttore e consumatore prima o poi devono eseguire.
##### Soluzione
Si usano semafori binari per garantire la mutua esclusione. La [soluzione](consumer%20and%20producer%20with%20PN.pdf) viene proposta in modo incrementale.
# ESTENSIONE
Le PN possono essere estese in modo da aggiungere espressività: lo spazio dei modelli delle PN è meno ampio di quello delle PN estese.
Basta aggiungere uno dei dettagli che vedremo per aumentare l'espressività e raggiungere quella delle macchine di turing; l'aggiunta di più di un dettaglio non aggiunge maggior espressività, ma può comunque risultare utile per definire un modello più semplicemente.
### Archi inibitori
Solitamente è l'estensione che si usa: tramite gli archi inibitori si perde la proprietà di monotonia. Questa ci dice che aggiungendo gettoni non si possono diminuire le possibilità di firing; tramite gli archi inibitori questa proprietà non sparisce.
##### PN
Una PN con archi inibitori è una tupla $\langle P,T,A^-,A^+,A^{\bullet}\rangle$, dove:
- $P,T,A^-,A^+$ sono gli stessi elementi delle PN.
- $A^{\bullet}\subseteq P\times T$ è un arco inibitore (la direzione è quella del bullet).
##### Posti
Un posto $p$ è un posto inebitore per una transazione $t$ se $(p,t)\in A^{\bullet}$, cioè se c'è un arco inebitore da $p$ a $t$. [Esempio](PN%20with%20inhibitor%20arcs.png).
##### Transizione abilitata
Una transizione $t$ è abilitata da una marcature $m$ se e solo se:
- $m(p)>0,\forall p\in P|(p,t)\in A^-$, cioè se $m$ assegna almeno un token a ogni posto in ingesso di $t$. È la condizione delle PN.
- $m(p)=0,\forall p\in P|(p,t)\in A^{\bullet}$, cioè se $m$ non assegna alcun token a ogni posto inebitori di $t$.
- [Esempio](PN%20with%20inhibitor%20arcs%20example.png).
### Priorità
##### PN
Una PN con priorità è una tupla $\langle P,T,A^-,A^+,Z\rangle$, dove:
- $P,T,A^-,A^+$ sono gli stessi elementi delle PN.
- $Z:T\to\mathbb{N}$ è una funzione che associa a ogni transizione una priorità. La convenzione è la stessa dei sistemi Unix, dove naturale minore indica priorità maggiore. La priorità di una transizione viene inserita nel [grafico](PN%20with%20priority%20representation.png) sopra di essa.
##### Transizione abilitata
Una transizione $t$ è abilitata da una marcature $m$ se e solo se:
- $m(p)>0,\forall p\in P|(p,t)\in A^-$, cioè se $m$ assegna almeno un token a ogni posto in ingesso di $t$. È la condizione delle PN.
- $\forall t'\in T|t\ne t'$, se $m(p)>0,\forall p\in P|(p,t')\in A^-$ allora $Z(t)<Z(t')$, cioè $t$ deve avere la priorità più alta tra le transizione con posti in ingresso non vuoti (cioè tra le transizione abilitate nelle PN).
- [Esempio](PN%20with%20priority%20example.png).
### Enabling e update function
##### PN
Una PN con enabling e update function è una tupla $\langle P,T,A^-,A^+,E,U\rangle$, dove:
- $P,T,A^-,A^+$ sono gli stessi elementi delle PN.
- $E$ associa a una transizione $t\in T$ una enabling function $E(t):\mathcal{M}\to\{True,False\}$, dove $\mathcal{M}$ è l'insieme delle marcature raggiungibili. Nella pratica una enbaling function è una condizione sulla transizione; sono utili quando si hanno troppi archi e non si riesce a rappresentare una PN in maniera leggibile.
-  $U$ associa a ogni transizione $t\in T$ una update function $U(t):\mathcal{M}\to\mathcal{M}$. Nella pratica una update function è un modo algebrico per assegnare gettoni.
- [Esempio](PN%20with%20enabling%20and%20update%20funciona%20example.png)
##### Transizione abilitata
Una transizione $t$ è abilitata da una marcature $m$ se e solo se:
- $m(p)>0,\forall p\in P|(p,t)\in A^-$, cioè se $m$ assegna almeno un token a ogni posto in ingesso di $t$. È la condizione delle PN.
- $E(t)(m)=True$, cioè se l'enabling function di $t$ è valutata $True$ sotto la marcatura $m$.
##### Cambio marcatura
Data una marcature $m_{i-1}$ e una transizione $\gamma_i$ abilitata da $m_{i-1}$, la marcatura $m_i$ raggiunta dopo l'attivazione di $\gamma_i$ è derivata da $m_{i-1}$ tramite:
- La rimozione di un token da ogni posto di ingresso di $\gamma_i$. Formalmente $m_{temp}=m_{i-1}(p)-1,\forall p|(p,\gamma_i)\in A^-$, dove $m_{temp}$ è la marcatura temporanea. È la condizione delle PN.
- L'aggiunta di un token in ogni posto di uscita di $\gamma_i$. Formalmente $m_{temp2}=m_{temp}(p)+1,\forall p|(\gamma_i,p)\in A^+$, dove $m_{temp2}$ è un'ulteriore marcature temporanea. È la condizione delle PN.
- L'applicazione della update function di $\gamma_i$, cioè $m_i=U(m_{temp2})(\gamma_i)$.
- [Esempio](PN%20with%20enabling%20and%20undate%20function%20example%20cambio%20marcatura.png).