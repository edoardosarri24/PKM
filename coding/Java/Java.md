Java è una piattaforma che ha lo scopo di rendere indipendente l’esecuzione di un’applicazione dall’HW sottostante. Questo permette di non ricompilare il codice ongi volta che lo vogliamo portare su più disposivi. In questo modo si può scrivere il codice in un linguaggio compatibile con JVM (Java o Scala); viene compilato il bytecode, che non è specifico di nessun OS; viene eseguito dalla JVM attraverso l’interprete; questo può tradurre il bytecode in codcie macchina (Compatibile a questo punto con l’HW sottostante) una volta sola oppure just-in-time. Si basa sulla [Java Platform](Java%20Platform.md).

- Java è un linguaggio con [tipizzazione statica](Tipizzazione.md).
- Esegue chiamate per [riferimento](Chiamata%20funioni%20(OOP).md). Viene passato il valore della locazione di memoria dove si trova l’oggetto.
- [Teoria](Teoria%20Java.md)
## LIBRERIE STANDAND
Forniscono i componenti essenziali, cioè le API, per sviluppare applicazioni.
Le librerie sono organizzate in package; ogni package contiene un insieme di altri pacchetti, interfacce e classi correlate che forniscono funzionalità specifiche. Da specificare che i pacchetti non sono nested: pacchetti con prefissi uguali non sono correlati.
- $java.lang$ contiene gli elementi di base per la programmazione Java.
	- Interfacce
		- Appendable
			Da implementare se un ad un oggetto si possono aggiungere valori e seguenze di caratteri.
		- $Readable$
		- $AutoCloseable$
			Da implemetare se un oggetto può contenere risorse finchè non viene chiuso. Viene chiamato in automatico il metodo $close$ all'uscita di un blocco try-with-resources.
		- $Cloneable$
			Se non implementata alla chiamata del metodo clone viene sollevarta un’ecceezione.
		- Comparable
		- Iterable
		- Runnable
	- Classi
		- $Object$
		- $Class$
		- Classi wrapper per i valori primitivi.
		- $StringBuffer$ / $StringBuilder$
			Stringa a grandezza dinamica. $StringBuffer$ da usare se c’è la concerrenza (Le chiamate vengono eseguite nell’ordine in cui sono state fatte); $StringBuilder$ è più efficiente perchè non gestisce la sincronizzazione.
		- $Math$
			Le operazioni sono implementate in modo efficiente, ma il risultato può non essere bit a bit; se si vuole precisione massima usare la classe $StrictMath$.
		- $Enum$
			È la classe bae per tutte le enumerazioni.
		- $System$
			Gestire flussi di input (Variabile statica $in$), di output (Variabile statica $out$) e di errore (Variabile statica $err$) standard.
		- $Thread$
		- $Throwable$
		- $StackTraceElement$.
- $java.util$ contiene il framework delle collezioni.
	- Interfacce
		- $EventListener$
			Interfaccia di marcatura per tutti i listener.
		- $Formattable$
			Da implemetare se si vuole poter eseguire una formattazione personalizzata.
		- $Spliterator$
			Serve per suddividere il processo di iterazione di un aggragato di oggetti (Utile con più thread). Per operare con uno $Spliterator$ si usa la classe $Sliterators$.
	- Classi
		- $Objects$
		- $Optional$
		- $Calendar$
			Serve per manipolare e gestire un calendario (Giorno e orario) su lungo perido. Se si vuole più flessibilità e si vuole un oggetto di tipo $Calendar$ immutabile allora si deve usare $Calendar.Builder$.
		- $Locale$
			Rappresenta una specifica regione geografica, politica o culturale. Si usa per adattare le informazioni all'utente. Per costruirlo in maniera safe usare $Locale.Build$.
		- $Date$
			Obsoleta. Sostituita con $LocalDateTime$.
		- $TimeZone$
			Obsoleta. Sostituita con $ZonedDateTime$.
		- $Scanner$
			Semplice scanner di testo che elabora tipi primiti e stringhe.
		- $Formatter$
		- $Random$
			Genera numeri pseudocasuali (Generati a partire da un seme; se il seme è lo stesso allora anche i numeri sono gli stessi). Le sue istanze sono thread-safe, ma per maggiore efficienza usare $ThreadLocalRandom$. Le sue istanze non sono sicure dal punto di vista crittografico e per questo usare $SecureRandom$.
		 - $Currency$
		- $Timer$
			Usata per pianificare attività. Le attività affidate ad un $Timer$ sono eseguite su un unico thread. Se si vuole più flessibilità e più effficienza usare $ScheduledExecutorService$.
    - java.util.random
        - _RandomGenator_ → Interfaccia che accomuna chi genera valori pseudocausali. In generale non sono oggeti thread-safe; per questo usare _Random_ o _ThreadLocalRandom_. Le sue istanze non sono sicure dal punto di vista crittografico; per questo usare _SecureRandom_.
        - _RandomGeneratorFactory_ → È una fabbrica di generatori pseudocasuali alla uqale si può specificare che tipo di algorimito deve usare il generatore.
    - java.util.function → Contiene tutte le interfacce funzionali
- java.time
	Sono le API poer date, orari, istanti e durate. Tutte le classi sono immutabili e thread-safe.