Quando si costruisce un sistema si deve garantire il Design For Reliability (DFR). In questo caso il termine reliability è inteso nel senso della dependability: il DFR ha questo nome perché la reliability è l'attributo più usato, ma in generale si intende costruire un sistema affidabile nel senso della dependanbility, quindi considerando tutti gli [attributi](attributi.md) RAMS.
# Vantaggi
Lo scopo del DFR è quello di ridurre i costi e aumentare la [qualità](qualità.md) del prodotto.
- Gli errori vengono trovati in fase di progettazione e questo comporta che il sistema possa essere migliorato prima che si inizi la sua produzione, riducendo i costi.
- Si possono confrontare più soluzioni, dove ognuna può soddisfare in modo diverso i target di affidabilità. Si deve poi valutare la migliore dal punto di vista costo/beneficio, dove il benefico solitamente è la lunghezza di vita del sistema.
# Pipeline
La DFR segue la seguente pipeline. Ogni step deve essere revisionato e documentato.
- Definire parametri e valori target di dependability.
	Non si vuole ottenere il sistema perfetto, ma realizzare un prodotto che rispetti questi obiettivi.
	Ad esempio sia la garanzia che la garanzia aggiuntiva sono definite dall'azienda più lontane rispetto agli obiettivi di affidabilità che sono stati fissati.
- Individuare i componenti più critici, cioè che possono essere un problema per l'affidabilità, e decidere il livello di derating da implementare.
	Il derating è la pratica con cui si utilizza un componente sotto il suo valore di utilizzo nominale dichiarato dal produttore, in modo da allungarne la vita. È definito da un rapporto $S$ tra i valore di utilizzo nominale e quello effettivamente usato: avere un valore di $S$ maggiore tende a diminuire il livello di stress e quindi il tasso di guasto, facendo aumentare l'affidabilità.
	- I vari livelli di derating devono sempre essere testati: può capitare che diminuire il livello di derating di un componente possa far alzare quello di un altro perché se uno lavora meno allora un altro deve lavorare di più.
	- Tramite delle [misure](dependability/misure.md) possiamo stimare il tasso di guasto e quindi la reliability del sistema. Se il tasso di guasto ottenuto è troppo alto allora possiamo lavorare sul derating, limitando quindi il fattore di stress del componente.
- Si devono scegliere le tecniche di progettazione.
	Si deve mantenere il design semplice anche quando il progetto è complesso. Si deve gestire la [ridondanza](reliability%20block%20diagram.md), visto che questa aumenta l'affidabilità ma aumenta anche i costi e quindi andrebbe usata solo per i componenti critici.
- Si devono minimizzare gli effetti dell'interazione del prodotto con l'ambiente esterno. Questo è vero sia che sia un sistema che [interagisce con un uomo](human%20reliability%20analysis.md) sia per un software in un server.
- Prevedere la [manutenzione](manutenibilità.md) per limitare i guasti.
- Raccogliere dati sul campo tramite [misure](dependability/misure.md), per capire se gli attributi di dependability hanno raggiunto i valori che avevamo definito: in caso positivo si passa alla fase di prototipazione e poi allo sviluppo; in caso negativo si inizia nuovamente il ciclo.
