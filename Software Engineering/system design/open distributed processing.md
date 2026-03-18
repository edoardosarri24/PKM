Si tratta di uno stardard (ISO 10746) architettura utilizzato nella fase di [design](system%20design.md#Design) dello sviluppo di un sistema.
# Viewpoint
L'ODP fornisce punti di vista diversi dello stesso sistema che dovrebbero essere analizzati: ogni viewpoint analizza una parte del sistema (non per forza una partizione); mettendo insieme tutte queste parti si ha maggiore confidenza di aver analizzato tutto il sistema.
I viewpoint sono:
- Enterprise
	È composto dall'ambiente con cui il nostro sistema interagisce. Può essere sia l'hardware dove viene eseguto il software, l'ambiente fisoc dove è contenuto l'hadrware o le persone che interagiscono con il sistema.
- Information
	Identifica i dati che il sistema tratta.
- Computational
	Analizza la parte funzionale, cioè come il sistema manipola i dati.
- Engineering
	Si anazza la aprte non funzionale: sicurezza, rete o il deployment su nodi.
- Technology
	Definsice quali siano le tecnologie da utilizzare.