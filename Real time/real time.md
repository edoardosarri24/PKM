A system is defined as real-time if, in addition to the functional requirements, it has also a timing requirements.
# Tipologie
I sistemi real-time si differenziano in base al livello di garanzie che devono fornire. Non sono classi vincolate, ma posisamo avere uno spettro all'interno dei due estremi.
##### Soft
Mancare una deadline causa un decadimento della qualità del servizio.
##### Hard
Mancare una deadline causa il fallimento del sistema e può provocare la morte di persone.
# Determinismo
Più i requisiti di tempo sono stringenti più il sistema deve gestire i [task](Real%20time/task.md) in modo deterministico. Questo porta a ridurre la concorrenza (i.e., risorse allocate dinamicamente), porta ad allocare allocare le risorse in fase di progettazione e quindi ad usarle in modo molto [conservativo](timing%20analysis.md#Consevatività).
##### Problemi
Ciò che può minare la predicibilità è:
- Multitasking
	Eseguire più task in parallelo può portare a ritardi indefiniti (unbounded).
- Scheduling basato su priorità
	Può portare al problema del priority inversion.
	Può succede che un task a priorità più alta sia bloccato da uno a priorità più bassa: se un task con priorità 1 attende una risorsa condivisa e al momento usata da un task a priorità 20 e arriva un task a priorità 10 che non condivide risorse con nessuno, allora il controllo andrà a quello con priorità 10 e quello con priorità 1 dovrà aspettare sia quello con cui condivide la risorsa (situazione legittima) sia quello con priorità più bassa (situazione non voluta).
- Interruzioni esterne
	Se si vuole permettere le interruzioni dei task a favore di input esterni si implementa un sistema più reattivo, ma si possono verificare ritardi indefiniti.
	Solitamente un embedded system gestisce un solo aspetto e ha un preciso scopo, cioè non deve essere reattivo.
- DMA (Direct Memory Access)
	Se permettiamo di accedere alla memoria principale senza passare dalla CPU, si velocizza il processo di accesso alla memoria ma ci possono essere dei ritardi indefiniti quando la CPU vuole accedere alla memoria insieme a qualcun altro.
	Se vogliamo il real-time ci servirà schedulare i tempi di accesso alla memoria tra CPU e altri; se fatto staticamente allora si possono sprecare risorse.
- Memoria
	Se si gestiscee la memoria staticamente si riuscirà a garantire maggiormente il real-time, ma si avrà uno spreco di risorse.
- Comunicazione e sincronizzazione
	Causano ritardi indefiniti se non si usano dei protocolli validi, cioè se non si decide quando far parlare i vari componenti.
# Timing behaviour
Il timing behaviour di un programma composto da [task](Real%20time/task.md) deterministici dipende da due aspetti:
- Input
	La dimensione dell'input spesso è positivamente correlata con il tempo di esecuzione.
- Hardware
	Lo stato dell'hardware è composto da cache, valore dei registri e stato dei buffer.
	Una piattaforma hardware è detta essere time-predicatable se il tempo di esecuzione del task non dipende dallo stato dell'hardware, altrimenti è detto time-randomised.