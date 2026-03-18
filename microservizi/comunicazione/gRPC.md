gRPC (google Remote Procedure Call) è un framework open source sviluppato da Google. Si tratta di un protocollo di [comunicazione](microservizi/comunicazione/comunicazione.md) binario (molto efficiente) che forza a usare il paradigma API-first.
# ProtoBuf
Il Protocol Buffer è il IDL (Interface Definition Language) di gRPC e definisce le interfacce con cui client e server comunicheranno. Descrive il nome del metodo che si vuole chiamarte, i tipi attesi come parametri e quelli di ritorno.
##### Utilizzo
Si parte definendo un file $.proto$.
A partire dal file proto, si utilizza il compilatore $protoc$ che gRPC mette a disposizione. In questo modo viene generato tutto il codice necessario e le cose che poi dobbiamo fare sono:
- Server
	Il compilatore genera un'interfaccia. Il server deve implementare questa interefaccia eseguendo l'override dei metodi.
- Client
	Il client a questo punto, dopo aver configurato tutto (e questo dipende dal framewrok che si usa), può chiamare i metodi del server come se fossero metodi di un oggetto a cui ha un riferimento.
# Vantaggi
- Se abbiamo molte operazioni complesse allora definire delle API [REST](REST.md) può essere difficile se vogliamo mantenere la sintassi dei verbi corretta.
- Permette, usando HTTP/2, di inviare più risposte a seguito di un solo messaggio (o viceversa). Questo da la possibilità di implementare uno [streaming](gRPC.md#Streaming).
- Essendo un protocollo binario che non si basa su JSON, è molto efficiente. Il vantaggio sta nella serializzazione (portare un oggetto in un file) e deserializzazione del protobuff rispetto al JSON.
# Streaming
Uno degli usi migliori che possiamo fare di gRPC è lo streaming. Per implementarlo basta usare la keyword $\texttt{stream}$ quando si definisce il nome dell'input e dell'output nel file $.proto$.
Questo streamiing può essere visto da più punti di vista:
- Server
	Dopo aver ricevuto un messaggio, il server invia più dati.
- Client
	Dopo aver inviato un messaggio, client riceve più risposte.
- Bidirezionale
	Sia il client che il server inviano e ricevono più dati.



# StreamObserver
Mutiny is a reactive programming library extensively used

in Quarkus. In this case, Mutiny acts as a replacement for the StreamObserver API. The

StreamObserver API is what the protoc compiler uses by default. With Quarkus, it is

commonly replaced with Mutiny, as Mutiny provides a more straightforward API, and

using Mutiny types makes the code more consistent and compatible with what is used by

other Quarkus extensions. Don’t worry if you don’t understand all the details yet. Chap-

ter 8 focuses on reactive programming and will explain Mutiny concepts in more detail.