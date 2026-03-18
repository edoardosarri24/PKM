Lo standard 802.11 nel tempo si è evoluto facendo diventare il Wi-Fi una tecnoliga che permetteva un bit rate sempre più alto.
Nel mondo IoT, tramite gli standard l'802.11ah e 802.11ba, vogliamo usare questa tecnologia per:
- Connettere molti dispositivi
- Avere un basso consumo energetico (usare poca batteria)
- Connettere i dispositivi a grande distanza.
# 802.11ah
### Livelli
##### Livello fisico
Utilizza frequenze dette Sub-Giga Herts (che stanno sotto il Giga) e non è retro compatibile con lo standard legacy.
- Vantaggi
	- Le frequenze utilizzate non necessitano di licenza.
	- Permettono di coprire una grande distanza, maggiore di quella del 2.4GHz. Si parla di circa 1km.
	- Sono frequenze meno congestionate e quindi offrono una maggiore affidabilità.
	- Permettono una miglior penetrazione dei materiali.
	- Permettono di consumare poca energia e quindi portano a un risparmio di batteria.
- Svantaggi
	- Le frequenze sotto il Giga sono le stesse delle tv, e quindi si deve stare attenti a non andare a interferire con quelle.
##### MAC
Per ridurre il livello di consumo si deve cercar di trasmettere il minor numero di pacchetti possibili; per far questo si deve massimizzare il payload di un pacchetto e ridurre l'header. Il livello MAC di questo protocollo cerca di fare proprio questo:
- Si riduce la dimensione dell'header rimuovendo campi non fondamentali e si utilizza due soli indirizzi (sorgente e destinazione) invece che tre (l'indirizzo dell'AP).
- Si rimuove l'acknowledgment con il meccanismo di Bi Directional Transmit (BDT). Se nella nel WI-Fi classico si invia un pacchetto ACK dopo aver ricevuto quello dei dati, in questo caso si elimina l'ACK e si risponde subito con altri dati; un ACK sarà inviato al termine della comunicazione. In questo modo si riduce il numero di frame scambiati e l'overhead dovuto all'invio dell'ACK.
### Collisioni
Se nel Wi-Fi legacy si può collegare al più circa 2000 station (device), con questo standard si vuole permettere l'accesso ad almeno 8000 dispositivi.
Questo porta al problema delle collisioni: se molti dispositivi accedono contemporaneamente allora molte comunicazioni saranno perse.
Si usa per questo motivo la tecnica Restricted Access Window (RAW): si dividono i dispositivi in gruppi e il tempo in slot; ogni slot è associato a un gruppo, i cui dispositivi possono comunicare solo in quello slot di tempo. In questo modo:
- Ci saranno delle collisioni anche all'interno dello slot, ma il numero sarà molto minore.
- I dispositivi possono andare in modalità sleep quando non sono all'interno del loro slot temporale e risparmiare quindi batteria.
### Power saving (PS)
Se un dispositivo in modalità PS non si connette per un determinato lasso di tempo allora viene scollegato e dimenticato dall'AP: nel caso del Wi-Fi legacy questo tempo è 18 ore; nel caso del Wi-Fi ah questo tempo è 5 anni.
##### legacy
Il Wi-Fi legacy definisce due modalità operative: nella active mode la station è sempre sveglia e può inviare e ricevere pacchetti; nella modalità PS la station alterna momenti dove è sveglia e momenti in cui è in fase sleep, cioè non può ne ricevere né trasmettere.
Nella modalità PS, quando il dispositivo è in fase sleep, l'AP fa da buffer per non perdere i pacchetti destinati al dispositivo. Periodicamente l'AP invia un beacon frame, che contiene il Traffic Indication Map (TIM), il quale indica la presenza di pacchetti destinati a ogni station.
Quando la station passa dalla fase sleep a quella sveglia aspetta il bacon frame e poi decide se restare sveglia per ricevere i dati oppure se tornare in fase sleep.
##### ah
In questo caso la station non si sveglia periodicamente aspettando il beacon frame, ma definisce con l'AP un Target Wake Time (TWT); sulla base del TWT allora la station si sveglia e riceve il beacon.
In questo modo la station non deve svegliarsi periodicamente e aspettare il beacon, ma questo arriverà quando si sveglia perché è un tempo negoziato. In questo modo la station può stare sveglia meno tempo e risparmiare energia per quelle station che raramente ricevono e trasmettono dati.
- Il problema di quanto descritto sopra è che, se una station occupa un bit (0 se non ci sono messaggi 1 altrimenti), il beacon frame è troppo lungo perché ci sono molte station. Si ricorre così alla frammentazione del beacon: l'AP e la station negoziano la partizione del beacon frame destinata a lui, cioè l'unica che deve ascoltare; in questo modo la station può dormire per la maggior parte del tempo.
# 802.11ba
Utilizza frequenze dette Sub-Giga Herts (che stanno sotto il Giga) e non è retro compatibile con lo standard legacy.
Il motivo dell'introduzione di questo protocollo è dato dalla sovrapposizione di 802.11ah con 802.11ax; quest'ultimo utilizza le frequenze legacy (2.4GHz e 5GHz) e inoltre introduce il concetto di TWT per peremettere di risparmiare energia; questo lo rende adatto non solo per dispositivi complessi ma anche per il mondo IoT.
### TWT
Il meccanismo del TWT di 802.11ah server per far restare in modalità sleep un dispositivo per molto tempo; proprio per questo motivo ha problemi di sincronizzazione con il clock con l'AP.
Questo porta a far si che la station si svegli prima (0,36s) rispetto a quanto dovrebbe, che debba chiedere il clock all'AP o che contenda il canale radio con altri dispsositivi per indicare all'AP che è sveglio.
### Wake-Up Radio (WUR)
Questo standard introduce un nuovo canale, il WUR.
Il WUR consuma molta meno energia del canale principale, ed è usato dall'AP in trasmissione per indicare ad una station di riattivarsi.
In questo modo una station ha due canali: uno primario e bidirezionale (PCR), che dovrebbe rimanere attivo il minor tempo possibile; uno solo in ricezione (WUR) usato dall'AP per svegliare la station quando ci sono pacchetti.
- In questo modo la station viene risveglia nel momento esatto in cui ci sono pacchetti, e non deve stare sveglia più del necessario.
- Si elimina il problema del duty cycling.