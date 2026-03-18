È un diagramma strutturale usato nella programmazione a oggetti per descrivere formalmente le relazioni tra classi e interfacce.
# Rettangoli
Definisce classi o interfacce. [Esempio](Class%20diagram%20-%20Esempio%20classe.png).
- Per i campi la sintassi è [[Class diagram - Sintassi attributi.jpeg|questa]]. Le proprietà possono essere $readonly$, $unique$, $non-unique$, $ordered$, $unordered$.
- I parametri possono essere: $out$ indica che dopo l'operazione il parametro ha un nuovo valore; $in$ indica che il parametro è usato nell'operazione; $inout$ comprende entrambi.
- Le interfacce sono indicate con $<<Interface>>$ oppure $<I>$. Le enumerazioni con $<<Enumeration>>$.
- Il nome della classe o interfaccia è scritto in maiuscolo. Il nome dei membri è scritto in minuscolo.
- L'astrazione è indicata in corsivo.
- Lo statico è indicato con il sottolineato.
- La visibilità dei membri è indicata da $+$, $-$ o $\#$.
# Associazioni
Rappresentano relazioni tra classi, inteso come concetto di usabilità.
- Sono rappresentate da frecce aperte unidirezionali o bidirezionali: le prime sono rappresentate da una freccia con o senza una "x" nella direzione di non navigabilità; le seconde sono rappresentate o con due frecce o senza frecce. Si può indicare la molteplicità dall'associazione in entrambi i sensi. Nel lato di navigabilità della freccia (Lato classe che viene usata) si può indicare il ruolo; questo sarà il nome del riferimento della classe che usa. [Esempio](Class%20diagram%20-%20Ruolo.png).
- Due classi possono essere associate a una terza con un $or$ esclusivo: la terza classe non può usare contemporaneamente le altre due. Questo si indica con ${xor}$ e due linee tratteggiate ai collegamenti. [Esempio](Class%20diagram%20-%20Usabilità%20eslusiva.png).
- Una classe può essere associata con se stessa.
- Ci possono essere associazioni $n-arie$. In questo caso la navigabilità è in tutte le direzioni e non può essere specificata. Si procede per insiemi: fissate $n-1$ classi si definisce la $n-esima$.
- Se in un'associazione una delle due classi non ha cardinalità unaria allora gli attributi che definiscono l'associazione non possono essere inseriti come campi all'interno di una classe (Quella con cardinalità unaria). Per questo motivo si definisce una classe di associazione e la si collega con una linea tratteggiata al collegamento dell'associazione. Questo è possibile anche con relazione $n-arie$. [Esempio](Class%20diagram%20-%20Classi%20di%20associazione.png).
# Aggregazione
Indica che una classe è parte di un'altra, cioè chi è il componente principale e quello secondario.
Si indica con un segmento con un rombo che parte dalla classe che ha un riferimento all'altra.
- Gode della proprietà transitiva e asimmetrica: se $A$ fa parte di $B$ allora $B$ non può far parte di $A$.
- Shared
	Si indica col rombo vuoto. Indica che la classe aggregata (Quella riferita dall'altra) può esistere anche senza l'aggregazione. La molteplicità della classe che contiene il riferimento può essere maggiore di 1.
- Composition
	Si indica col rombo pieno. Indica che la classe aggregata (quella riferita dall'altra) non può esistere anche senza l'aggregazione: se si elimina, l'altra non può esistere. La molteplicità della classe che contiene il riferimento deve essere al più 1.
# Generalizzazione
Indica l'inheritance. Si indica con una freccia chiusa, continua e vuota dalla classe che estende verso quella estesa.
- Gode della proprietà transitiva.
- È consentita la generalizzazione di più superclassi (In Java non si può realizzare).