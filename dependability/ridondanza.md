Per ridondanza in generale si intende la possibilità di utilizzare più mezzi per eseguire una certa funzione.
# Design diversity
La design diversity è l'utilizzo della ridondanza con componenti che sono progettualmente diversi: non solo il componente è ridondato, ma si utilizza un progetto di costruzione diverso. Questo vuol dire che un pezzoo hardware si compra da un altro produttore o che un pezzo software si fa fare a due aziende diverse. 
# Modalità
Ci sono 3 modalità per gestire la ridondanza.
##### Attiva
Abbiamo $n$ componenti tutti attivi contemporaneamente.
Ha il vantaggio di aumentare di molto la [reliability](attributi.md#Reliability) di un sistema visto che stiamo mettendo più componenti uguali in [parallelo](reliability%20block%20diagram.md#Parallelo). Lo svantaggio è i componenti sono sempre tutti attivi e quindi tutti soggetti allo stesso livello di degradamento.
##### Cold stand-by
Il componenti in ridondanza (i.e., quello che aspetta) non è collegato, cioè prima di entrare in funziona ha bisogno di un certo intervallo di latenza. Il suo scopo è ottimizzare la vita dell'unità ridondata.
- È più sicuro perché quando entra in funziona tale componente è nuovo e quindi con $R(t=0)=1$ (i.e., reliability massima).
- Si può usare solo quando una latenza di attivazione è accettabile.
##### Warm stand-by
Il componente in ridondanza (i.e., quello che aspetta) non è attivo ma collegato, cioè può entrare in fuzione subito se il primo si guasta.
- Si azzera la latenza di intervento dell'unità ridondata.
- Abbiamo un livello di degradamento diverso per i due componenti. Per l'unità ridondata il degrado non è nullo.
# Misure
Quando si parla di ridondanza si parla di [inaffidabilità](attributi.md#Reliability) invece che di affidabilità, come si fa per il calcolore della reliability nei sistemi in [parallelo](reliability%20block%20diagram.md#Parallelo) nel RBD.
##### Quantità
Solitamente non si mettono più di 4 unità in parallelo per realizzare un componenti: si aumentano i costi, si ha bisogno di più spazio e la reliability raggiunge un limite. Quello che si fa è quindi valutare la reliability al tempo in cui dobbiamo garantire l'affidabilità del sistema e gestire la ridondanza in base alla safety-critical del componente.