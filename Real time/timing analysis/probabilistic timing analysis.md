La probabilistic time analysis differisce dai [timing analysis](timing%20analysis.md) perché caratterizza il timing behaviour di un programma non sulla base di un singolo valore (i.e., [WCET](timing%20analysis.md#WCET)) ma su una distribuzione di probabilità (i.e., pWCET). L'obiettivo di queste analisi non è quindi dare il classico responce time del task ma una sua distribuzione di probabilità nel caso pessimo; per arrivare a questo si utilizza la [probabilistic responce time analysis](probabilistic%20responce%20time%20analysis.md).
In questo contesto il termine probabilistic non si riferisce alle tecniche usate per determinare il pWCET/pWCRT, ma solo al fatto che il risultato è una distribuzione e non un singolo valore; le tecniche usate infatti sono ancora deterministiche, cioè con uno stesso input otteniamo lo stesso output. È comodo rappresentare un parametro così complesso in modo statistico per [questo](stochastic%20process.md#Filosofia) motivo.
# Tecniche
Quando i tempi di esecuzione diventano variabili aleatorie e sono modellate dalla [pWCET](probabilistic%20timing%20analysis.md#pWCET) per definire la pWCRT ci sono solitamente due famiglie tecniche: analitiche e non analitiche.
##### Non analitiche
Queste tecniche vogliono calcolare la pWCRT a partire dalle pWCET in modo diretto.
Il problema di questi approcci è dato dal numero di job rilasciati all'interno dell'intervallo di tempo considerato, che solitamente è molto grande; questo porta ad avere una complessità di calcolo in termini di tempo e spazio, che se crescendo in modo esponenziale se risolto tramite la classica [convoluzione](statistica/convoluzione.md), diventa intrattabile.
Per utilizzare queste tecniche facendo in modo che siano trattabili si utilizzano tecniche come il Down-sampling, le simulazioni Monte Carlo e le di [stochastic TPN](stochastic%20TPN.md).
##### Analitiche
Queste tecniche voglio usare teoremi matematici e disuguaglianze per stimare delle forme chiuse dei bound del tempo di esecuzioneò.
Alcuni esempio sono i limiti di Chernoff, Bernstein e Hoeffding oppure una tecnica [pubblicata nel RTSS](analytic%20pWCRT%20with%20CLT%20di%20Lyapunov.md) (Real-Time Systems Symposium).
- Vantaggio
	Sono soluzioni efficienti che superano i problemi di intrattabilità per delle tecniche non analitiche.
- Svantaggi
	Il risultato è un'approssimazione dei bound, più o meno corretta della tecnica.

# pWCET
![pWCET](pWCET.png)
Si tratta di una distribuzione che esprime l'execution time nel caso peggiore, cioè quella distribuzione che sovrasta tutte quelle ottenute dall'esecuzione di vari scenari. È definito come il limite superiore delle distribuzioni dei tempi di esecuzione di ogni possibile scenario (i.e., curve diverse nella figura), cioè $pWCET=\displaystyle\sup_{\theta\in \Theta}\bar{F}_{\theta}$, dove $\bar{F}_{\theta}$ è $1-CDF$ per lo scenario $\theta$ e $\Theta$ è lo spazio dei possibili scenari.
Si rappresenta solitamente come $1-CDF$, una funzione detta exceedance funtion della pWCET: dato che la [CDF](random%20variable.md#CDF) è la probabilità che la random variable $X$ assuma un valore minore di una data soglia $x$, $1-CDF$ è la probabilità che la random variable assuma un valore maggiore di una data soglia. Questo è comodo in uno scenario [real time](real%20time.md) perché, dove il valore di $x$ è la dead line, possiamo dire che vogliamo che questa probabilità sia il più bassa possibile.
Nella figura si nota che pWCET può essere precisa, e questa può non corrispondere con nessuna delle CDF degli scenari presi in esame, oppure può essere un upper bound (solitamente il least uppur buond) di tutti gli scenari.
##### Scenario
Per ottenere questo pWCET non ci basiamo più sull'esecuzione di molti singoli run, ma sull'esecuzione di più scenari: uno scenario è una sequenza di run (teoricamente infinita) dove ogni run ha stti di input e stati dell'hardware diversi ma ottenuti con la stessa logica (e.g., campionati dalla stessa distribuzione); ciò che invece differenza gli scenari è la logica con sono scelti gli stati dell'input e dell'hardware.
# Measurement-based analysis
L'idea è la stessa della classica [measurement-based analaysis](timing%20analysis.md#Measurement-based%20analaysis), ma per ottenere il pWCET invece che il WCET si raccolgono i dati di più scenari e poi si utilizzano tecniche di Extreme Value Theory (EVT) per ottenere una stima statistica della distribuzione pWCET.
Gli scenari son decisi anche in questo caso da un measurement protocol; gli scenari decisi devono essere rappresentativi anche di casi futuri in modo che la pWCET sia rappresnetativa di tutte le posisbili situazioni.
##### EVT
La EVT ci permette di risolvere il problema di modella le code della distribuzione, cioè quelle parti dove finiscono pochi dei campioni ottenuti durante gli esperimenti.
Perché queste tecniche (e.g., Block Maxima e POT) funzionino servono delle supposizioni sulle misurazioni:
- IID (Indipendenti e Identicamente Distribuite)
	Nello scenario ideale qeusta proprietà deve valere: ogni esecuzione deve provenire dallo stesso scenario e le esecuzioni non devono essere correlate.
- Stazionarietà
	Se non si riesce a garantire la proprietà di IID, allora almeno i dati che hanno delle dipendenze devono essere stazionari, cioè la loro media e varianza non deve cambiare nel tempo, cioè non devono aumentare con il passare degli esperimenti.