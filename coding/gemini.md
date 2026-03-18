Gemini può essere usato anche con il comando di pipe $\texttt{|}$ in script.
# Contesto
Per gestire il contesto possiamo vedere cosa l'agente sa sul progetto con il comando $\texttt{memory}$.
- Se volgiamo fare forza l'agente a ricaricare i file di contesto si usa $\texttt{refresh}$.
# Chat vecchie
Si usa il comando $\texttt{resume}$.
# Planed-based
Invece che implementare direttamente quello che ci serve, possiamo stipulare un piano e poi farlo eseguire al modello.
- Conductor
	È un'[estensione](#Estensioni). È da usare quando vogliamo il massimo della struttura: il piano resta anche a fine sessione.
- Plan mode
	È una modalità interna all'agente. Pianifica ed esegue, ma noi siamo esterni a questa procedura; possiamo vedere a che punto l'agente è durante l'esecuzione con $\texttt{Cntr+T}$.
	Per attivarla possiamo usare il linguaggio naturale oppure eseguire il comando $\texttt{plan}$.
# Estensioni
Dopo averla installata ogni estensione ha i suoi comandi.
- [Conductor](https://github.com/gemini-cli-extensions/conductor)
	Permette di seguire un approccio documentation-first sul progetto.
	I comandi principali sono:
	- $\texttt{/conductor:setup}$: Per inizializzare il contesto del progetto. Crea file specifici per architettura librerie e come usarle.
	- $\texttt{/conductor:newTrack "Titolo Feature"}$: Per creare le specifiche e il piano d'azione.
	- $\texttt{/conductor:implement}$: Per far partire l'agente nell'esecuzione dei task pianificati.
# Skill
È un modo per specializzare l'agente in compiti precisi e che gemini attiva automaticamente. Si basa su uno standard e quindi è anche cross model.
Si puà creare a mano oppure chiedere a gemini di crearlo. Quello che succede è che viene creata una cartella $\texttt{my\_skill}$ con almeno il file $\texttt{SKILL.md}$; questo è uno yaml che definisce il tutto.