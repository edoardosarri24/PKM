La stima dei costi (time & money) è sempre più precisa più ci si avvicina alla fine del progetto; le aziende hanno però bisogno di stimare i costi il prima possibile.
Ci si può basare sull'esperienza, ma in alcuni casi diventa difficile fare delle stime: per progetti grandi si deve seguire un metodo; se si svolge un qualcosa di nuovo e diverso rispetto a quanto siamo abituati; le tecnologie cambiano di frequente ed è possibile che si debba studiare qualcosa di nuovo durante il progetto.
Un altra metodologia è una stima algoritmica, usando una funzione matematica.
COCOMO (**COnstructive COst estimation MOdel**) predice lo sforzo e il programma per progetto SW.
## PROGETTI
I progetti possono avere diversi livelli di complessità di sviluppo. Ci sono tre modelli (distinti in base al progetto obiettivo):
- Organic mode
	- Il progetto è relativamente piccolo, sviluppato in un ambiente familiare stabile.
	- Il team di sviluppo è abbastanza piccolo e il progetto è simile ai precedenti.
	- Non sono richieste innovazioni o nuove conoscenze.
	- Gli sviluppatori si conoscono e sono tutti dello stesso livello.
- Semi-detached mode
	- Lo sviluppo si basa su qualcosa che non si conosce bene.
	- Nel team di sviluppo ci sono anche persone con poca esperienza.
- Embedded mode
	- Il progetto ha vincoli HW precisi che devono essere rispettati.
	- Si richiede grande quantità di innovazione e conoscenze nuove.
## COCOMO 1
È un modello nato nel 1981 usato per stimare i costi e la durata di un progetto. Si basa sulla formula $E=A\times Size^B \times M$, dove: $A$ rappresenta le caratteristiche dl team di sviluppo o dell'azienda; $B$ rappresenta la grandezza del progetto; M rappresenta le caratteristiche del progetto.
### Unità di misura
Solitamente l'unità di misura è il $uomo/mesi$ (PMs). Ci indica sia il totale del lavoro sia come questo è distribuito: nel primo mese ci possono volere 2 persone, nel secondo 5 e nel terzo 3.
Il PM è l'integrale del valore $uomo/mese$. Questa curva ha un valore maggiore nel periodo centrale del progetto e un valore inferiore all'inizio e alla fine. Se ha valore ad esempio 10, significa uno sforzo di 10 persone per 10 mesi.

Supposto che il management sia buono, sia dal punto di vista del cliente che degli sviluppatori, e che i requisiti non cambino, allora i costi del progetto dipendono dalla sua grandezza, cioè dal Line Of Code (LOC).

Se il Tdev si riduce allora il PM deve aumentare perché più persone implicano minor produttività.
### Precisione
Ci sono tre livelli di precisione:
- **Basic**
	Serve per avere un ordine di grandezza più che una vera e propria stima. Il problema è che considera solo la dimensione del progetto (il costo della forza uomo richiesta) e non gli altri costi, come servizi esterni, vincoli sull'HW o tool utilizzati.
	- I costi sono calcolati come $E=a_1\times(KLOC)^{a_2}$ con unità di misura il PM.
	- Il tempo è calcolato come $Tdev=b_1\times E^{b_2}$ con unità di misura i mesi.
	- $a_1$, $a_2$, $b_1$ e $b_2$ sono costanti per ogni categoria di progetto.
- **Intermediate**
	Non valuta i costi solo rispetto alla grandezza del progetto, ma anche rispetto a 15 fattori. I costi sono calcolati come $E=a_1\times(KLOC)^{a_2}\times EAF$ dove è tutto come prima tranne l'effort adjustment factor (EAF) che è la media dei 15 fattori.
	I fattori si divino in si dividono nei seguenti gruppi:
	- Attributi del prodotto software
		- Complessità
		- Grandezza del database
		- Affidabilità richiesta
	- Attributi dell'HW
		- Vincoli sulle performance run-time
		- Vincoli di memoria
		- OS
	- Capacità del team
		- Capacità di analisi
		- Capacità di progammazione
		- esperienza con applicazioni, VM, linguaggi di programmazione.
	- Attributi del progetto
		- Uso di tool
		- Metodi di SW engineering
- **Complete**
	Il progetto viene diviso in più parti e ogni parte viene valutata in modo indipendente secondo il livello di dettaglio intermediate.

## COCOMO 2
È un modello che stima i costi e i tempi del progetto sulla base di sotto modelli.
I sottomodelli in COCOMO2 sono:
### Composizione dell'applicazione (application composition model)
Usato quando il SW è composto da più parti. Supporta progetti con prototipi e progetti con ampio riuso di codice. I costi dipendono dal numero di funzionalità e dal numero di oggetti creati. Si calcola come $PMs=(NAP\times(1-\%reuse/100))/PROD$, dove: $NAP$ (Number of Application Points) è la stima del business process (Cioè della parte che distingue l'applicazione, dove gli application points sono una tecnica di stima che valuta il lavoro necessario per realizzare le varie componenti dell'applicazione); PROD è la capacità del team e dei tool utilizzati (misurata in $NAP/month$).
### Modello di progettazione iniziale (early design model)
Usato quando si sono definiti i requisiti ma non si ancora fatto il design. La stima può essere fatta dopo che i requisiti sono stati concordati. I costi dipendono dal numero di funzioni. Si calcola come $PMs=A\times size^B\times M$, dove $A$ è una costante, B dipende dalle nuove conoscenze che servono e $M$ è il prodotto di determinati fattori, come l'abilità del personale, la complessità del prodotto e la flessibilità della consegna. $Size$ è in KLOC (Kilo Line Of Code).
### Post architettura (post architecture model)
Usato quando si è già fatto il design e si deve passare all'implementazione. I costi dipendono dal numero di LOC. Si calcola come $PMs=A\times size^B \times M$, dove $A$ è una costante, $B$ dipende da 5 fattori (risk analysis, flessibilità dello sviluppo, esperienza utile derivante da vecchi progetti, maturità del processo, coesione del team) e $M$ è il prodotto di determinati fattori, come l'abilità del personale, la complessità del prodotto e la flessibilità della consegna.
Il tempo di sviluppo di misura con la formula $TDEV = 3 * (PM)^{(0.33+0.2*(B-1.01))}$ dove B è l'esponente calcolato come detto prima. 
### Riuso (reuse model)
Usato quando si devono integrare più parti esistenti (riuso). I costi dipendono dal numero di LOC riutilizzate. Ci sono due versioni:
- Black-box
	Si intende il riuso completo, senza dover mettere mano al codice. Si calcola come $PMs=(ASLOC\times AT/100)/ATPROD$, dove ASLOC è il numero di LOC integrate, AT il numero di LOC generate automaticamente e ATPROD la produttività del team nell'integrare il codice.
- White-box
	Si intende la modifica di qualcosa di già esistente e la sua integrazione. Si calcola come $PMs=ASLOC\times (1-AT/100)\times AAM$, dove è tutto come prima e AAM è un fattore di aggiustamento determinato dal costo di cambiare il codice riusato e della sua integrazione e il costo delle decisioni prese per il riuso.