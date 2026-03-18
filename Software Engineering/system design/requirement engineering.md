# Requisiti di sistema
Costituiscono le specifiche e la descrizione del sistema; sono quindi un contratto tra il cliente e gli sviluppatori.
Ci dicono cosa, perché e come costruirlo; il come non indica le cose pratiche e implementative.
### Tipi
- Functional
	Descrivono cosa il sistema vuole fare: solitamente sono le funzionalità, gli input, gli output e i dati da modellare, oppure il login.
- Not-functional - [[Not-Functional requirement.pdf|Diagramma]]
	Descrive come il sistemare farà le cose. Sono gli aspetti solitamente difficili da quantificare (Si deve fare attenzione a non descriverli in modo soggettivo): safety, security, reliability e performance; solitamente questi requisiti generano dei requisiti formali per definire il funzionamento del sistema.
	Non influiscono direttamente sul componente e sulla sua implementazione quanto più sull'architettura del sistema (ad esempio per rispettare le performance si dovrebbe non far comunicare troppo i processi).
### Livelli di astrazione
Le specifiche si suddividono in più livelli di astrazione:
- User requirements
	Sono le specifiche definite con l'utente. Si usa solitamente un linguaggio naturale e dei diagrammi.
- System requirement
	Si espandono le specifiche definite nel livello superiore in sotto specifiche, in modo che siano più utili agli sviluppatori per vederne i dettagli.
	Si definiscono i servizi e le operazioni che devono essere implementate.
- Component requirement
	Se necessario, cioè se il progetto è molto grande, si espandono ancora definendo le specifiche dei singoli componenti.
##### Correlazione
Le specifiche ai vari livelli devono essere correlate sia top-down che bottom-up attraverso una matrice di tracciabilità: si deve collegare ogni requisito del livello superiore con quelli in cui questo è stato espanso e viceversa.
In questo modo ogni cosa è documentata e non si rischia di aggiungere requisiti non richiesti.
# Requirement engineering process
È il processo che porta alla documentazione e al mantenimento dei requisiti.
Il motivo di questa disciplina sono i costi: correggere errori introdotti in questa fase costa tanto di più quanto il progetto è andato avanti.
![[Requirement engineering process.png]]
##### Elicitation and analysis
- È un colloquio col cliente che coinvolge il project manager e lo staff tecnico, sia sviluppatori che architetti.
- Si basa su un ciclo di quattro fasi: la scoperta dei requisiti, la classificazione, la prioritizzazione e la specifica.
- Nella fase iniziale si usa l'astrazione degli [[Use-case diagram]], un tipo di diagramma UML.
- Problemi
	- I clienti di solito non sanno cosa vogliono.
	- Se ci sono più clienti si possono aver più richieste e anche contrastanti.
	- Le regole interne ed esterne possono portare a una modifica dei requisiti.
	- Il fatto che in questa fase ci siano persone con ruoli diversi può portare ad avere terminologie diverse e a un'incomprensione.
##### Specification
- I requisiti devono essere specificati in un documento; lo [[SRS template (IEEE).pdf|standard]] (IEEE) di questo documento è vecchio ma il suo template è ancora molto usato.
- Scrittura
	Non si deve usare un gergo troppo tecnico. Si deve indicare cosa andiamo a costruire e non come, cioè si devono evitare i dettagli implementativi.
	- Naturale
		Solitamente non è una prosa, ma ci sono delle regole e dei template da seguire.
	- Grafici
		Sono modelli grafici che esprimono soprattutto i requisiti funzionali. Un esempio sono gli [[Use-case diagram]].
	- Matematica
		Sono notazioni complesse basate su concetti matematici come le macchine a stati finiti. Il problema è la loro comprensione: ci sono poche persone, sia clienti che addetti ai lavori, che conoscono tali linguaggi.
- Ci sono delle regole per indicare l'importanza delle specifiche:
	- Must (Mandatory)
		È qualcosa che deve essere incluso nel progetto. Si usa anche must not per indicare qualcosa che assolutamente non ci deve essere.
	- Should (Reccomended)
		È qualcosa che dovrebbe essere incluso nel progetto. Stessa cosa di cui sopra per should not.
	- May (Optional)
		È da implementare solo se resta tempo.
- Il documento delle specifiche è utilizzato da:
	- Client
		Ciò che sarà implementato è totalmente scritto nei documenti; se qualcosa manca o è sbagliato allora deve segnalarlo.
	- Manager
		Usa il documento per pianificare il ciclo di sviluppo.
	- Software/System developers
		Devono implementare ciò che sta dentro il documento.
##### Validation
- Si deve dimostrare che i requisiti rispettino effettivamente cosa il cliente vuole. Il costo di un errore a questo punto del progetto ha un costo enorme, perché impone di iniziare da capo.
- Non ci devono essere requisiti in conflitto.
- Le specifiche dei requisiti sono SMART:
	- Specifiche
		Non deve essere prolisse, ma andare subito al punto. evono essere inoltre complete.
	- Misurabili
		Le performance devono essere misurabili 
	- Achievable (Realizzabili)
		Si traduce col fatto che si deve poter realizzare dei test.
	- Reali
	- Tracciabili
		Tracciabili nel senso della matrice di tracciabilità.
# Cambiamenti
Durante lo sviluppo del software ci saranno dei cambiamenti dei requisiti.
Si deve cambiare la loro tracciabilità e si devono inserire i cambiamenti nella documentazione.