Per misurare il tempo si utilizza un dispositivo che incrementa il proprio contatore periodicamente, dove questo periodo è detto microtik.
# Tempo
Il tempo è una misura continua, densa (i.e., tra due istanti $t_{1}$ e $t_{2}$ è sempre presente un istante $t_{3}$) e totalmente ordinabile (i.e., due elementi sono sempre ordinabili).
Relativamente al tempo possiamo definire vari aspetti:
- Durata
	È un intervallo di tempo, con una fine e un inizio.
- Evento
	È un qualcosa che avviene istantaneamente, cioè senza durata. L'insieme degli eventi è solo parzialmente ordinabile: se due eventi occorrono insieme questi non possono essere ordianti.
# Errori
Un clock può commettere due tipi di errore:
- State
	È un errore relativo al contatore.
- Rate
	È un errore sulla frequenza del microtik.
# Sincronizzazione
La sincronizzazione si basa su due parametri: la precisione e l'accuratezza. Se i clock hanno un'accuratezza di $A$ allora essi hanno una precisione di $2A$.
##### Precisione
Indica quanto sono vicini i clock che appartengono a un gruppo (e.g., insieme di calcolatori) e si misura con il massimo offset tra ogni coppia di clock.
##### Accuratezza
Indica quanto un clock è vicino al clock di riferimento.
# Sistemi distribuiti
In un sistema distributo i clock dei nodi non saranno perfettamente sincronizzati
##### Clock globale
Permette ai nodi di avere una visione comune dello scorrere del tempo.
Gli standard sono due: l'UTC (Universal Time Coordinated) è uno standard astronomico su cui si basano i wall clock; il TAI (Internationa Atomic Time) è uno standard fisico basato sulle vibrazioni del cesio.
##### Misurazione
La misurazione può comunque essere fatta. Ci sono delle tecniche a seconda del contesto:
- Locale
	Due eventi che avvengono localmente al singolo nodo possono essere ordinati senza problemi considerando il clock del nodo stesso.
- Roun Trip Time (RTT)
	Un operazione di andata-ritorno (e.g., request responce in API [REST](REST.md)), può essere misurata osservando solo il clock di chi osserva enteambe (e.g., chi fa la request).
##### Durate Distribuite
Per misurare la distanza tra due eventi senza utilizzare un clock globale si usnao le catena temporali: tra due nodi non ci deve essere solo un messaggio ma due; in questo modo ogni nodo ha 4 informazioni che può usare per fare calcoli.