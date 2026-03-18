In una classica applicazione Java quando vogliamo vedere delle modifiche solitamente dobbiamo ricompilare tutto, eseguire i test almeno una volta e far ripartire l'applicazione; se siamo in un contesto distribuito dove si utilizza un application server, si deve rifare il deploy sul server.
Tramite le dev mode di [quarkus](quarkus.md) tutto questo processo è sostituito da un refresh del localhost sul browser. Tutto questo non funziona solo per cambiamenti al codice sorgente, ma anche per variazioni nei file di configurazione (e.g., $appliation.properties$), nel frontend o nelle dipendenze.
La development mode si attiva tramite il comando $\texttt{quarkus dev}$ da terminale.
# Test
Quando siamo nella dev mode i test vengono eseguiti automaticamente in background: se cambiamo qualcosa allora dal terminale possiamo vedere subito del rosso, che indica che qualcosa non funziona più nei testi.
Per cambiare l'esecuzione automatica dei test in background si usa il comando $\texttt{r}$ una volta che siamo nella dev mode.
# Errori
Se introduciamo degli errori sintattici nel codice ed eseguiamo un refresh della pagina, allora notiamo una pagina di errore. In particolare avremo un [codice HTTP 500](HTTP.md#Codici%20di%20stato).
# Funzionamento
Il ricaricamento del progetto non avviene quando un file viene salvato dopo un cambiamento, ma solo quando il server riceve una nuova richiesta e ci sono stati dei cambiamenti a qualche parte sistema.
La modalità dev utilizza una [JVM](building.md#JVM) per completare il building. Quando facciamo il refresh non viene inizializzata una nuova JVM, ma le modifiche vengono caricate nell'istanza già in uso.
##### Tempi
Dopo che il sistema è stato ricaricato i tempi per rispondere a una richiesta sono leggermente più lunghi, proprio a causa di tutto quello che succede dietro.
In ogni caso si parla comunque di centinaia di millisecondi.
##### Stato
Quando viene eseguito il ricaricamento del sistema, lo stato precedente viene perso.
Per ovviare leggermente a questo "problema", si può eseguire un $instrumental-based\ reload$: in questo modo il tempo di ricaricare il sistema è inferire, perché la JVM non cambia istanza e non viene avviato il garbage collector. Questa configurazione è disattivata di default, ma si può attivare con il comando $\texttt{i}$ quando siamo nella dev mode.
È una possibilità solo quando cambiamo il comportamento del sistema, cioè quando modifichiamo il corpo di uno o più metodi; se cambiamo altri dettagli (e.g., le variabili) allora il sistema sarà caricato da capo. Questa impostazione di sicurezza è dovuto al possibile comportamento errato del sistema se venissero cambiate alcune sue parti e l'applicazione non venisse rilanciata da zero. Se l'impostazione è attivata dal terminale della dev mode ci viene indicato dopo il refresh il tipo di reload che è stato fatto.