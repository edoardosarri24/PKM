Le misure nella dependability servono per valutare la [qualità](qualità.md) di un sistema: il controllo qualità serve a capire se i [parametri RAMS](attributi.md) del prodotto rientrano nei valori accettati dalle specifiche.
Se ci concentriamo sulla [reliability](attributi.md#Reliability), definita come si usa $R(t)=e^{-\int_{0}^\infty\lambda (t)dt}$, l'obiettivo principale delle misure è stimare $\lambda$.
##### Nines
Nelle misure si parla di nines, cioè il numero di 9 dopo la virgola in una probabilità. Più il sistema è critico più dovrebbe avere un numero di nines alto (almeno 6 per i critical-systems). Questo valore è definito ovviamente all'interno dei requisiti.
# Sperimentale
Dato il sistema, o un suo componente, si eseguono prove in laboratorio o sul campo per ottenere i dati necessari per calcolare parametri statistici come MTTR e MTTF o per ricavare il tasso di guasto sperimentale come $\lambda=\frac{\text{sum of failure}}{\sum \text{quantity}\times \text{time to failure}}$.
##### Conseguenze
- Sono più costose in termini di tempo e soldi, ma sono più precise.
- Il limite principale è che non riusciremo mai a riprodurre tutte le condizioni che si possono verificare nella realtà.
- Ci mostrano non solo dopo quanto avviene il guasto ma anche la sua causa.
##### KPI
Se l'obiettivo è trovare i guasti, allora ci servono:
- Dei parametri da valutare.
- Dei valori soglia (Key Performance Indicators) che definiscono quando un guasto occorre sulla base del valore rilevato sui parametri.
- Delle rilevazioni eseguite periodicamente: se queste sono eseguite troppo raramente abbaimo informazioni sommarie su quando il guasto è avvenuto; se sono rilevate troppo spesso influenziamo l'esperimento del sistema.
##### Pipeline
La pipeline, guidata dalla norma IEC 60068, che può essere eseguita anche in modo iterativo per aumentare la precisione, è la seguente:
- Si mette il componente in una condizione iniziale (non sotto stress) e si raccolgono i dati iniziali per avere una baseline. Si controlla che questi dati rispettino le specifiche.
- Si fanno una o più prove per un tempo che è definito dalle norme del dominio in cui operiamo e prendiamo le misure. Una prova è definita da una determinata sollecitazione, più sollecitazioni contemporanee o da più sollecitazioni in sequenza. Per definire le prove si seguono quattro fasi:
	- Si definisce il ciclo di vita del componente dal punto di vista del livello di usura, cioè quanto il componente si usura rapidamente.
	- Si definiscono le sollecitazioni (i.e., le prove). Queste possono influenzare direttamente il sistema, che l'ambiente in cui opera.
	- Si deve valutare il livello di severity e il tempo di durata che dobbiamo applicare a ogni esperimento. È una scelta complessa perché dobbiamo valutare tutte le possibili combinazioni.
- Si interrompe la sollecitazione e il dispositivo torna alle condizioni iniziali e si prendono nuovamente le misere.
- Si confrontano le misure ottenute nelle tre fasi.
##### Classi
In base alla fase di sviluppo del sistema possiamo eseguire prove diverse:
- Conformità
	Le prove di [conformità](conformità.md) prove che si fanno sul prototipo e vogliono esprimere la [[qualità]] del sistema dopo la sua progettazione.
- Qualifica
	Hanno l'obiettivo di verificare il funzionamento e la robustezza del sistema durante il suo utilizza nel range nominale; visto che le condizioni devono essere controllate solitamente sono esperimenti che si fanno in laboratorio e non sul campo.
	Questo ci permette di confrontare dispositivi dello stesso tipo ma di produttori diversi e fare una scelta sulla base dei dati raccolti.
- Affidabilità
	Servono per testare la durata (i.e., il grado di usura) della vita del componente o del sistema sotto esame.
	- Prove accelerate
		L'obiettivo è far guastare il sistema e osservare quando questo guasto occorre. Nel tempo i tassi di guasti sono sempre migliorati (i.e., con l'avanzare della tecnologia le cose funzionano meglio) e per questo la prova può anche concludersi perché è passato troppo tempo e non si è verificato nessun guasto. Possiamo in questo caso fare delle prove accelerate: si utilizza il componente con valori che superano i valori nominali per ridurre il tempo necessario a osservare l'effetto della sollecitazione; tali prove accelerate non devono modificare la causa o il meccanismo del guasto. Le prove più dure sono dette HAST (Highly Accelerated Stress Test), dove si alza il livello di temperatura e di umidità; in questo caso è importante che le prove non diventino distruttive.
	- Modelli di analisi
		Una volta arrivati al guasto vogliamo analizzare cause, tempi, cronologie dei guasti, pertinenza del guasto e possibili rimedi. Per arrivare a questi risultati si usano dei modelli fisico-matematici e si scelgono in base al fattore di stress usato.
	- Fattori di stress
		I fattori di stress possono essere applicati in vari modi: uno stress costante è semplice da eseguire, garantisce di avere dei modelli molto testati e conosciuti e permette delle tecniche di analisi più semplici; lo stress a gradino porta a un guasto più veloce, ma ha modelli di analisi complessi; lo stress lineare porta a un guasto molto veloce, ma ha modelli più complessi ed è difficile controllare lo stress.
