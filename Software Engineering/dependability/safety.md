La safety è un'estensione del concetto di [reliability](attributi.md#Reliability): un sistema che rispetta la safety è un sistema che si trova in uno stato di corretto funzionamento oppure in uno stato di comportamento non allineato con le specifiche funzionali ma non dannoso per la salute delle persone. Può essere definita come il verificarsi di un rischio inaccettabile che porta a danni fisici per l'uomo, per beni o per l'ambiente.
##### Confronti
È diverso dalla security: questa protegge il sistema da attacchi volontari esterni; la safety lo protegge da quello che può succedere all'interno del sistema. Non si parla neanche di affidabilità: un sistema può essere inaffidabile ma essere perfetto dal punto di vista della safety.
##### Cause
La safety si considera nei sistemi safety-critical, come trasporti, sanità ed energia. In sistemi di questo tipo gli incidente che possono accadere sono molti e sono suddivisi in tre categorie con stessa cardinalità: errore umano (che spesso non può essere eliminato); design del sistema errato in qualche suo componente; guasto dei componenti del sistema.
# Funzionale
la functional safety si occupa di far rispondere nel modo corretto il sistema quando qualcosa non va come previsto.
Ci sono due modi per implementarla:
- Attivo
	È un ciclo di controllo che risponde a input non corretti. Un esempio è un sistema che spenge un motore se la sua temperatura diventa troppo calda.
- Passivo
	È una contromisura passiva, che non richiede attivazione. Un esempio è isolare un tubo caldo con la lana di roccia.
##### Funzioni
Le funzioni che si occupano di riportare il sistema in uno stato corretto dal punto di vista della safery quando succede qualcosa di non desiderato sono detti Safety Related System (SRS) o Safety Instrumental System (SIS). Sono quindi i modi per portare il [rischio](#Rischio) a un livello accettabile in modo da ottenere una cerca certificazioned i SIL.
Ognuna di queste funzioni è caratterizzata da un Safety Integrity Level ([SIL](#SIL)) ovvero una misura del corretto funzionamento della safety function.
# Rischio
In un sistema sappiamo che il [rischio](rischio.md) 0 non può esistere. Quello che dobbiamo fare in questo tipo di sistemi è portare il rischio residuo (i.e., quello che affligge il sistema) sotto il rischio tollerabile (i.e., quello che le specifiche ci dicono essere consentito); questo permette di avere un margine di sicurezza che permette di gestire gli imprevisti che non abbiamo considerato.
##### Valutazione
Il rischio deve essere valutato in modo da capire quali situazioni debbano coprire le safety function.
Questa valutazione in alcuni casi può essere fatta con un risk graph, grazie al quale si identificano i rischi e se ne fornisce una rappresentazioen grafica. A partire da un rischio si crea un albero: i primi figli sono le conseguenze, secondi sono i tempi di esposizione, la riduzione del pericolo e poi l'occorrenza; l'idea è di assegnare i possibili SIL.
##### Risk Reduction Factor
Per capire quando le safety function devono essere potenti si utilizza il livello di gravità si utilizza il Risk Reduction Factor (RRF), che indica quante volte le safety function devono ridurre il rischio esistente ed è calcolato come $RRF=\frac{1}{PFD}$, dove la $PFD$ è la Probability of Failure on Demand.
# Norma
Ci sono due norme principali:
- IEC61508
	È utilizzata da chi costruisce sistemi per un uso generico; è più stringente perché si devono prevedere più utilizzi del prodotto.
	È composto da più parti: nella 1 abbiamo i requisiti generali; la 2 e la 3 trattano hardware e software; la 4 e la 5 sono esempi di sistemi; la 6 definisce le linee guida; la 7 da un'idea delle tecniche da utilizzare.
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
L'assegnazione del livello SIL avviene tramite due tecniche: la Probability of Failure on Demand (PFD) si utilizza per quei componenti a bassa richiesta, dove il rischo è che non si attivino quando servono; il Probability of Failure per Hour (PFH) si usa per sistemi che elaborano in modo continuo o con un alto rate di richieste, dove il rischio è che si rompano mentre lavorano.
##### Livelli
I livelli di SIL sono da 1 a 4.
- 1
	Per ottenere il primo livello la PDF deve stare in $[10^{-2},10^{-1}]$ e la PFH in $[10^{-6},10^{-5}]$.
- 4
	Per l'ultimo livello si hanno rispettivamente $[10^{-5},10^{-4}]$ e $[10^{-9},10^{-8}]$.
##### Compromesso
Si deve ottenere un livello di SIL più alto solo se serve. Passare da un livello all'altro è costoso in termini di componenti [ridondati](ridondanza.md), di costi per hardware migliore e di duplicazione di software.