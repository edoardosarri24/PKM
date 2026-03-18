MicroProfile Config è una specifica core di [MicroProfile](MicroProfile.md#MicroProfile%20platform) che permette di disaccoppiare il codice dalle configurazioni dell'applicazione: l'applicazione può accedere a un configuration source e caricare le configurazioni opportune senza che esse siano hard coded.
# Motivazioni
Spesso la nostra applicazione utilizza configurazioni diverse durante diverse fasi della progettazione: nella fase di sviluppo si usa un database open source, nella fase di integrazione si usa un databsae economico, mentre nella fase di produzione si usa un database enterprise.
# Accesso
Le configurazione nel configuration source sono stringhe in formato chiave-valore. Quando sono caricare nel codice, queste diventano oggetti Java e sono gestite da oggetti di topo $\texttt{Config}$.
Ci sono due modi per caricare le configurazioni:
- programmatic API
	Si accede agli ogetti $\texttt{Config}$ direttamente tramite il metodo statico $\texttt{getConfig()}$.
- CDI injection
	Si inietta tramite il [CDI](Context%20and%20Depencency%20Injection%20(CDI).md) tramite opportune annoazioni.