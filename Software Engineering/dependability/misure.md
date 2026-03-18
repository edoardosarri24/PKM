Le misure nella dependability servono per valutare la [qualità](qualità.md) di un prodotto e di fatto si utilizzano i [parametri RAMS](attributi.md): il controllo [qualità](qualità.md) serve a capire se i valori del prodotto rientrano nei valori accettati dalle specifiche.
In generale possiamo dire che, dato un tasso di guasto costante, per valuta la [reliability](attributi.md#Reliability) si usa $R(t)=e^{-\lambda t}$; il nostro obiettivo è quindi stimare $\lambda$.
##### Nines
Nelle misure si parla di nines, cioè il numero di 9 dopo la virgola in una probabilità. Più il sistema è critico più dovrebbe avere un numero di nines alto (almeno 6 per i critical-systems). Questo valore è all'interno dei requisiti.
# Sperimentale
Dato il sistema, o un suo componente, si eseguono prove in laboratorio o sul campo per ottenere i dati necessari per calcolare parametri statistici come MTTR e MTTF o per ricavare il tasso di guasto sperimentale come $\lambda=\frac{\text{sum of failure}}{\sum \text{quantity}\times \text{time to failure}}$.
##### Conseguenze
- Sono più costose in termini di tempo e soldi, ma sono più precise.
- Il limite principale è che non riusciremo mai a riprodurre tutte le condizioni che possiamo avere nella realtà.
- Ci mostrano non solo dopo quanto avviene il guasto ma anche il suo motivo.
##### Pipeline
La pipeline, guidata dalla norma IEC 60068-1, che può essere eseguita anche in cicli per prendere più misure, è la seguente.
Durante l'esecuzione degli esperimenti possono verificarsi dei guasti e questi devono essere rilevati. Si deve stabilire dei KPI (Key Performance Indicators), cioè dei valori soglia che ci permettono di stabilire se il componenti è fallito. Dobbiamo poi stabilire ogni quanto campionare i dati per vedere se rientrano nei range: troppo raramento non sappiamo quando è avvenuto il guasto; troppo spesso influenziamo il sistema.
- Si mette il componente in una condizione iniziale (non sotto stress) e si raccolgono i dati iniziali per avere una baseline. Si controlla che questi dati rispettino le specifiche.
- Si fanno una o più prove per un tempo che è definito dalle norme del dominio e prendiamo le misure. Una prova è definita da una determinata sollecitazione, più di una contemporaneamente o più di una in successione. Per definire le prove si seguono quattro fasi:
	- Si definisce il ciclo di vita del sistema dal punto di vista del livello di usura. Questo è utile perché in base alla fase di vita in cui il sistema opera ha un grado di usura diverso e questo può essere un fattore che influenza i dati.
	- Si definiscono le sollecitazioni (i.e., le prove) che possono essere sia ambientali che operative.
	- Per ogni esperimenti si deve valutare il livello di severity che si deve applicare e il tempo di durata. La scelta della severity dipende da fattori ambientali e funzionali, è complesso perché dobbiamo scegliere tra tutte le possibili combinazioni della severity dei componenti.
- Si interrompe la sollecitazione e il dispositivo torna alle condizioni iniziali. e si prendono le misere.
- Si confrontano le misure ottenute nelle tre fasi.
##### Classi
Ci sono varie tipi di prove che si possono ottenere in base alla fase dello sviluppo del sistema.
- Conformità
	Sono prove che si fanno in fase di prototipo e voglio esprimere la [[qualità]] del sistema dopo la sua progettazione.
- Qualifica
	Hanno l'obiettivo di verificare il funzionamento e la robustezza del sistema in determinate condizioni queste condizioni non devono eccedere i valori nominali (altrimenti si cambia tipo di esperimento); visto che le condizioni devono essere controllate solitamente sono esperimenti che si fanno in laboratorio.
	Questo ci permette di confrontare dispositivi dello stesso tipo ma di produttori diversi e fare una scelta sulla base dei dati raccolti.
- Affidabilità
	Servono per testare la durata della vita del componente o del sistema sotto esame.
	L'obiettivo è far guastare il sistema: visto che il tasso di guasto nel tempo è sempre diventato migliore (i.e., più basso), la prova può anche concludersi perché è passato troppo tempo: possiamo anche fare delle prove accelerate, cioè prove dove si superano i valori nominali dei componenti per ridurre il tempo necessario a osservare l'effetto della sollecitazione; tali prove accelerate non devono modificare la causa o il meccanismo del guasto. Le prove comunque più dure sono dette HAST (Highly Accelerated Stress Test), dove si alza sia il livello di temperatura sia il livello di umidità; in questo caso è importante che le prove non diventino distruttive e quindi che il dispositivo possa lavorare in quelle condizioni di stress.
	I fattori di stress possono essere applicati in vari modi: uno stress costante è semplice da eseguire, garantisce di avere dei modelli molto testati e conosciuti e permette delle tecniche di analisi più semplici; lo stress a gradino porta a un guasto più veloce, ma ha modelli di analisi più complessi; Lo stress progressivo (i.e., lineare) porta a un guasto veloce, ma ha modelli più complessi ed è difficile controllare lo stress; possiamo avere poi lo stress ciclico o casuale.
	Una volta arrivati al guasto vogliamo analizzare cause, tempi, cronologie dei guasti, pertinenza del guasto e possibili rimedi. Per arrivare a questi risultati si usano dei modelli fisico-matematici e si scelgono in base al fattore di stress usato; spesso i modelli hanno problemi nel caso durante la prova non ci siano guasti; in questi casi si deve utilizzare un modello che ci fornisce questa valutazione.
	Per eseguire la prova si deve definire: il tipo di prova; cosa ci serve per eseguirla; definire i range in cui la prova è superata o non superata; le condizioni in cui eseguire i test; monitorare il sistema durante la prova in real-time per capire esattamente quando si guasta.
- Screening
	Sono quelle prove che si fanno nella prima fase della [bath curve](bath%20curve.png) per evitare di immettere sul mercato quei prodotti che hanno dei problemi di produzione. Non sono prove distruttive, ma si deve scegliere le condizioni di stress appropriate: i sistemi che passano la prova sono immessi sul mercato subito dopo di essa. Il rapporto tra elementi rimossi ed elementi totali è detto SS: se dopo la prova questo è zero allora si deve aumentare il livello di stress e rieseguire la prova.
	Tali test possono essere fatte sia a livello di componente (solitamente fatta dal produttore del componente) che a livello di sistema; le prove di sistema sono più delicate perché solitamente mettendo insieme più componenti il tasso di guasto aumenta.
	Le prove sono difficili da costruire e fino a poco tempo fa non esisteva una norma (si usava solo uno standard militare, i.e. Environment Stress Screening (ESS)) perché è molto dipendente dal dominio e applicazione. Oggi esiste la norma RSS (Reliability Stress Screening).
	Questa norma è un processo in cui vengono applicate delle sollecitazioni ambientali (o elettroniche) a componenti con lo scopo di: portare alla luce i guasti latenti; individuare errori produttivi o di assemblaggio che non è facile trovare; modificare la data della garanzia.
I test sono decisi sulla base dello screening: si evidenziano quali sono i problemi possibili e si identificano le cause; si stressano i componenti che sono responsabili sopra il valore nominale ma sotto il massimo supportato; in questo modo i prodotti difettosi si romperanno e non saranno messi sul mercato. Per quesi difetti che non sono identificabili esiste la garanzia.
# Analitico
Si utilizzano dei modelli per prevedere i parametri statistici. Fare la previsione dell'affidabilità ci permette di capire se il sistema può rispettare i requisiti già nella fase di progettazione.
##### Obiettivi
Questo metodo è usato per:
- In fase progettuale per non avere i tempi e i costi degli esperimenti.
- Riconoscere il componente più critico in modo da poterne aumentare l'affidabilità.
- Capire quali sono le parti più stressate del sistema in modo da analizzare come allungargli la vita.
- Confrontare diverse soluzioni in fase di progettazione.
- Capire quali sono i componenti del sistema che richiedono un ricambio maggiore. In questo modo posso avere le parti di ricambio pronte e ottimizzare il MTTR (to recovery).
- Posso gestire gli aspetti della garanzia.
##### Conseguenze
- Sono meno precise ma sono fondamentali durante la fase di progettazione.
- Hanno un costo minore in termini di tempo e soldi, visto che non dobbiamo fare esperimenti sul campo.
- Non ci danno nessuna informazione sul motivo del guasto, ma solo sui suoi tempi.
- Non possono bastare da sole e devono essere integrate, in qualche momento prima della produzione, con le tecniche [sperimentali](#Sperimentale).
##### Banche dati
Per eseguire la stima hardware (per il [software](attributi.md#Software) la reliability dipende dal testing), ci servono in ogni caso dei dati; questi provengono da esperimenti fatti dal produttore o da altri (i.e., presenti in una [banca dati](previsione%20guasti.md)), e quindi possiamo dire che queste tecniche sono derivate da quelle sperimentali.