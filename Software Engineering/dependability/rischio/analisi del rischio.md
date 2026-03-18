L'analisi del rischio è la prima fase della pipeline per la [gestione del rischio](gestione%20del%20rischio.md).
Studia l'utilizzo del sistema, le performance e si rappresentano i vari rischi.
 Ci sono tre metodi convenzionali per combinare i [parametri](#Parametri) che definiscono il rischio. Queste sono diverse, rappresentano aspetti diversi e devono essere scelte bene.
 - Indici di rischio
	Sono dati medi in forma tabellare, molto semplici e che spesso non rappresentano bene un sistema complesso.
 - Rischio puntuale
	Correlano il rischio a un preciso punto geografico. Ci sono diverse tecniche: rischio locale, dove il target è sempre presente e non via di fuga o protezione; rischio individuale, dove il target è capace di muoversi e usare protezioni.
 - Rischio diffuso
	L'idea è la stessa del rischio puntale ma tiene in considerazione anche la tipologia del target, la presenta di target particolarmente vulnerabili.
# Parametri
Quando si rappresenta il rischio dobbiamo sempre considerare due aspetti.
##### Qualitativo
Stiamo valutando una misura qualitativa dell'effetto del danno basandoci su chi subisce tale effetto.
A seconda di chi subisce l'effetto l'osservazione sarà diversa: un sistema può essere distrutto, una persona no, l'ambiente è una via di mezzo.
Si può fare un'analisi anche per momento in cui l'effetto del danno agisce. Possiamo poi classificare gli effetti del danno.
##### Quantitativo
Stiamo valutando i livelli di gravità e la loro probabilità secondo la norma ISO 14971.
- Gravità
	In ordine abbiamo: catastrofico, che causa il decesso; critico, che compromette permanentemente; grave, che causa una lesione che richiede un intervento; minore, che causa una lesione che non richiede intervento; trascurabile, che causa disagio.
- Probabilità
	Nelle nuove norme (e.g., 14971) la probabilità che poi da vita al rischio non è un singolo valore, ma viene definita pesando due fattori: il primo rappresenta la probabilità che avvenga un guasto e questa si può stimare da valori statistici, cioè da dati storici; la seconda è la probabilità che tale guasto porti al danno e questa è molto complessa da stimare perché dipende da variabili non controllabili.
	In ordine abbiamo: frequente, $\ge 10^{-3}$; probabile, $10^{-3}\le x\le 10^{-4}$; grave, occasionale, $10^{-4}\le x\le 10^{-5}$; remoto, $10^{-5}\le x\le 10^{-6}$; improbabile, $\le 10^{-6}$. In realtà i valori in cui rientrano le categorie di probabilità dipendono anche dal dominio: per sistemi elettronici/meccanici è circa $10^{-6}$ per farmaci è circa $10^{-10}$.
	La probabilità non sempre è facile da capire; si utilizzano dati storici ed esperienza di chi valuta.
# Tecniche
Le tecniche per eseguire l'analisi del rischio sono molte e dipendono dal contesto e dai dati inizialmente a disposizione.
Ci sono due classi:
- Induttivo
	Si parte dalle cause e si determino le conseguenze.
- Deduttivo
	Si parte dalle conseguenze e si arriva alle cause che le hanno generate.
##### PHA (Preliminary Hazard Analysis)
È una tecnica qualitativa e induttiva usata nelle prime fasi di progettazione e permette di individuare i possibili rischi e risolverli prima che diventino un problema, quando i costi per risolverli sono ancora bassi. Si tratta di una tecnica semplice che si svolge in pochi giorni anche per sistemi complessi.
Si parte dal top event, cioè l'evento indesiderato principale; si stimano le sue conseguenze e a ognuna di questa gli viene assegnato un livello (i.e., trascurabile, marginale, critico e catastrofico); i risultati vengono riassunti in una tabella di [questo](PHA%20tabel.png) tipo, molto descrittiva, qualitativa e che rimane molto generica.
##### HAZOP (Hazard and Operability Studies)
È definita dalla norma IEC 61882, qualitativa e induttiva usata soprattutto in sistemi complessi.
Il vantaggio è che si ottiene un'analisi completa: non si guardano solo i componenti (nodi interni), ma anche le relazioni tra essi (nodi di frontiera). Lo svantaggio è che rispetto a PHA è molto complessa e richiede tempo e molto personale.
- Prima di analizzare i guasti si deve mappare il sistema: si suddivide in nodi: i nodi interni sono componenti del sistema che svolgono il loro compito all'interno di un perimetro ben definito (e.g., serbatoio o un modulo software isolato); i nodi di frontiera componenti che interagiscono con l'esterno (e.g., valvola o API software).
- Per ogni nodo si deve definire cosa dovrebbe fare usando dei parametri, i quali dipendono da applicazione in applicazione (e.g., per un software latenza, throughtput, input): i parametri sono in pratica le funzioni che il nodo svolge.
- Si associa una keyword a ogni parametro per ottenere delle deviazioni, cioè un comportamento indesiderato di un parametro. Le keyword sono definite dallo standard: NO, PIÙ, DIVERSO, PRIMA, DOPO. 
- Per ogni deviazione si deve compilare una tabella che descrive la catena deviazione-causa-conseguenza-rimedio: la causa della devizione, cioè perché è successo; la consegue al sistema; il rimedio che abbiamo già implementato o che dobbiamo fare.
##### ETA (Event Tree Analysis)
È una tecnica induttiva e quantitativa definita dalla norma IEC 62502.
Si basa su un albero binario: a partire da un evento iniziale si cerca di mettere delle barriere (i.e., delle guardie per arginare il problema); il ramo superiore rappresenta il successo della barriera con una certa probabilità $P(A)$, il ramo inferiore il fallimento con probabilità $1-P(A)$; percorrendo l'albero e attraversando le possibili barriere si arriva alle foglie dove sono rappresentate le possibili conseguenze. Possiamo in questo modo calcolare la probabilità di ogni singola conseguenza usando la probabilità congiunta, cioè moltiplicando le probabilità del percorso.
Il vantaggio è che possiamo vedere chi ha salvato il sistema, capire quanto è il rapporta beneficio/costo di aggiungere una barriera ed è semplice calcolare le probabilità di finire in una conseguenza. Lo svantaggio è che per ogni evento iniziale si deve costruire un albero a parte.
##### [FMEA](FMEA.md)
##### [FMECA](FMECA.md)
##### [FTA](FTA.md)