La Failure Modes and Effects and Criticality Analysis (FMECA) è una tecnica per l'[analisi del rischio](analisi%20del%20rischio.md), che rispetto alla [FMEA](FMEA.md) aggiunge un'analisi delle criticità (dopo l'[analisi degli effetti](FMEA.md#Fasi)), che mi permette di dare un peso ai modi di guasto e agli effetti che questi hanno e quindi la rendere semi-quantitativa. 
L'analisi delle criticità deve essere svolta su: persone che interagiscono con il sistema, il sistema stesso e l'ambiente in cui il sistema esegue.
# Parametri di rischio
Alla tabella vista per la FMEA, si devono [aggiungere](tabella%20effetti%20FMECA.png) delle colonne. Ognuno di questi valori ha un range tra 1 e 10.
- Occurrence (O)
	È la probabilità che si verifichi il guasto durante la vita del sistema. È strettamente legato al tasso di guasto, con l'idea che ogni modo di guasto avrà il suo peso.
	I livelli di occurence possono essere trovati all'interno di una tabella della norma, dove per ogni livello ci sono dei range quantitativi come la frequenza o il tasso di guasto; per questo è un parametro molto oggettivo.
- Severty (S)
	È la gravità del guasto e rappresenta gli effetti, locali e globali al sistema, del guasto.
	I livelli di severity possono essere trovati all'interno di una tabella della norma, dove per ogni livello c'è una descrizione in linguaggio naturale; per questo è un parametro un po' soggettivo.
- Detection (D)
	È la possibilità di rilevare il modo di guasto prima che si verifichi un effetto significativo.
	I  livelli di severity possono essere trovati all'interno di una tabella della norma.
##### Risk priority number
Questo parametro è il risultato dei tre sopra e viene calcola come $RPN = S \times O \times D$; ha quindi un valore compreso tra 1 e 1000.
Non tratta quindi il [rischio](rischio.md#Rischio%20(risk)) nel modo classico (i.e., effetto per probabilità che si verifichi) ma attribuisce un peso ai singoli modi di guasto per prioritizzare quelli più significativi.
# Azioni
Dopo aver calcolato il $RPN$, l'idea è quella di agire sulle componenti più critiche eseguendo azioni di [controllo del rischio](gestione%20del%20rischio.md#Controllo%20del%20rischio) per mitigare il rischio. Dopo aver esegue queste azioni si deve ricalcolare l'$RPN$ per vedere se hanno avuto successo.
##### Occurrence
Il modo più semplice di limitare il $RPN$ è lavorare sull'occurrence. Ci sono quattro modi per falro:
- Manutenzione preventiva.
- Usare componenti di qualità migliore. In questo modo si abbassa il tasso di quasto.
- Utilizzare derating.
- Ridondanza.
##### Detection
Possiamo ridurre il $RPN$ agendo monitorando di più il sistema e quindi migliorando la diagnostica.
Il problema è che diminuire il valore della detection probabilmente aumentare i costi e il tasso di guasto (perché si aggiungono componenti) e quindi si peggiora l'occurrence.
# Conseguenze
Vediamo vantaggi e svantaggi della FMECA
##### Vantaggi
- Migliora la dependability del sistema.
- Determina la necessità di ridondanze, il livello di utilizzo del derating o semplificazione del progetto.
- Documenta i rischio del sistema e dei singoli componenti.
- Si stabiliscono le attività di manutenzione.
##### Svantaggi
- Il valore $RPN$ va da 1 a 1000, ma in raltà ha valori molto sparsi: ad esempio da 900 ($10^2\times{9}$) a 1000 ($10^3$) non ha valori; in pratica non abbiamo 1000 valori ma solo 120. Si ottengono gli stessi valori di $RPN$ con combinazioni diverse. Per risolvere questi problemi ci sono delle soluzioni (quasi mai migliori in ambito industriale):
	- $ARPN$: è un'alternativa al $RPN$ dove si sommano i valori invece che moltiplicarli. Risolve il problema dei salti ma non delle combinazioni.
	- $URPN$: utilizza l'esponenziale nella severity, dando molta più importanza rispetto alle altre.
	- $LRPN$: usa il logaritmo dei tre parametri.
	- Range: Si usa il modello normale, ma il range dei valori è da 1 a 5 in modo di limitare i possibili valori del $RPN$ e buchi.
- Non è facile identifica un valore soglia dell'$RPN$: devo abbassare quei valori troppo alti, ma per quali valori questo succede? Questo valore soglia dipende dall'applicazione, ma non è facile da determinare e introduce ancora una volta soggettività, motivo per cui quest'analisi viene fatta in gruppo. Per eliminare la soggettività possiamo:
	- Bluvband
		Si graficano le coppie $(\text{modi di guasto,RPN})$ e in questo grafio si dovrebbero vedere due pattern: all'inzio la derivata del livello di $RPN$ è più piccola (i.e., la curva è meno ripida) poi diventa invece più elevato; questo punto di rottura è il nostro valore cercato.
		È una tecnica che ci da un valore ma porta un po' di soggettivaità.
	- Zhao
		Date le coppie $(\text{modi di guasto,RPN})$ si fa un'interpolazione e si calcolano i livelli di confidenza al 90%. I punti che stanno fuori da queste bande sono quelli di cui dobbiamo ridurre il $RPN$.
		È una tecnica che genera falsi negativi: dovrebbero essere ridotti punti che sono interni alle bande.
	- Pareto
		Afferma che il 20% dei guasti genera l'80% del valore cumulativo dei $RPN$.
		In questo caso abbiamo una soglia troppo bassa e quindi abbiamo troppo falsi positivi.
	- Boxplot
		Si rappresentano i dati in 25-esimo, 50-esimo e 75-esimo percentile. I valori fino alla mediana vanno bene, quelli dalla mediana fino al 75-esimo percentile sono da controllare e quelli sopra il 75-esimo hanno valori critici da ridurre.
# Fuzzy if-then
È la tecnica FMECA alla quale abbiamo aggiunto la [logica fuzzy](logica%20fuzzy.md) dopo l'analisi degli effetti e prima dell'analisi del rischio. In questo caso il RPN fuzzy non è più ottenuto come $RPM=O\times S\times D$, ma si utilizza un sistema di inferenza basato un set specifico di regole.
##### Step
La pipeline per ottenere il RPN fuzzy è la seuquente. Gli input a questo processo sono le tre variabili occurance, severity e detection.
- Si devono determinale le [funzioni di appartenenza](logica%20fuzzy.md#Funzioni%20di%20appartenenza) per i tre input. Queste possono essere diverse per ognuna delle tre variabili linguistiche, dove ogni variabile può avere più valori linguistici e più funzioni di appartenenza.
- Si definsice il motore di inferenza, cioè le regole $if (and/or) then$.
- Faccio il defuzzificamento dell'output ottenedo un valore numerico.
# Fuzzy aritmico
La parte più complessa è quella di definire tutte le regole $if-then$. Per ovviare a questo problema possiamo usare direttamente le operazioni matematiche sul valore delle variabili linguistica ottenuto dopo la fuzzizazione.
##### Pipeline
- Si definiscono dei pesi $(w_O​,w_S​,w_D​)$ per i valori degli input, i.e. per i valori di occurance, severity e detection.
- Ogni valore passato viene fuzzificato e trasformato in una variabile linguistica.
- Per ogni variabile linguistica, e per ogni valore linguistico, si utilizzano i pesi come esponente, e.g. si fa $O^{w_{O}}$.
- Per ottenere il valore RPN finale si fa il prodotto dei tre input dopo questo processo e poi si fa la defuzzificazione.