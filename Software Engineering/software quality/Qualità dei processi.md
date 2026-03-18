Qualità -> grado col quale un insieme di caratteristiche inerenti ad un oggetto soddisfano i requisiti.
Quality managment -> managment con attenzione alla qualità:
- quality policies: intenzioni e direzioni di una organizzazione in relazione alla qualità;
- quality objectives: step misurabili nel raggiungimento della qualità;
- processes;
- quality planning: impostazione degli obiettivi di qualità;
- quality assurance: fornire fiducia nel fatto che la qualità richiesta sarà soddisfatta;
- quality contol: ha come scopo soddisfare la qualità richiesta;
- quality improvement: incrementare la capacità di soddisfare la qualità richiesta.
Nelle aziende un prodotto deve essere buono abbastanza: non deve essere perfetto, altrimenti si spende troppo nella qualità; se è di qualità scadente nessun cliente lo vuole.
Per fare questo è necessario definire dei processi, cioè dei modi di fare le cose (attività generali che hanno delle loro caratteristiche e requisiti); questi ci fanno perdere tempo, ma senza di essi, soprattutto in grandi progetti, si farebbe molta confusione. Possono essere di più tipi:
- Primari
	Sono lo sviluppo e il mantenimento del software.
- Supporto
	Sono applicati in modo intermittente o costante e sono di supporto a quelli primari. Esempi sono la verifica, la validazione e la quality assurance (Concetto generale per esprimere contemporaneamente la qualità dei processi e del SW).
- Organizzativi
	Sono di supporto per la software engineering. Esempi sono la formazione e i processi di analisi della qualità.
## VALUTAZIONE
La valutazione della qualità non è riferita solo alla parte di sviluppo, ma si vuole valutare anche i [processi](Qualità%20dei%20processi.md#Processi) sono collegati all'azienda o al progetto.
Le valutazioni possono essere **qualitative** o **quantitative**: le prime sono giudizi di esperti, le seconde hanno risultati numerici (Ad esempio il numero di bug per tempo).
Le motivazioni legate al raggiungimento di certi livelli di qualità possono essere diverse, tra cui lo **sviluppo di capacità di valutazione**, per individuare processi da migliorare, e la **valutazione della maturità del processo** per concludere contratti o ottenere premi. 
- Quando si parla di assessment si intende valutare in modo informale se il nostro software soddisfa un determinato criterio di qualità (Cioè un modello), secondo la valutazione di un esperto. Determinano il livello di capacità o maturità per identificare i processi software da migliorare.
  Questo può essere interno o esterno all'azienda; nel secondo caso (valutazione della capability) solitamente è un possibile acquirente che sta valutando il nostro livello di competenza per poi assegnarci un lavoro.
  I criteri di assessment sono visti come buone pratiche per i processi SW.
- Gli audits invece sono valutazioni formali. L'esperto, che in questo caso è esterno, valuta se il prodotto soddisfa degli standard rigidi. Sono ispezioni statiche: non si guarda il prodotto in esecuzione, ma lo si confronta con gli standard.
## MIGLIORAMENTO
Per migliorare un processo si devono fare delle iterazioni di cicli per il miglioramento continuo basati sulla misurazione, analisi e cambiamenti.
Un esempio di modello è il Plan-Do-Check-Act: si individua cosa si vuole migliorare; si introducono i cambiamenti, inclusi il cambio di personale e la formazione; si valuta i nuovi risultati; si implementano le modifiche.
È necessario effettuare avere un sistema di valutazione continuo (continuous rating) che assegna una valutazione ad **ogni** processo SW di interesse e un sistema di valutazione graduale (staged rating) che assegna una valutazione **globale** ad un insieme di processi SW.
### Misura
Quando si vuole fare un miglioramento o una modifica è fondamentale la misura di ciò che si sta abbandonando (misura del sistema corrente), in modo da poter poi fare un paragone con quello che si è introdotto.
- Efficienza (Efficiency)
	È il rapporto tra le risorse consumate e quelle che avremmo voluto consumare in relazione ad un processo SW, un'attività o un task. Si misura quanto un processo sia in grado di raggiungere il risultato utilizzando al meglio le risorse. La misura che si usa di solito è il costo.
- Efficacia (Effectiveness)
	È il rapporto tra l'output vero e quello atteso prodotto da un processo SW, un'attività o un task. Si misura quanto un processo sia in grado di produrre quello che si vuole. (ex. difetti totali, densità dei difetti, qualità dei requisiti, documentazione della progettazione, ecc.).
Bassa efficacia ed efficienza possono essere causati da personale inesperto, mancanza di tool e infrastrutture, utilizzo di domini non familiari.
La qualità delle misurazioni dei processi è determinata soprattutto dalla affidabilità e validità dei risultati delle misurazioni. Potrebbero altrimenti essere fuorvianti e condurre a scelte/iniziative sbagliate.
Per costruire un modello **analysis-driven** si deve innanzitutto effettuare lo **sviluppo**, stabilendo potenziali trasformazioni di input nell'output desiderato, per poi effettuare la **calibrazione**, aggiustando i parametri nel modello per farli corrispondere ai risultati dei progetti passati, per poi infine effettuare la **validazione**, confrontando i risultati ottenuti dalle computazioni per insiemi di dati tra loro similari.
Le tecniche di misura dei processi sono usate per collezionare dati quantitativi e qualitativi, che verranno poi analizzarli e trasformati in informazioni utili per comprendere dove effettuare miglioramenti.
## COSTI
I costi di un processo e la qualità del prodotto ottenuto sono strettamente collegati. Un'organizzazione deve bilanciare bene i costi interni ed esterni (Vedi sotto) e posizionarsi a metà: non deve rilasciare un prodotto perfetto né un prodotto con troppi errori. Abbiamo:
- Costi di prevenzione
	Sono investimenti nel miglioramento dei processi, negli strumenti e nella formazione.
- Costi di valutazione
	Sono i costi dei processi che si occupano di trovare gli errori.
- Costi interni
	Sono i costi che vanno sostenuti prima del rilascio del prodotto per correggere eventuali errori trovati in fase di valutazione.
- Costi esterni
	Sono i costi che vanno sostenuti per sistemare problemi del SW dopo che è già stato distribuito ai clienti.