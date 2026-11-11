Il CDI è il Contexts and Dependency Injection. È una specifica di [Jakarta](Jakarta.md) ed è responsabile di:
- Dependency injection
	Inietta automaticamente le dipendenze nei [beans](beans.md), senza che si debba farlo manualmente.
	Questo permette di dividere i concetti di configurazione e uso degli oggetti tramite il [dipendency inversion principle](Dipendency%20inversion%20principle.md): una classe $School$ che ha un campo di tipo $President$ (questa è una dipendenza) non deve inizializzare tale campo; sarà infatti il CDI a farlo quando è necessario.
- Context management
	Tiene traccia del ciclo di vita di un bean (i.e., quando creare oggetti di quel tipo e quando distruggerli) in base allo scope del bean.
# Container
Il motore del CDI è un [container](container.md), piccolo ma che svolge tutto i suoi compiti.
In applicazioni server che seguono le specifiche [Jakarta](Jakarta.md) parte in autonomia e non c'è bisogno di fare nulla.
Se vogliamo invece usare il CDI in applicazioni stard dobbiamo usare container leggeri con Weld (di solito questo) o OpenWebBeans.
##### Funzionamento
Il container CDI quando viene avviato scansione le classi presenti nell'archivio (e.g., $.jar$ in Java) e cerca le annotazioni CDI definendo i [beans](beans.md) dell'applicazione.
Per la scansione prima era necessario un file di configurazione $beans.xml$, adesso non è più richiesto, ma viene usata una libreria di scanning o tool per il classloader per trovare annotazioni e configurazioni CDI.
# Conseguenze
Il meccanismo del CDI, nonostante possa essere complesso, ha molti vantaggi, tra cui:
- Loose coupling
	Una classe specifica solo il tipo che intende usare e lo annota, ma non lo implementa in alcun modo. Non si deve inoltre occupare di creare l'istanza o di gestirne il ciclo di vita, ma questo sarà totalmente sotto il controllo del CDI.
- Type-safe Resolution
	Il CDI gestisce le dipendenze in base al tipo e non a un qualche nome. In questo modo non si posso creare inconsistenze ma tutto sarà verificato staticamente e non ci possono essere errori di questo tipo a runtime.