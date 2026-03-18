In un sistema [real time](real%20time.md) per verificare i [vincoli di tempo](real%20time.md#Timing%20behaviour) è necessario capire i comportamenti temporali dei [task](Real%20time/task.md). Nella timing analysis vogliamo studiare se i task completano la loro esecuzione prima dell'arrivo della rispettiva deadline; per fare questo solitamente si utilizzano algoritmi come la [responce time analysis](responce%20time%20analysis.md) basati sul tempo di esecuzione dei task.
# Scenari
Lo studio delle caratteristiche temporali apre a due scenari:
- [Hard real-time](real%20time.md#Hard)
	I vincoli temporali devono essere tutti rispettati per ogni [job](Real%20time/task.md#Jobs) del task. In caso contrario si hanno eventi catastrofici.
	Il rispetto dei vincoli temporali è un concetto binario ed è molto stringente: per metterci nel caso pessimo e passare all'algoritmo (e.g., [responce time analysis](responce%20time%20analysis.md)) i dati corretti si utilizza come tempo di esecuzione dei task il [WCET](#WCET), e come tempo in cui il task viene rilasciato il suo [istante critico](istante%20critico.md#Hard%20real-time).
- [Soft real-time](real%20time.md#Soft)
	I task possono sforare le deadline; a seconda del livello di sicurezza richiesto parliamo di una perdita di qualità del servizio oppure di una perdita di denaro.
	In queste situazioni i sistemi possono continuare a funzionare anche se i task occasinamelmente sforano le proprie deadline; ci sono delle norme (e.g., IEC-61508 e ISO-26262) che ci definiscono questi limiti tramite un upper bound probabilistico.
	Essendo quindi un concetto probabilistico si utilizza spesso una [probabilistic timing analysis](probabilistic%20timing%20analysis.md): vogliamo una probabilità di fallimento bassa (anche molto bassa), ma non necessariamente zero.
# Difficoltà
Nel fare timing analysis in sistem real-time, ci sono spesso più problemi:
##### WCET
Oggi i processori moderni implementano acceleratori (e.g., branch prediction, out-of-order execution, write-buﬀers and gerarchi di memorie) che rendono il compito di stabilire il WCET molto complesso. Infatti questi processori tendono a ottimizzare il tempo di esecuzione medio e non quello pessimo, facendo si che eseguendo più volte il codice su input diversi si abbia una varianza più alta rispetto al passato. Diventa in questo modo ancora più complesso trovare i path e gli input con cui stimare il WCET.
##### Consevatività
Spesso il tempo di esecuzione reale è molto inferiore a quello stimato nei worst-case bound. Questo è un compromesso che nei sistemi real-time ci va bene accettare perché vogliamo un alto livello di [determinismo](real%20time.md#Determinismo), ma spesso è troppo elevato e le risorse sono usate nel caso medio molto meno di quello che potrebbe essere fatto.
Per cercare di risolvere questo problema si stanno valutando anche tecniche di AI, come ad esempio metodi [simbolici](https://github.com/edoardosarri24/violation-prediction-in-realtime-system).
# WCET
Il Worst Case Execution Time (WCET) è un upper bound del tempo di esecuzione che un programma impiega su ogni input valido e nello stato iniziale dell'hardware.
Ci sono tre tecniche per trovare il WCET.
# Tecniche
Per determinare il WCET ci sono varie tecniche.
##### Static analysis
Si vuole trovare il WCET senza mai eseguire realmente il programma, né sull'hardware né in un simulatore.
Ci sono diversi problemi: è difficile avere un modello dell'hardware, soprattutto per processori moderni; l'analisi produce un WCET molto pessimistico.
Si basa su tre step:
- Flusso di controllo
	Si utilizza il control flow graph per identificare i possibili percorsi di esecuzione.
- Micro-architetturale
	Si utilizza un modello astratto dell'hardware per stimare come le caratteristiche del processore in question (e.g., cache e registri) influenzino il tempo di esecuzione sui percorsi identificati.
- Percorsi
	I risultati delle fasi precedenti vengono combinati per calcolare il WCET.
##### Measurement-based analaysis
È lo standard che usi nell'industria.
L'idea è di eseguire il programma in hardware o in un simulatore molto accurato (a livello di clock) e di derivare il candidato WCET come il tempo più alto ottenuto; a questo punto si può aggiungere un 20% (misura trovata empiricamente in molti studi) o di considerare il candito come un lower bound al WCET.
Il problema più complesso di questa tecnica è definire il measurement protocol, cioè l'algoritmo che ci restituisce quali dati di input debbano essere utilizzati per ottenere le misure.
##### Hybrid analysis
Vogliono unire le due tecniche precedenti.