La safety è un'estensione del concetto di [reliability](attributi.md#Reliability): un sistema che rispetta la safety è un sistema che si trova in uno stato di corretto funzionamento oppure in uno stato di comportamento non allineato con le specifiche funzionali ma non dannoso per la salute delle persone. Può essere definita come il verificarsi di un rischio inaccettabile che porta a danni fisici per l'uomo, per beni o per l'ambiente.
##### Confronti
È diverso dalla security: questa protegge il sistema da attacchi volontari esterni; la safety lo protegge da quello che può succedere all'interno del sistema. Non si parla neanche di affidabilità: un sistema può essere inaffidabile ma essere perfetto dal punto di vista della safety.
##### Cause
La safety si considera nei sistemi safety-critical, come trasporti, sanità ed energia. In sistemi di questo tipo gli incidente che possono accadere sono molti e sono suddivisi in tre categorie con stessa cardinalità: errore umano (che spesso non può essere eliminato); design del sistema errato in qualche suo componente; guasto dei componenti del sistema.
# Safety Instrumental System
La functional safety si occupa di riportare il sistema in uno stato corretto (dal punto di vista della safety) quando succede qualcosa di non desiderato. Le funzioni che implementano questo comportamento sono dette Safety Related System (SRS) o Safety Instrumental System (SIS).
Sono quindi dei componenti a parte staccati dal sistema principale che lo devono proteggere, in modo da portare il [rischio](#Rischio) a un livello accettabile. Non dobbiamo confondere queste funzioni con quelle caratteristiche funzionali del sistema principale che tentano di riportare dei valori fuori range all'interno di esso; le safety function portano il sistema in uno stato corretto solo se le funzioni del sistema non risolvono un potenziale problema.
##### Tipi
Ci sono due tipi di safety function:
- Attivo
	È un ciclo di controllo che risponde a input non corretti. Un esempio è un sistema che spegne un motore se la sua temperatura diventa troppo calda.
- Passivo
	È una contromisura passiva, che non richiede attivazione. Un esempio è isolare un tubo caldo con la lana di roccia.
##### Comportamenti
Ci sono due comportamenti che possiamo implementare con le SIS:
- Fail safe
	Il sistema che stiamo monitorando viene spento. Questo comporta di portare in uno stato safety correct ma comporta anche un'indisponibilità del sistema, motivo per cui non si può fare sempre (e.g., aerei).
- Fault tollerant
	Con la [fault tollerance](fault%20tollerance.md) si costruisce un sistema tollerante al guasto, implementato solitamente con la [ridondanza](ridondanza.md). In questo modo non si perde l'avaibility del sistema, ma solitamente dobbiamo procedere con una manutenzione del sistema.
##### Composizione
Sono composti da tre blocchi in serie:
- Sensore
	Controlla lo stato del sistema principale monitorando i dati che si è deciso essere rilevanti durante l'[analisi del rischio](analisi%20del%20rischio.md).
- Logica
	Elabora i dati provenienti dai sensori e che decide se eseguire effettivamente la funzione.
- Attuatore
	Esegue fisicamente la funzione di sicurezza riportando il sistema in uno stato sicuro. Possiamo avere più sensori e attuatori che condividono una stessa logica.
##### Progettazione
La progettazione non è banale:
- Si deve scegliere la tecnologia, cioè quali sensori, attuatori e logiche saranno utilizzati.
	Il problema dei componenti è che anche essi sono soggetti a guasti; questo solitamente viene mitigato utilizzando componenti ad alta affidabilità e con la ridondanza.
	I guasti in questo scenario sono considerati essere guasti casuali e sono divisi in due categorie:
	- Pericoloso
		Un guasto è pericoloso quando non permette l'esecuzione della SIS.
		Per trovare i guasti pericolosi si utilizza la FMEDA (Failure Modes, Effects and Diagnostic Analysis), una variante della [FMECA](FMECA.md): mentre questa si ferma all'effetto del guasto, la FMEDA aggiunge la Diagnostic Coverage (DC); è definita come il rapporto tra i guasti pericolosi detected e i guasti pericolosi totali e idealmente deve valere 1; da un'idea della diagnostica a bordo del sistema che identifica i guasti pericolosi.
	- Sicuro
		Un guasto è sicuro quando porta a un'esecuzione della SIS quando non necessario, detta intervento spurio.
- Si deve progettare l'architettura, cioè quali componenti saranno ridondati in modo ricorsivo (i.e., scomponendo i vari componenti).
# Rischio
In un sistema sappiamo che il [rischio](rischio.md) zero non può esistere. Quello che dobbiamo fare in questo tipo di sistemi è portare il rischio residuo (i.e., quello che affligge il sistema) sotto il rischio tollerabile (i.e., quello che le specifiche ci dicono essere consentito); questo permette di avere un margine di sicurezza che permette di gestire gli imprevisti che non abbiamo considerato.
Il rischio residuo è definito come $residualRisk=O \times S \times PFD$, dove: la $O$ è la occurence, la $S$ è la severity e la $PFD$ è la Probability of Failure on Demand.
##### Valutazione
Il rischio deve essere valutato in modo da capire quali situazioni debbano coprire le safety function.
Questa valutazione in alcuni casi può essere fatta con un risk graph, grazie al quale si identificano i rischi e se ne fornisce una rappresentazioen grafica. A partire da un rischio si crea un albero: i primi figli sono le conseguenze, secondi sono i tempi di esposizione, la riduzione del pericolo e poi l'occorrenza; l'idea è di assegnare i possibili SIL.
##### Risk Reduction Factor
Per capire quando le safety function devono essere potenti si utilizza il livello di gravità si utilizza il Risk Reduction Factor (RRF), che indica quante volte le safety function devono ridurre il rischio esistente ed è calcolato come $RRF=\frac{1}{PFD}$, dove la $PFD$ è la Probability of Failure on Demand.
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
L'assegnazione del livello SIL avviene tramite due tecniche: la Probability of Failure on Demand (PFD) si utilizza per quei componenti a bassa richiesta (i.e., utilizzo meno di una volta l'anno), dove il rischo è che non si attivino quando servono; il Probability of Failure per Hour (PFH) si usa per sistemi che elaborano in modo continuo o con un alto rate di richieste (i.e., più di una volta l'anno), dove il rischio è che si rompano mentre lavorano.
##### Livelli
I livelli di SIL sono da 1 a 4. Chi valuta e assegna il SIL a una SIS deve essere tanto più distante dal progetto quanto il livello di SIS deve essere elevato.
- 1
	Per ottenere il primo livello la PDF deve stare in $[10^{-2},10^{-1}]$ e la PFH in $[10^{-6},10^{-5}]$.
	La persona che assegna il SIS deve essere indipendente, cioè esterna al progetto.
- 2
	È molto simile al SIS 1.
- 3
	La persona che assegna il SIS deve essere almeno di un dipartimento diverso; non basta che sia una persona iindipendente.
- 4
	Per l'ultimo livello si hanno rispettivamente $[10^{-5},10^{-4}]$ e $[10^{-9},10^{-8}]$.
	La persona che assegna il SIS deve essere necessariamente di un'altra azienda.
##### Compromesso
Si deve ottenere un livello di SIL più alto solo se serve. Passare da un livello all'altro è costoso in termini di componenti [ridondati](ridondanza.md), di costi per hardware migliore e di duplicazione di software.