- Screening
	Sono prove che si fanno nella prima fase della [bath curve](bath%20curve.png) su tutti i prodotti: l'obiettivo è portare alla luce i guasti latenti e individuare errori produttivi o di assemblaggio di quei prodotti che hanno dei problemi di produzione per evitare di metterli sul mercato. Il rapporto tra elementi rimossi ed elementi totali è detto $SS$: se dopo la prova questo è zero allora si deve aumentare il livello di stress e rieseguire la prova.
	- Sistema
		Tali test possono essere fatte sia a livello di componente (solitamente fatta dal produttore del componente) che a livello di sistema; le prove di sistema sono più delicate perché solitamente mettendo insieme più componenti il tasso di guasto aumenta.
	- Norma
		Le prove sono difficili da costruire e fino a poco tempo fa non esisteva una norma (si usava solo uno standard militare, i.e. Environment Stress Screening (ESS)) perché è molto dipendente dal dominio e applicazione. Oggi esiste la norma RSS (Reliability Stress Screening).
	- Test
		Per individuare quali test eseguire si evidenziano quali sono i problemi possibili e si identificano le cause; si stressano i componenti che sono responsabili di queste cause sopra il valore nominale ma sotto il livello di rottura.
# Analitico
Si utilizzano dei modelli per prevedere i parametri statistici. Fare la previsione dell'affidabilità ci permette di capire se il sistema può rispettare i requisiti già nella fase di progettazione.
##### Obiettivi
Questo metodo è usato per:
- Confrontare diverse soluzioni in fase di progettazione.
- In fase progettuale se non abbiamo tempo e soldi per eseguire esperimenti usando un prototipo.
- Riconoscere il componente più critico in modo da poterne aumentare l'affidabilità.
- Riconoscere il componente più stressato in modo allungare il suo ciclo di vita (e.g., tramite derating).
- Capire quali sono i componenti del sistema che richiedono un ricambio maggiore. In questo modo posso avere le parti di ricambio pronte e ottimizzare il MTTR (to recovery).
- Posso gestire gli aspetti della garanzia già in fase di progettazione.
##### Conseguenze
- Sono meno precise rispetto alle misure sperimentali.
- Hanno un costo minore in termini di tempo e soldi, visto che non dobbiamo fare esperimenti sul campo.
- Non ci danno nessuna informazione sul motivo del guasto, ma solo sui suoi tempi.
- Non possono bastare da sole e devono essere integrate, in qualche momento prima della produzione, con le tecniche [sperimentali](#Sperimentale).
##### Banche dati
Per eseguire la stima hardware (per il [software](attributi.md#Software) la reliability dipende dal testing), ci servono in ogni caso dei dati; questi provengono da esperimenti fatti dal produttore o da altri (i.e., presenti in una [banca dati](military%20handbook.md)), e quindi possiamo dire che queste tecniche sono derivate da quelle sperimentali.