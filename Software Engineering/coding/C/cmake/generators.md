Un build system è l'insieme di strumenti e regole per compilare il sorgente, collegare le librerie e generare l'eseguibile. [cmake](cmake.md) può generare un build system tramite il suo generator; potendo generare buil system diversi vuol dire che supporta più generator.
La variabile d'ambiente che definisce il generator usato è $CMAKE\_GENERATOR$. Se non viene specificata allora usare il generatoer di deafult, che in Mac e Linux è Unix Makefiles.
# Build type
La buil dype definsice il motivo per cui stiamo compilando il codice: se siamo durante lo sviluppo allora sarà $Debug$, se è per la produzione allora sarà $Release$, oppure altre cose.
Ci sono generator che definiscono single-configuration build system, quindi con un singolo obbiettivo; ci sono invece quelli che hanno più obiettivi.
La variabile d'ambiente da settare se si vuole specificare è $CMAKE\_BUILD\_TYPE$ per un generators single-configuration o $CMAKE\_CONFIGURATION\_TYPES$ per uno multi-configuration.