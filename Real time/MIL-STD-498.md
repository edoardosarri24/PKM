È un modello per lo sviluppo di sw in ambito militare pensato per essere uno standard. A ogni [fase](MIL-STD-498%20and%20V-model.png) di questo modello corrisponde una fase nel [V-Model](Plan-Driven%20process.md#V-Model); il V-model è rivisto in modo specifico per questo standard in modo da creare varie fasi specifiche.
### Model Driven Engineering
Le [PTPN](Preemptive%20Time%20Petri%20Nets%20(PTPN).md) sono fondamentali nella MDE.
- Model Driver Engineering (MDE)
	È il processo che porta alla realizzazione di modelli a partire da altri modelli; si usano modelli diversi per livelli di astrazione diversi dello stesso concetto. Vuole garantire l'analizzabilità di un modello: realizzare un modello a mano può essere molto faticoso e molto error prone; farlo in modo incrementale e con dei tool invece porta a creare modelli che seguono regole precise e che sono facilmente analizzabili.
- Model Driver Develpment (MDD)
	È il processo che porta alla creazione di codice a partire da modelli.
![fase](MIL-STD-498%20and%20V-model.png)
# PLANNING AND BUDGET
È la fase in cui si fa il piano di quello che sarà il processo e si stabilisce il budget. È una fase preliminare che non ha una corrispondente nel V-model.
# SYSTEM/SUBSYSTEM ANALYSIS AND DESIGN
Corrisponde alle fasi SD1, SD2 e SD3.1 del V-model.
- SD1
	Si definiscono i requisiti di sistema, cioè quelli richiesti dal committente. Ha uno scope di sistema.
- SD2
	Si definiscono le unità che comporranno il sistema e si associano con i requisiti definiti dall'utente. Ha uno scope di sistema; non si entra infatti nel dettaglio delle varie unità.
- SD3
	Si decompone ogni unità, descrivendo per ognuna il suo hw (HW Configuration Items - HCIs), sw (Computer SW Configuration Items - CSCIs) e firmware (Firmware Configuration Items - FCIs). Si passa da uno scope di sistema a uno di unità.
### Documenti
Produce il documento System/Subsystem Design Description (SSDD). In pratica quindi dice come il sistema è suddiviso in unità; non vengono anche descritte le unità nei loro dettagli, perchè siamo ancora a un livello di sistema.
Per realizzare questo documento si usa il component-diagram di [UML-MARTE](MARTE.md).
# SW REQUIREMENT ANAYSIS
Corrisponde alla fase SD3.2; descrive solamente la parte sw, cioè i vari CSCIs. Lo scope è quello dell'unità.
### Documenti
Produce:
- Interface Requirements Specification (IRS) per ogni unità.
- Software Requirements Specification (SRS) per ogni CSCI dell'unità. Descrive le funzioni di ogni componente sw dell'unità e le relazioni tra di essi.
# SW DESIGN
Corrisponde alle fasi di SD4-SW e SD5-SW del V-Model.
- SD4-SW
	Descrive l'architettura sw di ogni unità (CSCI) come un insieme di task, definendo i loro aspetti funzionali (cosa effettivamente fanno, cioè i compiti che svolgono), i periodi e le deadline. Sono decisioni alto livello, cioè non ci interessa le decisioni prese dallo sviluppatore per implementarle.
	Lo scope è quello di componente.
- SD5.1-SW
	Descrive il design sw di ogni unità (CSCI) e dei task che la compongono in base alle scelte prese dal programmatore: ad esempio tramite il numero di computazioni per ogni task, il loro tempo di esecuzione, l'utilizzo di risorse in mutua esclusione o le risorse usate su cui si può fare preemption.
	Lo scope è quello di modulo.
- SD5.2-SW
	Comporta due fasi:
	- Simulazione del modello PTPN ottenuto.
		Fare la simulazione prima dell'analisi è una buona pratica perché ci permette di avere una confidenza (più o meno forte) sulla correttezza del modello.
		Non è una verifica esaustiva, ma spesso con l'enumerazione dello spazio degli stati vegnono generate molte classi e obvviamente non possiamo andarle a controllare tutte.
	- Analisi dello spazio degli stati del modello PTPN ottenuto.
		Con questa analisi si verificano le proprietà che [abbiamo detto](Preemptive%20Time%20Petri%20Nets%20(PTPN).md#ANALISI).
### Documenti
Produce il Software Design Descripton (SDD). Per comporre questo documento:
- Si parte dai digrammi UML-MARTE come component-diagram, object-diagram (utile soprattutto per la parte non funzionale), class-diagram e activity-diagram (utile soprattutto per la parte funzionale).
- Da questi si deriva un modello intermedio detto timeline; questa fase è facoltativa, cioè si può inserire per maggiore completezza e chiarezza, ma si può anche passare alle PTPN. Per passare dalle timeline alle PTPN c'è quasi una relazione 1:1 e i costrutti sono [questi](da%20timeline%20a%20PTPN.pdf).
  Le timeline sono una rappresentazione semiformale (intuitiva) del funzionamento di un taskset; è più comprensibile per i non esperti rispetto ai diagrammi UML. Non supporta la verifica automatica del modello.
- Da questo si derivano le PTPN, cioè una rappresentazione formale del componente, che permette la verifica e la successiva fase di sviluppo. Usare tutti questi passaggi permette di diminuire la possibilità di errore nella costruzione delle PTPN (se fosse fatta a mano).
# IMPLEMENTATION
Corrisponde alla fase SD6.1-SW del V-Model. Ha uno scope di modulo.
Porta alle seguenti implementazioni di ogni CSCI:
- [Executable architecture](MIL-STD-498.md#Executable%20architecture)
- Entry point function
	Sono i comportamenti funzionali dei task, cioè quelli che li differenziano l'uno dall'altro.
### Executable architecture
Rappresenta i comportamenti non funzionali dei task, cioè il codice di controllo che al suo interno chiama le funzioni che definiscono il comportamento funzionale tramite gli entry point.
##### Esempi
- Acquisizione di semafori.
- Innalzamento e abbassamento della priorità dei task.
- Invocazione delle funzioni (le computazioni che compongono i task) tramite gli emtry point.
- [Esempio di implementazione](esempio%20implemetazione%20non-functional%20beheviour.pdf).
##### Generazione automatica
Quello che vogliamo ottenere è usare un tool che, a partire da dei modelli, genera il codice che realizza l'architettura eseguibile dei task.
Vogliamo inoltre che questo codice generato sia comprensibile per il designer del sistema: questo vuol dire che il tool potrebbe generare un codice intermedio a cui il progettista può rimettere mano in modo da realizzare funzionalità non supportate dal tool.
I class diagram di UML-MARTE realizzano la struttura dell'architettura eseguibile con gli stereotipi di MARTE. Avendo a disposizione questo formalismo si può non solo generare codice, ma anche controllare che ciò è stato generato corrisponda alle specifiche.
# TESTING
Corrisponde alla fase SD6.2-SW del V-Model. Ha uno scope di modulo.
Porta agli unit test, cioè al testing di ogni modulo all'interno dell'architettura eseguibile di ogni CSCI.
##### Metodi stub
In questa fase sono importati quelle funzioni definite stub: sono metodi senza un comportamento reale, ma che simulano l'utilizzo della CPU per un determinato periodo.
L'implementazione e quindi il testing del comportamento funzionale e non funzionale dei task avviene in contemporanea; avere dei metodi stub permette di poter testare il codice non funzionale senza dover aspettare quello funzionale.
Inoltre è possibile che, dopo l'implementazione del codice funzionale, un task non abbia un execution time distribuito in modo uniforme all'interno dell'intervallo definito dai suoi EFT e LFT (e solitamente neanche in quell'intervallo, ma in un suo sotto intervallo). Testando il comportamento dell'architettura eseguibile con l'implementazione funzionale vera e propria starei testando solo una parte di tutti i comportamenti previsti nelle specifiche; usando i metodi stub riesco invece a testare tutti i comportamenti previsti, anche se poi nella realtà non si verificano (con questa implementazione no, magari con usccessive però si). Una pratica diffusa è quella di testare le implementazioni funzionali in isolamento l’una dall’altra, cioè introdurre una sola implementazione funzionale insieme alle funzioni stub.
##### Logging
L'architettura eseguibile può essere loggata all'attivarsi degli eventi corrispondenti alle transizioni della PTPN. In questo modo si riesce a ricostruire gli execution time di ogni entry point function (e più in generale di ogni transizione che compare nel modello).
In questo modo si riesce a mettere in relazione il comportamento effettivo con le specifiche. Un aspetto che si evidenzia in questa relazione è simile a quello descritto nei [metodi stub](MIL-STD-498.md#Metodi%20stub): tramite le misure ottenute dai log riesco a definire come il tempo di completamento di task è distribuito all'interno dell'intervallo di firing (e poi stesso concetto dei metodi stub).
È importante che le operazioni di logging abbiano un tempo trascurabile rispetto alle transizioni; per questo non si usa il log su file, ma si usano strutture dati molto veloci (es: code) oppure si invia un segnale su una certa porta catturato poi da un sistema esterno.
# INTEGRATION
Corrisponde alla fase SD7.1-SW del V-Model. Ha uno scope di componente.
È un'attività fortemente supportata dalle PTPN.
Dopo la selezione dei test case si devono eseguire e verificare la copertura ottenuta rispetto allo State Class Graph (SCG) prootto dall'enumerazione di classi di stato.
### Selezione dei test case
##### Fallimenti
I fallimenti sono la deviazione di un componente dal servizio atteso. Sono le entità che dobbiamo scoprire con i test case.
Il secondo e terzo problema elencato sotto sono meno importanti, nel senso che potrebbero non causare problemi all'utente; vanno comunque risolti perché magari un giorno potrebbero diventare rilevanti.
- Mancamento della deadline
	È il problema più importante e l'unico che può effettivamente arrivare all'utente.
- Si eseguono le computazioni di un task in un ordine che è diverso da quello delle specifiche.
- Un parametro temporale prende un valore che non è all'interno dell'intervallo definito dalle specifiche. Ad esempio un rilascio che non avviene quando previsto.
##### Difetti
I difetti a un componente sono problemi che possono causare guasti al sistema. Sono ciò che dobbiamo scoprire con i test case.
- Task programming defect
	Sono difetti rispetto al controllo della concorrenza e all'interazione tra task. Ad esempio quando un'operazione di acquisizione di un semaforo non è combinata con l'inalzamento della priorità.
- Cycle stealing
	Rimozione delle risorse per via dell'arrivo di altri task.
##### Criteri
Le certificazione richiedono che il sistema soddisfi determinati criteri di copertura rispetto al grafo di controllo del flusso, che rappresenta tutti i path che un programma può attraversare.
Quindi a partire dallo State Class Graph (SCG) si definiscono i seguenti criteri:
- All-marking
	Si deve coprire ogni marcatura raggiungibile; ha una complessità proporzionale al numero di marcature raggiungibili.
	Garantisce la copertura di ogni possibile stato di concorrenza (cioè insieme di blocchi che sono running, ready, blocked).
	Per ogni marcatura $m$ si seleziona la classe di stati $S_m$ (con la marcatura $m$) e si include nel test suite (insieme di test case) ogni percorso che va da un controllable starting point (CSP) a $S_m$.
- All-marking-edge
	Si deve coprire ogni arco tra due marcature raggiungibili; ha una complessità proporzionale al numero di marcature raggiungibili per il numero massimo di eventi in una classe.
	Garantisce la copertura di tutte le possibili transizioni tra stati concorrenti; ad esempio operazioni su semafori o incremento e decremento della priorità).
	Per ogni marcatura $m_1$ e $m_2$ selezionare una classe $S_1$ (con la marcature $m_1$) con un evento che porti nella classe $S_2$ (con la marcatura $m_2$) e includi nel test suite uno percorso dal CSP che copre l'arco.
- All-classes
	Si deve coprire ogni classe di stati del grafo raggiungibile; ha una complessità maggiore di all-marking.
	Include all-marking.
- All-class-edge
	Si deve coprire ogni arco tra classi di stato raggiungibili; ha una complessità maggiore di all-marking-edge
	Include all-marking-edge.
- All-symbolic-run
	Si deve coprire ogni symbolic-run che inizia con il rilascio di un job e termina con il suo completamento o con la sua deadline mancata; ha una complessità maggiore di all-class-edge.
	Include all-class-edge.
- All-symbolic-execution
	Si deve coprire ogni sequenza di eventi che inizia con il rilascio di un job e termina con il suo completamento o con la sua deadline mancata.
	Riduce la complessità di all-symbolic-run.
# SYSTEM INTEGRATION/UTILIZATION
Corrisponde alla fase SD7.2-SW SD8 e SD del V-Model.
- SD7.2-SW
	Test e integrazione dei CSCIs, HCIs e FCIs all'interno di ogni unità.
- SD8
	Test e integrazione di ogni unità all'interno del sistema.
- SD9
