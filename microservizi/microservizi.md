Un micro servizio è un deployment che esegue all'interno di un singolo processo, isolato rispetto a tutti gli altri e che rappresenta una singola funzionalità di business.
- La singola funzionalità di business è detta bounded context. In un'applicazione monolitica spesso si avevano bounded context dipendenti: spaghetti code.
- Tecnicamente più micro servizi potrebbero girare in un solo ambiente, ma questo porterebbe a una complicata gestione delle risorse, degli aggiornamenti, delle versioni delle dipendenze comuni a più micro servizi e alla scalabilità. Isolare un micro servizio (solitamente in un [container](container.md)) è più un aspetto architetturare che tecnico.
# Vantaggi
- I micro servizi sono indipendenti l'uno dall'altro. Possono essere testati, aggiornati e duplicati senza accoppiamento.
- Ogni micro servizio realizza un preciso caso d'uso e può essere più facilmente utilizzato da altri micro servizi, rendendo un sistema più modulare.
- Un micro servizio non deve avere per forza il suo frontend, ma può servire solo da utilità per altri micro servizi.
# Svantaggi
- La latenza aumenta al crescere delle interazioni tra micro servizi. Per questo motivo si deve trovare un trade off tra numero e dimensione dei micro servizi. Si può risolvere con canali privilegiati tra alcuni micro servizi.
- Come ci dice anche il [CAP theorem](CAP%20theorem.md), uno dei più grandi problemi è la consistenza del dato. Visto che i vari micro servizi hanno data base diversi è difficile mantenere in ogni momento tutti i data base aggiornati. Si può mitigare con un'architettura asincrona o pattern di persistenza distribuita.