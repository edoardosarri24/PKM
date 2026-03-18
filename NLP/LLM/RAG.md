Il Retrival Augmented Generation (RAG) è una tecnica usata nella generazione in un [LLM](LLM.md) che mitiga i seguenti problemi:
- Allucinazioni
	Si parla di allucinazione quando un modello inventa del testo che non è conforme alla realtà; non si tratta di un errore, ma proprio di invenzione.
- Attribuzione
	Un modello non riesce a collegare quanto generato con la fonte che lo afferma.
- Stantezza
	I modello sono aggiornati fino al momento in cui il dataset del pre-training viene costruito. Se vogliamo dargli altro contesto non potremmo.
- Custumizzation
	Si vuole personalizzare le risposte del modello in base all'utente che lo sta utilizzando.
# Architettura
![RAG architecture](RAG%20architecture.png)
Solitamente l’utente fornisce un prompt (i.e., giallo nell'imagine), il modello (i.e., blu nell'immagine) genera qualcosa e ci produce un output.
Il RAG introduce altri componenti a monte della generazione:
- Query encoder
	Prende il prompt dell'utente e lo codifica in un qualche modo.
- Doc e doc encoder
	L'utente da in input anche dei documenti, che sono in questo momento codificati secondo una qualche codifica.
- Retriver
	Si tratta di un database vettoriale. Permette di fare [information retrival](information%20retrival.md) e recuperare quei documenti che sono più vicini (e.g., distanza del [coseno](Distanza.md#Coseno)) al prompt. Questi documenti vengono passati come contesto al modello che li usa per generare.
##### Osservazioni
- I documenti ritornati non devono essere maggiori del contesto che l'LLM può accettare e gestire. Si può usare un [modello spaziale](modelli%20IR.md#Vectorial%20space%20model) e filtrare il tutto passando solo i primi $k$ documenti.
- Essendo il database vettoriale, le operazioni su vettori sono fatte molto velocemente dalle [GPU](GPU.md).
# Tecniche
Ci sono molte tecniche per fare RAG.
##### Frozen RAG
È una tecnica statica, la parte di retrival non viene imparata in nessun modo; molto spesso è sufficiente per aumentare di molto le prestazioni.
Il retrival in questo caso può essere fatto in più modi:
- In modo sparso tramite un modello di IR, come l'utilizzo del bag of words dove i pesi sono dati dall'[IF-IDF](modelli%20IR.md#Vectorial%20space%20model)). Utilizza solo le parole prsenti nei documenti per associare documenti e query.
- In modo denso trasformando i documenti in embedding. La distanza tra query e documenti si basa sulla loro semantica.
##### RePlug
L'idea è allenare il retrival (e mantenere statico l'LLM) a scegliere quei documenti che migliorano la generazione dell'LLM.
Per addestrare il retrival si fa così: il retrival campiona da una distribuzione dei documenti da passare all'LLM come contesto; si calcola la probabilità di generare la risposta corretta condizionata a ognuno dei documenti usati come contesto; il retrival imparare quali documenti sono efficaci con quel tipo di query.
- Il retrival si adatta in basa all'LLM che utilizza i documenti; se il modello cambia allora si deve riaddestrare il retrival.
##### FLARE
Con FLARE si vuole capire quando fare il retrival: nel RAG normale si fa all'inizio, ma questo è inefficiente se il modello conosce già tutto quello che serve per rispondere al prompt; inoltre potrebbe essere utile fare retrival di informazioni diverse in momenti della generazione diversi.
L'idea è quella di far generare al modello $n$ token e valutare la probabilità di quanto generato. A questo punto se il modello è molto confidente (i.e. la probabilità di tale sequenza è alta) si continua, altrimenti si esegue il retrival, si passa del contesto all'LLM e poi si rigenera la frase.
- Permette di fare il retrival in modo efficiente, cioè solo quando c'è bisogno.
- Mitiga le allucinazioni perché il contesto interviene proprio quando il modello non è sicuro di quello che sta producendo.