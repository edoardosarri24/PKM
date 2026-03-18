L'autenticazione è correlata con l'[identificazione](Identificazione.md): dell'identificazione e basta non ce ne facciamo nulla; dopo che abbiamo chiarito chi l'utente sia lo si deve autenticare.
Autenticazione e autorizzazione invece sono concetti scollegati.
##### Obiettivo
Tramite autenticazione vogliamo raggiungere due obiettivi:
- Evitare che qualcuno non autorizzato entri nel sistema.
- Autorizzare qualcuno in base all'autenticazione: dato il singolo utente vogliamo poter dare dei privilige spcifici in base all'utente.
# AUTENTICAZIONE DI GRUPPO
L'autenticazione di gruppo permette a un insieme di persone che hanno un segreto condiviso di accedere al sistema.
Va contro l’idea stessa di autenticare: si vorrebbe autenticare un soggetto, non un insieme di soggetti.
##### Problemi
- Se qualcuno esce dal gruppo allora l'unico modo per impedirgli di entrare nel sistema è cambiare il segreto condiviso.
- Va contro il Role-Based Access Control (RBAC): è un sistema di controllo basato sui ruoli, dove i permessi vengono assegnanti ai ruoli e non agli utenti. Questo permette di cambiare il ruolo di un utente e cambiargli i permessi associati in modo semplice. Nell'autenticazione di gruppo non si può fare distinzione tra le varie autorizzazioni, perché ogni partecipante avrà gli stessi privilegi.
##### Vantaggi
Si potrebbe volere un'autenticazione di gruppo perché si vuole nascondere l'identità dei partecipanti al sistema, cosa impossibile con un'autenticazione di utente.
# SISTEMA DI AUTENTICAZIONE
### Requisiti
In generale dipendono dal sistema, però possiamo elencare:
- Autenticazione per utente e non di gruppo.
- Autenticazione e autorizzazioni separate. Prima si dice sei e poi cosa puoi fare.
- Aggiungere e rimuovere un utente deve essere semplice. Nessun utente deve avere privilegi per cui ha potere sulla sua rimozione dal sistema.
- La chiave di autenticazione (es: password, certificato) e il segreto di autorizzazione (chi non ha un segreto non può fare determinate cose) devono essere separati.
- Perdere un dispositivo o averlo hackerato deve comportare solo la perdita di quel particolare dispositivo. Una situazione che è successa è che si sono buttati nel cestino device che avevano stampata nella memoria la chiave di un sistema, la quale poi è stata rubata.
- L'autenticazione deve essere fatta in entrambi i sensi: il client deve autenticare il server e viceversa; succede invece che sia solo il server che autentica il client. Un problema di questa situazione è un server compromesso che, se non autorizzato, ruba le chiave al client.
### Design
I requisiti ci conducono ad alcune scelte di design:
- L'autenticazione deve essere centralizzata e/o federata. Centralizzata perché è molto più semplice da gestire. Federata vuol dire che io posso autenticarmi non in un solo server ma in un ins di server che collaborano secondo protocolli e stadard comuni (eduroam).
- L'autorizzazione viene data sulla base dell'autenticazione: i permessi sono stabiliti in base a chi siamo.
- La revoca di un'autorizzazione può essere eseguita o levando l'autorizzazione o non permettendo l'autenticazione.
- La cifratura deve usare chiavi temporanee.
- Usare l'autenticazione in due sensi, cioè clinet-server e viceversa.
# 8002.1 X
L'802.1 X è il Port-Based Network Access Control e permette di implementare un sistema che rispetti tutti i requisiti visti sopra. La X identifica il numero di revisione dello standard. È un protocollo che specifica l'architettura, gli elementi funzionali e i protocolli di un sistema.
È conosciuto soprattutto per le reti Wi-Fi, ma è usato anche per le reti wired. Un esempio è il suo utilizzo nell'ethernet: si vuole impedire che un attaccante possa eseguire attacchi link-local attaccando solamente il cavo di rete.
### Scopo
Il suo scopo iniziale era quello di autenticare un dispositivo o un utente tramite una porta non sicura; per porta si intende un punto di accesso alla rete: in ethernet è il connettore al muro; nel Wi-Fi è la chiave di cifratura riconosciuta dall'Access Point.
Impedisce che un utente possa comunicare con la rete se non ha superato le fasi di autenticazione e autorizzazione; finché non ha superato queste fasi può comunicare solo con il controllore di questi due aspetti.
### Attori
È sempre bene separare i due concetti di authenticator e authentication server.
- Supplicant
	È l'entità a un lato del link della LAN che vuole essere autenticato dall'authenticator che sta dall'altra parte del link.
- Authenticator
	È l'entità che semplifica l'autenticazione del supplicant. È quindi il punto di accesso alla rete (es: access Point)
- Authentication Server
	È l'entità che fornisce il servizio di autenticazione all'authenticator. Il servizio permette, a partire dalle chiavi di autenticazioni fornite dal supplicant, di stabilire se questo è chi dice di essere e successivamente a fornire qualche autorizzazione per eseguire operazioni nella rete.
	Osserviamo che questo non eroga i servizi della rete, ma gestisce solo l'autenticazione.
### Funzionamento
- Il supplicant invia una richiesta di accesso all’autenticator (ci siamo attaccati al cavo Ethernet o si è trovato l’AP) tramite il pacchetto EAP.
- Il canale tra supplicant e authenticator è da considerarsi non sicuro. Questo canale è da considerarsi privato: una volta che avviene la disconnessione del supplicant, allora l'autenticazione deve essere rimossa.
- L'authenticator manda il pacchetto EAP (Extensible Authentication Protocol) all'authentication server. Questo canale deve considerarsi sicuro; il protocollo non ci dice come.
- Il server esegue i passi necessari per capire se il supplicant è effettivamente chi dice di essere. In questa fase ci possono essere anche uno scambio di informazioni tra il server e il supplicant.
- Passata la fase di autenticazione il server fornisce una serie di autorizzazioni al supplicant in base alle operazioni che la rete mette a disposizione. Il server dopo questa fase non è più coinvolto.
##### Osservazioni
Il supplicant prima di ricevere le autorizzazioni dal server non può mandare pacchetti nella rete, cioè non può mandare indirizzi IP.
Per questo motivo questo protocollo funziona a livelllo 2 di TCP/IP.
##### EAP
EAP è un protocollo (di livello 5, applicativo) che gestisce l'autenticazione vera e prorpia: ci dice il formato dei messaggi e il processo che i partecipanti devono usare.
Non ci dice nulla però sul tipo di autenticazione da utilizzare. È però un protocollo estendibile:i sono infatti vari metodi che implementano EAP e che specificano il modo in cui eseguire l'autenticazione, permettendo così di scegliere la soluzione migliore in base alla situazione.
##### EAPOL
Il canale tra supplicant e authenticator è da considerarsi non sicuro. Per questo motivo il pacchetto EAP viene incapsulato in un protocollo (di livello 2, connessione) chiamato EAPOL, che serve a proteggere l'integrità dei dati scambiati.
##### RADIUS/Diameter
Il canale tra authenticator e authentication server deve essere sicuro. Il protocollo RADIUS non garantisce la sicurezza, ma la da per scontata: ci deve essere a monte un processo (il protollo non specifica quale) di autenticazione tra authenticator e server.
Il protocollo RADIUS incapsula i pacchetti EAP nel canale tra authenticator e authentication server. È uno dei protolli AAA, autentication, autorization e accounting.
Ha molte vulnerabilità ma è presente in così tante applicazioni (stesso discorso di IPv4 e IPv6) che è difficile da aggiornare al suo successore Diamter.