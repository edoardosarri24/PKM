Vediamo l'algoritmo del [consenso](legge%20del%20consenso.md) a tempo discreto. Questo deriva dalla versione a tempo [continuo](consenso%20a%20tempo%20continuo.md).
# Definizione
La legge del consenso a tempo continuo sappiamo essere $\dot{x}_i(t)=\displaystyle\sum_{j\in N_i}(x_j(t)-x_i(t))$.
Tramite una serie di [manipolazioni](consenso%20a%20tempo%20discreto.md#Manipolazioni%20algebriche) si arriva alla legge del consenso a tempo discreto che è definita come $x_i(t+1)=\pi_{ii}x_i(t)+\displaystyle\sum_{j\in N_i}\pi_{ij}x_j(t)$, dove:
- $\pi_{ii}>0$
	È il peso associato allo stato locale.
- $\pi_{ij}>0,\ \forall j\in N_i$
	Sono i pesi associati agli stati dei vicini.
- $\displaystyle\pi_{ii}+\sum_{j\in N_i}\pi_{ij}=1$
	Proprietà che definisce la somma a 1 dei pesi.
##### Significato
L'interpretazione che possiamo dare all'algoritmo è che l'azione presa dall'$i$-esimo agente per aggiornare il proprio stato è data da una combinazione convessa (i.e., una media pesata dove i pesi devono essere positivi e sommare a 1, cioè devono godere delle proprietà di cui sopra) tra il proprio stato locale e quello dei vicini.
##### Manipolazioni algebriche
Vediamo le manipolazioni che ci portano alla legge a tempo discreto da quella a tempo continuo.
- Usando il rapporto incrementale abbiamo che $\dot{x}_i(t)=\tfrac{d}{dt}x_i(t)\approx\tfrac{x_i(t+1)-x_i(t)}{\epsilon}$.
- Sostituendo $\dot{x}_i(t)$ con quanto avevamo all'inizio abbiamo $\displaystyle\tfrac{x_i(t+1)-x_i(t)}{\epsilon}=\displaystyle\sum_{j\in N_i}[x_j(t)-x_i(t)]$.
- Possiamo derivare $x_i(t+1)=x_i(t)+\epsilon\displaystyle\sum_{j\in N_i}[x_j(t)-x_i(t)]$.
- Siccome stiamo [supponendo](legge%20del%20consenso.md#Supposizione) di essere nella dinamica a singolo integratore, abbiamo che $u_i(t)=x_i(t+1)=x_i(t)+\epsilon\displaystyle\sum_{j\in N_i}[x_j(t)-x_i(t)]$.
- Srotolando il calcolo e raccogliendo abbiamo che $u_i(t)=\displaystyle[x_i(t)-\epsilon\sum_{j\in N_i}x_i(t)]+\sum_{j\in N_i}\epsilon x_j(t)$.
- Da questo deriva $\displaystyle u_i(t)=(1-\epsilon d_i)x_i(t)+\sum_{j\in N_i}\epsilon x_j(t)$.
- Possiamo scrivere la policy come $u_i(t)=\pi_{ii}x_i(t)+\displaystyle\sum_{j\in N_i}\pi_{ij}x_j(t)$, dove:
	- $\pi_{ii}=1-\epsilon d_i$, con $d_i$ [grado](grafi%20indiretti.md#Grado) dell'agente $i$-esimo.
	- $\pi_{ij}=\epsilon,\ \forall j\in N_i$.
# Matrice del consenso
Se riscriviamo la legge del consenso a tempo discreto in forma matriciale abbiamo $x(t+1)=\Pi x(t)$.
La matrice $\Pi=\begin{cases}\pi_{ij}&\text{ if }i=j\lor j\in N_i\\0&\text{ otherwise }\end{cases}$ è detta matrice del consenso.
##### Algoritmo
Con questa matrice l'algoritmo del consenso a tempo discreto può essere definito come $x(t)=\Pi^tx(0)$.
Intuitivamente abbiamo che l'informazione al tempo 0 è solo quella locale; al tempo 1 è data da quella locale e quella dei vicini; al tempo 2 è arrivata l'informazione dei vicini dei vicini; si ripete finché non arriva tutta l'informazione.
##### Proprietà
- Matrice stocastica
	Una matrice è stocastica se tutti gli elementi sono non negativi e le righe sommano a 1.
	La matrice del consenso lo è per costruzione.
- Doppiamente stocastica
	Una matrice è doppiamente stocastica se è stocastica e anche le colonne sommano a 1.
	Quindi una matrice è doppiamente stocastica se è stocastica e simmetrica.
##### Pesi laplanciani
La matrice del consenso può essere ottenuta $\Pi=I-\epsilon L$, dove $L$ è il [laplanciano](laplanciano.md) del grafo associato al sistema. I pesi definiti dalla matrice del consenso ottenuti in questo modo sono detti pesi laplanciani.
[Ricordando](laplanciano.md#Proprietà) che $L$ è simmetrica per un grafo indiretto, allora abbiamo che $\Pi$ ottenuta in questo modo è doppiamente stocastica.
# Convergenza
Se un sistema multi agente definisce un grafo connesso indiretto e si applica la legge del consenso a tempo discreto allora:
- Per ogni condizione iniziale $x(0)$ abbiamo $x_i(t)\to_{t\to\infty}\bar{x},\ \forall i=1,\cdots,N$.
- Se $\Pi$ è doppiamente stocastica allora $\bar{x}=\tfrac{1}{N}\sum_{i=1}^Nx_i(0)$.
##### Osservazioni
- Riusciamo sempre a ottenere la sincronizzazione in un qualche punto.
- Se la matrice del consenso $\Pi$ è doppiamente stocastica allora ci sincronizziamo alla media. Questo è garantito se scegliamo i pesi laplanciani, dove $\epsilon\in(0,d_{max})$ con $d_{max}$ grado massimo dei nodi del grafo del sistema.
##### Scelte dei pesi
La difficoltà di usare i pesi laplanciani è che ogni agente deve conoscere il grado massimo del sistema; questa è un'informazione globale che potrebbe non essere nota a tutti gli agenti. Ci sono altri modi per scegliere i pesi:
- Uniformi
	Si definisce $\Pi_{ij}=\begin{cases}\tfrac{1}{d_i+1}&\text{ if }j=i\lor j\in N_i\\0&\text{ otherwise}\end{cases}$. Si usano solo informazioni locali, ma il problema in questo caso è che la matrice del consenso definita in questo modo non è doppiamente stocastica (perché non è simmetrica): se ci serve solo la sincronizzazione allora ok, ma altrimenti non va bene.
- Metropolis
	Si definisce $\Pi_{ij}=\begin{cases}\tfrac{1}{max\{d_i,d_j\}+1}&\text{ if }j\in N_i\\1-\displaystyle\sum_{k\in N_i}\pi_{ik}&\text{ if }k=i\\0&\text{ otherwise}\end{cases}$. In pratica per ogni cella della matrice (tranne quelli sulla diagonale) prendo il suo opposto e lo setto come il più piccolo dei due; faccio poi il complemento a 1 per gli elementi diagonali in modo che la riga sommi a 1. Questo è fattibile con solo le informazioni locali.
	Garantisce la simmetria e quindi la doppia stocasticità della matrice $\Pi$.
# Grafi diretti
Nei [grafi diretti](grafi%20diretti.md) i vicini del nodo $i$-esimo sono tutti i nodi che un singolo hop riescono a inviare informazioni a nodo $i$-esimo; questo vuol dire che $N_i=\{j\in N:(j,i)\in E\}$.
##### Convergenza
La convergenza nei grafi diretti dipende dalla presenta di un nodo leader, cioè il grafo deve essere [quasi fortementemente connesso](grafi%20diretti.md#Connessione) e non ci basta la connettività debole.
##### Teorema di convergenza
Se un sistema multi agente definisce un grafo diretto quasi strongly connected e si applica la legge del consenso a tempo discreto allora:
- Per ogni condizione iniziale $x(0)$ abbiamo $x_i(t)\to_{t\to\infty}\bar{x},\ \forall i=1,\cdots,N$.
- Il valore alla sincronizzazione è $\bar{x}=\sum_{i=1}^Nw_ix_i(0)$, dove $w_i>0$ se e solo se $i$ è un nodo leader. Osserviamo che il valore di sincronizzazione è una media rispetto ai pesi dei nodi leader.