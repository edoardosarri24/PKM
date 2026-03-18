Il problema dell'identità si può ricondurre alla dimostrazione che qualcuno sia effettivamente chi dice di essere; cioè il problema è legare qualcosa all'identità.
# FATTORI DI AUTENTICAZIONE
Per associare qualcosa a qualcuno si usano tre metodi.
- Conoscenza
	Qualcosa che solo chi si deve identificare sa. Ad esempio una password.
- Possedimento
	Qualcosa che non si deve lasciare mai. Ad esempio una smart card un telefono.
- Essere
	Qualcosa che ci caratterizza per chi siamo. Ad esempio l'impronta digitale o la retina.
### Problemi
Nessuno dei precedenti metodi è più sicuro dell'altro:
- Le password possono essere trovate.
- Un oggetto può essere perso o rubato.
- I dispositivi resposabili dell'autenticazione possono portare a falsi negati o positivi.
### Multifactor autentication
Si parla di utilizzo di più fattori quando un server valuta più metodi diversi contemporaneamente.
I metodi devono essere diversi: una smart card abilitata da una password corrisponde a un solo metodo, perché la password non identifica la perosna, ma la smart card.
### Macchine
Per le macchine i metodi elencati sopra diventano più sfumati: quale è per esempio la differenza tra conoscere e possedere qualcosa per una macchina? In ogni caso si deve trovare un modo per identificare in modo univo una macchina e poi per identificarla effettivamente.
# METODI
La [crittografia simmetrica](Crittografia.md#CRITTOGRAFIA%20SIMMETRICA) non va bene perché non identifica entrambi gli endpoint; dobbiamo rifarci a metodi che utilizzano la crittografia asimmetrica.
### Fingerprint
É un metodo ottimo per sapere se qualcuno si sta facendo passare per qualcun’altro; nei sistemi a chiave pubblica privata fornisce un claim che la chiave pubblica é la chiave nostra.
##### Metodo
Il metodo é prendere la chiave pubblica e ne estraggo una parte rappresentativa e la metto in tutte le mie comunicazioni; vuol dire che ovunque io compaia devo infierire questo pezzo di chiave, cioè il fingerprint deve essere diffusa il più possibile.
L’idea è che se abbiamo generato una coppia di chiave pubblica privata e per molto tempo abbiamo diffuso il fingerprint allora la probabilità che qualcuno dica no Alice sono io é molto bassa, perché ormai tutti ci conoscono con il fingerprint.
Il fingerprint lo può usare solo chi ha la chiave privata associata alla chiave pubblica dalla quale si è ricavato il fingerprint; se un attaccante firma il messaggio con la sua chiave privata e poi trasmette il mio fingerprint, allora chi lo riceve verifica che il fingerprint é associato alla mia chiave pubblica ma non riesce a decifrare il messaggio.
##### Generazione
Per la generazione del fingerprint si deve avere una funzione che generi un fingerprint piccolo da una chiede grande (la chiave pubblica é lunga) e che due chiavi pubbliche diverse diano due fingerprint diversi; si può usare la funzione hash, perché questa è la sua definizione. In realtà non importa usare la funzione hash perché si possono prendere i primi o gli ultimi x byte della chiave pubblica perché queste sono generate casualmente.
##### Problema
Va bene per un'interazione uomo-uomo o macchina-uomo perché il ricevente deve confrontare la fingerprint con la chiave pubblica del mittente e deve verificare che coincida con quelle lasciate nel tempo all'interno dei messaggi. Una macchina questo non lo può fare.
### Web Of Trust (WOT)
L'idea è fidarsi della fiducia degli altri.
Più un’entità è conosciuta allora più é degna di fiducia, perché é più improbabile che abbia fregato molte persone. Questo di trauce nel concetto per cui più firme uno ha più e più è una persona verificata; mi tendo a fidare di qualcuno che ha tante firme o che ha la firma di qualcuno che io conosco.
##### Funzionamento
La firma viene fatta facendo un hash della chiave pubblica e cifrando l’hash con la chiave privata di chi lo firma; é in pratica una firma digitale.
##### Problema
Il problema é che con una percentuale abbastanza bassa di utenti malevoli il sistema diventa totalmente inaffidabile. Non é scalabile e non va bene per le transizioni importanti.
È inoltre un meccanismo orizzontale, cioè dove tutti hanno la stessa importanza e ruolo: non è quindi centralizzato.
##### OpenPGP
È un sistema a sistema a chiavi pubblico privata stardardizzato dall'RFC 4880. Nel sistema la mia chiave pubblica viene firmata da altri con la loro chiave privata, ma solo se ci conosciamo personalmente.
### Certificati
È il metodo che effettivamente si usa; un esempio è HTTPS, dove LTS basa la sua crittografia su certificati. Non è uno standard, cioè non dice come devono essere usati i certificati, ma solamente quale deve essere il loro formato.
L'utente generale la coppia di chiavi pubblica privata; la Certification Authority (CA) associa l'identità dell'utente alla chiave pubblica.
##### Formato
Si utilizza il formato [x.509](certificate%20x.509%20format.png)
- Version
	Versione del formato.
- Serial Number
	Serve per identificare in modo univo un certificato.
- Signature Algorithm ID
	Indica l'algoritmo utilizzato per la cifratura della CA digital signature. È importante perché in questo modo si può cambiare l'algortimo, che probabilmente dopo x anni sarà obsoleto.
- Issuer (CA) name
	Nome della CA.
- Validity period
	Siccome è un meccanismo che si basa su chiavi pubblico private e queste hanno una scadenza, allora anche il certificato avrà una scadenza.
- Subject name and public key
	Le informazioni legate al proprietario del certificato e della chiave e che deve essere identificato.
- Issuer and Subject ID
	Servono per identificare in modo univoco il soggetto e la CA.
- Extension
	Definiscono per cosa il certificato è valido. Si dovrebbe usare il principio del single responsability, cioè associare al certificato le operazioni di cui si ha effettivamente bisgono (non si deve fare un certificato valido per tutto).
- CA Digital signature
	È la firma digitale del certiciato. Se prendiamo tutto il certificato tranne questo campo allora si può vedere come un messaggio; se ne calcola il codice hash e la CA lo firma con la sua chiave privata. Il ricevente calcola l'hash del messaggio (certificato) e o confronta la CA sigital signature per vedere se corrispondono.
##### Craccare un certificato
- Si può fare un attacco brutale alla chiave privata della CA. In questo modo possiamo creare un certificato associando qualunque chiave pubblica a chiunque.
- Si cerca una collisione: se potessi creare un altro certificato con lo stesso hash allora avrei un certificato da poter usare al posto di quello originale. Fare questo è possibile però ha una complessità molto elevata. Consideriamo però che all'inizio i certificati avevano una durata di 5 anni; siamo passati poi a tre anni; adesso l'IETF sta cercando di ottenere certificati della durata di 1 anno e basta. Questo vuol dire che ottenere una collisione sembra essere più facile.
##### Revoca
Quando un certificato viene compromesso (il proprietario non può più usare la sua chiave oppure si pensa che quest sia in ano a un attaccante) si deve revocare. Questo è complesso perché i certificati sono nati per essere auto verificati.
Un certificato revocato è all'interno della Certificate Revocation List (CRL) mantenuta dalla CA e disponibile tramite un url pubblico; un client che vuole sapere se un certificato emesso da una certa CA è valido o revocato deve controllare se il suo serial number è all'interno della lista.
Il problema di questo meccanismo è che la CRL diventa un punto di fallimento per quei client che la vogliono leggere e verificare che un certificato non sia revocato: se non è raggiungibile allora il client non può proseguire e si deve fermare.
Inoltre interrogare sempre la CRL è un meccanismo che rallenta la comunicazione; un client dovrebbe utilizzare la CRL solo per quelle comunicazioni su cui si ha bisogno di una certezza assoluta (es: banca). Questo motivo ci porta anche a osservare che non si devono condividere informazioni importanti tramite browser: essendo generl pourpose, non legge le CRL dei CA e quindi se un certifcato è revocato non lo si può sapere; si devono utilizzare le applicazioni ad hoc per quello scopo.
##### CA
Una CA non deve essere mai compromessa.
- Siccome la sua chiave pubblica è esposta a tutti, nel caso questa venga craccata la CA deve dirlo molto velocemente.
- La CA non deve mai rilasciare un certificato falso neanche per un test: così facendo perde di credibilità e poi nessuno si fida più.
- L'insieme della CA di cui ci possiamo fidare sono nella root certificate dell'OS; questo è il motivo per cui dobbiamo sempre aggiornare il sistema operativo. I browser possono avere una black list di CA, le quali non possono essere approvate su di esso; questo viene fatto perché aggiornare un browser è più pervasivo (gli utenti lo fanno prima) che aggiornare un OS.