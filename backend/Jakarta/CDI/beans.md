Un bean è una classe Java che viene gestita dal [CDI](Context%20and%20Depencency%20Injection%20(CDI).md). Il container CDI scopre i bean appena vieene lanciato.
Il container CDI gestisce il suo ciclo di vita e la inietta nelle dipendenze (i.e., campi il cui tipo è quello del bean) di altre classi.
# Contesto
Ogni bean ha uno scope, cioè un contesto, che specifica il ciclo di vita del bean: definisce quindi quando viene creato, la sua memorizzazione e dopo quanto/cosa deve essere distrutto.
È interessante notare come all'interno del contesto dichiarato esiste una sola istanza del bean, cioè questo è un [singleton](Singleton.md).
##### Tipi
Ci sono 5 tipi di contesto che CDI propone:
- Applicazione
	I beans associati al contesto valgono per tutta la durata dell'applicazione. Si annota con $@applicationScoped$.
	Si può usare anche $@Singleton$, ma ha solo svantaggi ed è più vecchia.
- Session
	Valido per tutta la sessione [HTTP](HTTP.md) dell'utente. È disponibile solo in applicazioni web. Si annota con $@SessionScoped$.
- Request
	Valido per una richiesta HTTP fino alla relativa risposta. È disponibile solo in applicazioni web. Si annota con $@RequestScoped$.
- Conversation
	Valido per il tempo tra due eventi. Si annota con $@ConversationScoped$.
	Può succedere che per completare un'operazione si abbia bisogno di un numero di attività più lungo di una richiesta ma più corto di una sessione. Con questo contesto un beans può mantenere lo stato per tutto il flusso di attività.
	Questo tempo deve essere gestito in modo esplicito dal programmatore tramite la chiamata dei metodi $begin()$ e $end()$ su un oggetto che è stato dichiarato come $Conversation$.
- Dependent
	Il bean annotato con $@Dependent$ non ha un ciclo di vita proprio, ma dipende da quello dell'oggetto in cui viene iniettato. Un oggetto viene creato e distrutto nello stesso momento di quello in cui viene iniettato.
	È l'annotazione di default.
# Definizione
Vediamo come una classe può diventare un bean ed essere gestita quindi dal CDI.
- Scope
	La cosa più semplice è annotarla con un contesto. Sopra la dichiarazione (i.e., il nome) della classe si usa una delle annotazioni del contesto.
	Se non si specifica allora quello di deafult è $@Depend$.
- Requisiti
	Diventa un bean se la classe:
	- È concreta
	- È pubblica (o package-private)
	- Ha un costruttore void, cioè senza parametri. Il motivo di questo è che il CDI deve sapere come creare l'oggetto e se il costruttore prende dei parametri non sa cosa passargli. Per risolvere si può usare l'injection sul costruttore.
# Type set
Il type set di un bean definisce i tipi in cui il bean può essere iniettato. Il type set di un bean sono importanti per mettere in pratica la Type-Safe Resolution nel [CDI](Context%20and%20Depencency%20Injection%20(CDI).md#Conseguenze).
Ci sono delle regole a seconda della situazione:
- Supertipi
	Il bean può essere iniettato nella classe che lo definisce e in tutti i suoi supertipi, cioè le superclassi o le interfacce la classe bean implementa.
- Generici
	Se una classe bean estende una classe generica allora suo type set sarà dato da la classe bean stessa e il tipo generico definito nella dichiarazione della classe.
- Producer
	Se un bean è restituito da un metodo annotato con $@Producer$ allora il type set del bean è il tipo di ritorno del metodo e non quello della classe bean stessa.
##### Qualifier
Sono delle annotazioni definite dal programmatore che permettono al container di risolvere i problemi di injection typesafe relativi a oggetti che hanno lo stesso type set, cioè che estendono le stesse super classi e implementano le stesse interfacce.