Quando si costruisce un sistema si deve garantire quello che è il Design For Reliability (DFR).
In questo caso il termine reliability è inteso nel senso della dependability: il DFR ha questo nome perchè la reliability è l'attributo più usato, ma in generale si intende costruire un sistema affidabile nel senso della dependanbility, quindi di tutti gli [attributi](attributi.md) RAMS.
Lo scopo del DFR è quello di ridurre i costi e aumentare la qualità del prodotto.
# Vantaggi
- Gli errori vengono trovati in fase di progettazione e questo comporta che il prodotto possa essere migliorato prima che si inizi la sua produzione.
- Si possono confrontare più soluzioni e valutare la migliore dal punto di vista costo/beneficio.
- Ogni soluzione può o meno soddisfare dei target di affidabilità. Si deve scegliere quella che permette di allungare la vita del prodotto il più possibile.
- Si deve dare garanzia di safety, cioè dimostrare la sicurezza del prodotto.
# Pipeline
La DFR segue la seguente pipeline. Ogni step deve essere revisionato e documentato.
- Capire quali sono i parametri e gli obiettivi di affidabilità.
	Questo è fondamentale perché non si vuole ottenere il prodotto perfetto, ma realizzare un prodotto che rispetti questi obiettivi.
	Sono derivati dal "mission profile": si definisce la funzione del prodotto e poi quale deve essere il suo ciclo di vita. Ad esempio sia la garanzia che la garanzia aggiuntiva sono definite dall'azienda più lontane rispetto agli obiettivi di affidabilità che sono stati fissati.
- Individuare i componenti più critici, cioè che possono essere un problema per l'affidabilità, e decidere il livello di derating da implementare.
	- I vari livelli di derating si devono sempre testare: può capitare che diminuire il livello di derating di un componente possa far alzare quello di un altro.
	- Quando si fa derating stiamo usando i componenti a un livello di stress inferiore al valore nominale dichiarato dal produttore, in modo da allungarne la vita. Solitamente per capire il livello di derating si definisce un rapporto $S$ tra i parametri nominali e quelli effettivamente utilizzati: avere un valore di $S$ maggiore tende a diminuire il fattore di stress e quindi il tasso di guasto, facendo aumentare l'affidabilità.
	- Tramite delle [misure](Software%20Engineering/dependability/misure.md) possiamo stimare il tasso di guasto e quindi la reliability del sistema. Se il tasso di guasto ottenuto è troppo alto allora possiamo giocare durante questa pipeline con il derating, limitando quindi il fattore di stress.
- Si devono scegliere le tecniche di progettazione.
	Si deve mantenere il design semplice anche quando il progetto è complesso. Si deve gestire la ridondanza, visto che questa aumenta l'affidabilità ma aumenta anche i costi e quindi va usata solo quando ci sono aspetti critici. Dobbiamo implementare tecniche per proteggersi dai rischi.
- Si devono minimizzare gli effetti dell'interazione del prodotto con l'ambiente esterno. Questo è vero sia che sia un oggetto usato da un umano o che sia un software in esecuzione in una macchina.
- Prevedere la manutenzione per limitare i guasti.
- Capire se i parametri di affidabilità, ottenuti tramite [misure](Software%20Engineering/dependability/misure.md), che avevo definito all'inizio sono stati raggiunti tramite la raccolta di dati sul campo (guasti e manutenzioni): in caso positivo si passa alla fase di prototipazione e poi alla progettazione; in caso negativo si inizia nuovamente il ciclo. Queste conoscenze rappresentano i know-how dell'azienda per prodotti futuri.