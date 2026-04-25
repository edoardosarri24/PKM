Il LwM2M (Lightweight Machine to Machine) è uno stardard proposto dall'OMA (Open Mobile Alliance), la cui versione del 2018 è accetta a livello mondiale. Quando si parla di M2M si parla di processi automatici che devono funzionare senza la supervisione dell'uomo.
È un protocollo leggero, stateless e basato sul modello client server. È progettato per dispositivi constrained sia mobili che fissi.
Il suo obiettivo è quello di permettere a un dispositivo di gestirne un altro.
##### Stack
È interessante notare come LwM2M sia un livello posto sopra [CoAP](CoAP.md) all'interno dello [stack](LwM2M%20stack.png). Questo infatti permette di aggiungere funzionalità, principalmente di gestione, al protocollo CoAP.
Un altro aspetto importante per una funzionalità è quello che oltre a UDP, LwM2M può usare anche il livello SMS (messaggi).
# OPERAZIONI
##### Configurazione
È la [configurazione](configuration%20LwM2M.png) di un dispositivo client da un server. In questo caso è il client che richiede al server di essere gestito e che interrompe questa richiesta.
- Questo avviene tramite una prima POST: il client chiede al server di riservare un'area di memoria per le sue variabili; dopo questa configurazione il client può inviare aggiornamenti sul suo stato al server per essere gestito.
- La disconnessione avviene tramite una DELETE.
- Nel mezzo tra configurazione e disconnessione c'è una serie di POST che il server invia al client per la gestione: il client dovrà creare aree di memoria ed essere in grado di gestirle.
##### Diagnostica e data reporting
Le operazioni di diagnostica principali riguardano: impostazioni, stato della batteria, stato della memoria e la versione del sw o dell'hw.
Si potrebbe mappare come una serie di domande e risposte, ma siccome siano in una situazione in cui i server fa una richiesta e il client invia una serie di risposte la cosa migliore è mapparlo con l'[osservazione](diagnostica%20LwM2M.png) del client da parte del server: il server fa una GET con un codice di osservazione e così si mette in ascolto del client.
##### Controllo
Il [controllo](control%20LwM2M.png) deve essere un'operazione real time e deve quindi essere sempre disponibile. Se il client è in sleep mode (ad esempio a causa del duty cycle) si deve trovare un modo per svegliarlo; questo modo è l'invio di un sms (livello inferiore a LwM2M e presente nello stack).
Una volta che il client è sveglio il server può mandare una serie di POST, che saranno salvati in aree di memoria stabilite in un momento precedente.