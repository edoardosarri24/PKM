La safety è un'estensione del concetto di [reliability](attributi.md#Reliability): un sistema che rispetta la safety è un sistema che si trova in uno stato di corretto funzionamento oppure in uno stato di comportamento non allineato con le specifiche funzionali ma non dannoso per la salute delle persone. Può essere definita la probabilità del verificarsi di un rischio inaccettabile che porta a danni fisici per l'uomo, per beni o per l'ambiente.
##### Confronti
È diverso dalla security: questa protegge il sistema da attacchi volontari esterni; la safety lo protegge da quello che può succedere all'interno del sistema. Non si parla neanche di affidabilità: un sistema può essere inaffidabile ma essere perfetto dal punto di vista della safety.
##### Cause
La safety si considera nei sistemi safety-critical (e.g., trasporti, sanità ed energia). In sistemi di questo tipo gli incidenti che possono accadere sono molti e sono suddivisi in tre categorie con stessa cardinalità: errore umano (che spesso non può essere eliminato); design del sistema errato in qualche suo componente; guasto dei componenti del sistema.
# Safety Instrumental System
La branca della functional safety si occupa di riportare il sistema in uno stato corretto (dal punto di vista della safety) quando succede qualcosa di non desiderato.
Le funzioni che implementano questo comportamento sono dette Safety Related System (SRS) o Safety Instrumental System (SIS). Sono quindi dei componenti a parte, staccati dal sistema principale che devono proteggere, in modo da portare il [rischio](#Rischio) a un livello accettabile. Non dobbiamo confondere queste funzioni con quelle caratteristiche funzionali del sistema principale che tentano di riportare dei valori fuori range all'interno di esso; le SIS portano il sistema in uno stato corretto solo se le funzioni del sistema non risolvono un potenziale problema.
##### Tipi
Ci sono due tipi di SIS:
- Attivo
	È un ciclo di controllo che risponde a input non corretti. Un esempio è un sistema che spegne un motore se la sua temperatura diventa troppo calda.
- Passivo
	È una contromisura passiva, che non richiede attivazione. Un esempio è isolare un tubo caldo con la lana di roccia.
##### Comportamenti
Ci sono due comportamenti che possiamo implementare con le SIS:
- Fail safe
	Il sistema che stiamo monitorando viene spento. Questo comporta di portare in uno stato safety correct ma comporta anche un'indisponibilità del sistema, motivo per cui non si può fare sempre (e.g., aerei).
- Fault tollerant
	Con la [fault tollerance](fault%20tollerance.md) si costruisce un sistema tollerante al guasto, implementato solitamente con la [ridondanza](ridondanza.md). In questo modo non si perde l'availability del sistema, ma solitamente dobbiamo procedere con una [menutenzione](manutenibilità.md) del sistema.
##### Composizione
Le SIS attive sono composte da tre blocchi in serie:
- Sensore
	Controlla lo stato del sistema principale monitorando i dati che si è deciso, durante l'[analisi del rischio](analisi%20del%20rischio.md), essere rilevanti.
- Logica
	Elabora i dati provenienti dai sensori e che decide se eseguire effettivamente la funzione.
- Attuatore
	Esegue fisicamente la funzione di sicurezza riportando il sistema in uno stato sicuro. Possiamo avere più sensori e attuatori che condividono una stessa logica.
##### Progettazione
La progettazione si un SIS non è banale e segue delle fasi:
- Si deve scegliere la tecnologia, cioè quali sensori, attuatori e logiche saranno utilizzati.
	Il problema dei componenti è che anche essi sono soggetti a guasti; in questo contesto solitamente si utilizzano componenti ad alta affidabilità e ridondati.
	I guasti in questo scenario sono considerati essere guasti casuali e sono divisi in due categorie:
	- Pericoloso
		Un guasto è pericoloso quando non permette l'esecuzione della SIS.
		Per trovare i guasti pericolosi si utilizza la FMEDA (Failure Modes, Effects and Diagnostic Analysis), una variante della [FMECA](FMECA.md): mentre questa si ferma alla criticità del guasto, la FMEDA aggiunge la Diagnostic Coverage (DC); è definita come il rapporto tra i guasti pericolosi detected e i guasti pericolosi totali e idealmente deve valere 1.
	- Sicuro
		Un guasto è sicuro quando porta a un'esecuzione della SIS quando non necessario, detta intervento spurio.
- Si deve progettare l'architettura.
	I componenti sono divisi in tipo A e tipo B: i primi sono quelli molto testati, di cui si conosce tutti i modi di guasto, il comportamento in caso di guasto e possiamo calcolare il [tasso di guasto](tasso%20di%20guasto.md) (solitamente sensori e attuatore); i secondi sono tutti gli altri (solitamente le logiche).
	Relativamente ai componenti testati si parla di Safe Failure Fraction (SFF), che rappresenta la percentuale di guasti pericolosi undetected (sono quelli che mi fanno funzionare male il SIS), e che idealmente voglio sia 0.
	La norma definisce gli Architectural Constraints, cioè dei vincoli architetturali per le SIS. Per scegliere questa architettura vincolata ci sono due approcci:
	- Route1
		L'idea è di incrociare i dati del tipo di componente (i.e., A o B), il SFF e il SIL target. Il risultato è il HFT minimo, definito come il numero di guasti che il SIS può tollerare prima che esso perda le sue funzioni di protezione verso il sistema principale. L'architettura più usata la $koon$: si possono guastare $k$ componenti su $n$.
		In questo contesto considero anche il Common Cause Failures (CCF), cioè le cause di guasto comuni a due componenti ridondanti (e.g., incendio), si risolve con la [design diversity](ridondanza.md#Design%20diversity).
	- Route2
		È utilizzata quando i componenti sono ampiamente testati: invece di utilizzare calcoli teorici complessi come l'SFF, si utilizza la storia statistica del componente.
		Questo permette di raggiungere un SIL elevato anche in assenza di ridondanza (i.e., con $HFT=0$) se i componenti si sono dimostrati sul campo molto affidabili.
- Decidere la filosofia di testing.
	Per capire se un SIS è guasto ci sono due approcci: con la diagnostica interna si vuole limitare i guasti pericolosi e undetected ed è il SIS stesso in maniera autonoma che si analizza per capire se è correttamente funzionante; con i Proof Test si vanno a eseguire direttamente le SIS trovando i dangerous undetected fault (che sono il nostro obiettivo e sono più complessi da trovare).
	La PFD (Probability of Failure on Demand, definisce la probabilità che le SIS non si attivino quando necessario) è una curva che [sale nel tempo](safety%20Proof%20Test.png), cioè più tempo passa dall'ultimo controllo più è probabile che avvenga un guasto pericoloso. Visto che la PFD determina l'approvazione del valore SIL richiesto, dovremmo tenerla bassa: la diagnosi ha il compito di abbassare il valore medio durante la salita della curva; il Proof Test riporta il componente in uno stato di as good as new. In generale però questi test sono da eseguire molto raramente (e.g., intervalli di almeno 6 mesi) perché costosi e perché richiedono di interrompere la disponibilità del sistema durante il test stesso.
- Valutare la Safety
	La SIS deve essere messa in parallelo (e.g., 1/2) o in serie (e.g., 2/2) a seconda dello scopo del componente: il parallelo è utilizzato nelle situazioni in cui la SIS ci fornisce un'apertura sicura, cioè permette il passaggio quanto il canale principale è bloccato; il sequenziale permette alla SIS di eseguire una chiusura sicura.
	Vediamo adesso come possiamo comporre i tre blocchi (i.e., sensori, logica e attuatori) insieme: in generale la PFD è data dalla somma delle tre PFD visto che i tre blocchi sono in serie e solitamente il peso maggiore in questa somma è dato dagli attuatori (perché sono parti meccaniche) e poi dai sensori; usare una di queste architetture solitamente aumenta la sicurezza ma diminuisce la disponibilità.
	Vediamo diversi tipi di ridondanza per ognuno dei tre blocchi
	- 1oo1
		Non abbiamo nulla di ridondato, cioè abbiamo un singolo canale (i.e., gruppo sensore, logica e attuatore): non abbiamo tolleranza ai guasti, ma eventualmente solo una diagnostica per migliorare la DC.
		La formula è $PFD=(\lambda_{DU}+\lambda_{DD})t_{CE}$, dove: $\lambda_{DU}$ è il tasso dei guasti pericolosi non rilevati; $\lambda_{DD}$ è il tasso dei guasti pericolosi rilevati dalla diagnostica; non abbiamo i guasti sicuri perché ci interessano solo quelli pericolosi; $t_{CE}$​ è il tempo medio per cui il canale rimane scoperto (i.e., c'è un guasto latente) prima che un guasto venga scoperto e riparato.
		La formula non è complessa, ma i dati sono difficili da reperire. Per migliorare la PFD possiamo migliorare il tempo tra due Proff test (da cui $t_{CE}$ dipende) oppure utilizzare componenti più affidabili.
	- 1oo2
		Abbiamo due canali che lavorano in parallelo e la SIS interviene se almeno uno dei due è in guasto.
		È molto utilizzata perché la probabilità di guasti periocolosi è molto bassa. Il problema è che se la sicurezza raddoppia, la disponibilità dell'impianto dimezza: questo vuol dire che nel caso di guasti sicuri, che saranno il doppio vista la ridondanza, il sistema si blocca. I guasti sicuri sono accettabili dal punto di vista della sicurezza ma non da quello della disponibilità.
	- 2oo2
		Abbiamo due canali ma il sistema si blocca se entrambi hanno un guasto.
		Il vantaggio non è tanto la sicurezza ma la disponibilità: la probabilità di guasto pericoloso (i.e., PFD) è circa il doppio rispetto all'architettura 1oo1. Possiamo dire che questa raddoppia la disponibilità ma dimezza la sicurezza.
	- 2oo3
		Tollera un guasto; si ha un'alta disponibilità.
		È costosa, ma aumenta sia la disponibilità che sicurezza.
# Rischio
In un sistema sappiamo che il [rischio](rischio.md) zero non può esistere. Quello che dobbiamo fare in questo tipo di sistemi è portare il rischio residuo sotto il [rischio tollerabile](rischio.md#Rischio%20accettabile) (i.e., quello che le specifiche ci dicono essere consentito); questo permette di avere un margine di sicurezza che permette di gestire gli imprevisti che non abbiamo considerato.
Il rischio residuo è definito come $residualRisk=O \times S \times PFD$, dove: la $O$ è la occurence, la $S$ è la severity e la $PFD$ è la Probability of Failure on Demand.
##### Valutazione
Il rischio deve essere valutato in modo da capire quali situazioni debbano coprire le safety function.
Questa valutazione in alcuni casi può essere fatta con un risk graph, grazie al quale si identificano i rischi e se ne fornisce una rappresentazione grafica. A partire da un rischio si crea un albero: i primi figli sono le conseguenze, secondi sono i tempi di esposizione, la riduzione del pericolo e poi l'occorrenza; l'idea è di assegnare i possibili SIL.
##### Risk Reduction Factor
Per capire quando le safety function devono essere potenti si utilizza il Risk Reduction Factor (RRF), che indica quante volte le safety function devono ridurre il rischio esistente ed è calcolato come $RRF=\frac{1}{PFD}$, dove la $PFD$ è la Probability of Failure on Demand.
# Norma
Ci sono due norme principali. In generale le norme che riguardano di safety parlano di SIL, tranne nell'avionico dove si parla di Design Assurance Level (SAL).
- IEC61508
	È utilizzata da chi costruisce sistemi per un uso generico; è più stringente perché si devono prevedere più utilizzi del prodotto.
	È composto da più parti: nella 1 abbiamo i requisiti generali; la 2 e la 3 trattano hardware e software; la 4 e la 5 sono esempi di sistemi; la 6 definisce le linee guida; la 7 da un'idea delle tecniche da utilizzare, in particolare si focalizza su come rimuovere i guastici sistematici (i.e., quelli causati dall'uomo).
- IEC 61511
	È utilizzata da chi utilizza componenti già normati e li mette insieme per costruire un sistema.
##### Safety lifecicle
Le norme gestiscono il ciclo di vita attraverso 12 fasi: dopo la costruzione del sistema questo deve essere manutenuto per correggere gli errori di progetto fino alla sua dismissione.
Ci sono 3 macro fasi:
- Analisi
	Si devono analizzare i rischi, identificare il livello di SIL da ottenere e decidere quali safety function implementare.
- Design
	Si definisce l'architettura del sistema e le tecnologie utilizzate. Poi si verifica che il SIL sia stato raggiunto.
- Costruzione e maintenance
	Comprende la costruzione del manuale di installazione e il piano di manutenzione.
##### Software
Si utilizza il modello a V.
# SIL
I Safety Integrity Level sono la misura della bontà delle [safety function](#Funzioni) e sono definiti dalla norma.
L'assegnazione del livello SIL avviene tramite due tecniche: la Probability of Failure on Demand (PFD) si utilizza per quei componenti a bassa richiesta (i.e., utilizzo meno di una volta l'anno), e definisce la probabilità che le SIS non si attivino quando necessario; il Probability of Failure per Hour (PFH) si usa per sistemi che elaborano in modo continuo o con un alto rate di richieste (i.e., più di una volta l'anno), dove il rischio è che si rompano mentre lavorano.
##### Livelli
I livelli di SIL sono da 1 a 4. Chi valuta e assegna il SIL a una SIS deve essere tanto più distante dal progetto quanto il livello di SIS deve essere elevato.
- 1
	Per ottenere il primo livello la PDF deve stare in $[10^{-2},10^{-1}]$ e la PFH in $[10^{-6},10^{-5}]$.
	La persona che assegna il SIS deve essere indipendente, cioè esterna al progetto.
- 2
	È molto simile al SIS 1.
- 3
	La persona che assegna il SIS deve essere almeno di un dipartimento diverso; non basta che sia una persona indipendente.
- 4
	Per l'ultimo livello per la PFD e PFH si hanno rispettivamente $[10^{-5},10^{-4}]$ e $[10^{-9},10^{-8}]$.
	La persona che assegna il SIS deve essere necessariamente di un'altra azienda.
##### Compromesso
Si deve ottenere un livello di SIL più alto solo se serve. Passare da un livello all'altro è costoso in termini di componenti [ridondati](ridondanza.md), di costi per hardware migliore e di duplicazione di software.