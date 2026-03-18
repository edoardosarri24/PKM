Fino al 2017 in ambito secuity si parlava di computer security, cioè della protezione di un sistema informativo automatizzato per preservare l'integrità, la disponibilità e la riservatezza delle sue risorse (hw, sw dati e telecomunicazioni).
Nel 2017 è stata definita l'information security, cioè la protezione delle informazioni e dei sistemi informativi da accessi non autorizzati, dalla divulgazione, modifica o distruzione per garantire la riservatezza, l'intergrita e la disponibilità.
La crittografia garantisce l'access control e la protezione dei documenti; quest'ultima proprietà viene garantita tramite l'integrità, la confidenzialità, l'autenticazione e la non repudiation. La crittografia non garantisce l'avaibility.
# PROPEITÁ SICUREZZA
La security deve essere centrata e ponderata in base all'asset e le sue proprietà. È importante che queste proprietà siano ortogonali, cioè ben separate l'una dall'altra e non indipendenti; in questo modo possiamo quantificare la sicurezza di un sistema tramite una valore più semplicemente.
Le proprietà che descrivo l'asset sono quelle sotto; si possono considerare solo confidentiality, integrity e availability, che insieme sono dette CIA.
- Availability
	Proprietà per cui un servizio deve essere sempre raggiungibile. È una proprietà molto difficile da garantire perché richiede una progettazione di rete molto accurata; inoltre si dovrebbe garantire rispetto al caso pessimo, cioè quando ci sono molte richieste nello stesso momento (cosa difficile da prevedere).
	Un altro aspetto che mira la disponibilità sono gli attacchi DoS, che esaurendo le risorse non permettono di avere un servizio disponibile.
- Authentication
	Il destinatario dei dati deve essere sicuro che il mittente sia chi dice di essere. Per ottenere l'autenticazione si usano le firme digitali.
- Confidentiality
	È la proprietà per cui i dati scambiati devono rimanere segreti tra mittente e destinatario. In questo contesto si dovrebbe distinguere tra dati in transito (l'attacctante deve essere on path) e dati fermi (l'attaccante deve rubare l'hard disk). Il modo per aggiungere la confidenzialità è la crittografia dei dati.
- Integrity
	È la proprietà per cui i dati devono raggiungere la destinazione senza essere alterati. È possibile alterare i dati criptati senza decriptarli e quindi anche senza capirli. Per ottenere l'integrità si usano funzioni hash.
- Non repudiation
	È la proprietà per cui chiunque invii un messaggio non può negare di averlo inviato. È importante quando i documenti vengono scambiati. Si utilizzano tecniche di firma digitale.
- Access control
	Assegnare a ogni utente un profilo e permettere le varie operazioni sulla base di questo. Dopo una violazione di sicurezza il controllo degli accessi è la prima cosa che si controlla. Per realizzarlo si utilizzano tecniche di accounting.
# HASH
Sono funzioni che aiutano a verificare l'integrità di un messaggio: il mittente manda il messaggio cifrato insieme al suo hash; il ricevente può ricalcolare l'hash del messaggio e confrontarlo con quello ricevuto in modo da verificare s eil messaggio è stato contraffatto. Il problema è che se mandiamo insieme messaggio e hash l'attacante porebbe modificarli entrambi portando un messaggio integro ma non autentico (cioè il cui mittente non è quello desiderato).
Sono diverse dalle funzioni di error dectection o error correction: queste servono a verificare che un canale non introduca (su una basa statistica) errori a causa della trasmissione; un attaccante modifica invece un messaggio in modo ragionato (su base logica).
### Proprietà
Osserviamo che le ultime tre propeità sono disgiunte, cioè nessuna è più forte delle altre e nessuna ne implica un'altra.
Computazionalmente impossibile non vuol dire impossibile, ma vuol dire che si dovrebbero provare tutti i valori possibili. Il suo contrario è avere una forte dipendenza statistica tra un bit in output e qualche byte in input.
- Dimensione
	L'out deve essere piccolo rispetto all'input. Allora si deve avere che $H:X\to Y,$ dove $dim(Y)<<dim(X)$.
- Velocità
	La funzione hash $y=H(m)$ deve essere veloce da calcolare, visto che spesso sono calcolate su input molto grandi. Solitamente sono realizzate tramite shift o xor, che sono funzioni veloci da calcoalre.
- Controimmagine 1
	Dato un valore hash $h$ deve essere computazionalmente impossibile trovare un qualunque messaggio (le funzioni hash sono non invertibili, quindi ci sono più messaggi che danno lo stesso valore hash) $m$ tale $h=H(m)$.
- Controimmagine 2
	dato un messaggio $m$ deve essere computazionalmente impossibile trovare un messaggio $m'$ tale che $H(m)=H(m')$.
- Collisione
	Deve essere computazionalmente impossibile trovare due messaggi distinti $m$ e $m'$ tali che $H(m)=H(m')$.
# HMAC
Abbiamo detto che le funzioni hash sono utili per garantire l'integrità. Il keyed-Hash Message Authentication (HMAC) serve per garantire l'autenticazione.
Con HAMC il messaggio è hashato con due input, il messaggio e un segreto (una chiave). L'algoritmo è $HMAM(K,m)=H((K'\oplus\ opad)||H(K'\oplus\ ipad)||m)$, dove:
- $||$ rappresenta la concatenazione.
- $\oplus$ è lo xor.
- m è diviso in blocchi di $j$ bit, dove $j$ dipende dal tipo di funzione hash usata.
- $K'$ è derivato da $K$:
- $K'= K$ se $K$ è lungo $j$ bit.
- $K'=K||pad(0)$ se $K$ è lungo meno di $j$ bit.
- $K'=H(K)$ se $K$ è lungo più di $j$ bit.
- $ipad$ e $opad$ sono interi già definiti e servono per evitare che l'attaccante possa fare un attacco logico, cioè possa sfruttare la correlazione statistica della chiave $K$.
### Vantaggi
- L'autenticazione è fornita dal fatto che entrambi i partecipanti debbano conoscere la chiave $K$. È comunque un'autenticazione debole: ci dice che il messaggio è stato mandato da qualcuno che conosce la chiave $K$, non che questo sia effettivamente chi dice di essere.
- Il messaggio e l'hash (cioè $HMAC(K,m)$) possono essere mandati insieme: solo chi possiede $K$ può decifrarlo; un attaccante potrebbe modificare il messaggio ma non l'hash (a meno che non conosca $K$).
### Svantaggi
- Si deve trovare un modo sicuro per condividere la chiave.
- È un metodo soggetto a un attacco brutale, cioè un attaccante che individua il pacchetto potrebbe predire $K$. Solitamente attacchi di questo tipo sono computazionalmente impossibili perché per provare tutte le chiavi ci vorrebbe migliaia di anni; il problema è se si può fare un dictionary attack, cioè se la chiave è una parola esistente e quindi per trovarla si devono provare solo le parole del dizionario (ci vogliono pochi minuti).
# CRITTOGRAFIA SIMMETRICA
Nella crittografia simmetrica la chiave per cifrare e decifrare il messaggio è la stessa (e spesso anche l'algoritmo).
Si può usare la crittografia simmetrica contemporaneamente al metodo HMAC: si genera un HAMC con la chiave K; si cifra $m+HMAC(K,m)$ con una seconda chiave $K'$; in questo modo si garantisce la confidenzialità, l'integrità e l'autenticità.
### Svantaggi
- Non ci garantisce l'autenticazione, perché non possiamo essere sicuri che la chiave non sia andata qualcun'altro.
- Sono gli stessi di [HMAC](Crittografia.md#Svantaggi). Però in questo caso fare un dictionary attack è più difficile, perché non si ha un hash con ui sapere se il messaggio è stato decifrato nel modo corretto.
# CRITTOGRAFIA ASIMMETRICA
I due partecipanti hanno 2 chiavi, una pubblica e una privata.
Per la chiave privata deve essere impossibile computazionalmente ottenerela da quella pubblica.
### Vantaggi
- Si garantisce la confidenzialità, perché ciò che è cifrato con la chiave pubblica può essere decifrato solo con la chiave privata.
- Si può garantire anche l'integrità e l'autenticazione. Oltre a cifrare il messaggio con la chiave pubblica del ricevente si cifra l'hash con la chiave privata del mittente; in questo modo solo il mittente può aver generato l’hash (perché solo lui può averlo cifrato), ma questo è verificabile da tutti (decifrando l'hash con la chiave pubblica del mittente).
- Mittente e ricevente non devono essere a conoscenza della stessa chiave. Se perdiamo la chiave privata è solo colpa nostra.
### Svantaggi
- Chiave pubblica e privata sono intercambiabili. Questo permette di ottenere dei vantaggi, ma si deve anche far attenzione a dove è la chiave privata.
- Solitamente le chiavi vanno generate nuovamente dopo un certo lasso di tempo.
- È più lento rispetto alla crittografia a chiave privata.
# FIRMA DIGITALE
La firma digitale si fa scambiando le chiavi pubbliche e private: il mittente cripta il messaggio con la chiave privata e il destinatario lo decifra con la chiave pubblica del mittente.
Questo metodo garantisce l'autenticazione e la non ripudiabilità, ma non garantisce la confidenzialità.

---
# CRIPTO ANALISI
La cripto analisi si occupa di analizzare i messaggi cifrati. Dato $m$ non cifrato e $c$ la cifratura di $m$ con una chiave $k$, allora chi esegue cripto analisi vuole, dato $c$ ottenere o $k$ o $m$.
Ci sono due proprietà che evitano che si possa fare cripto analisi:
- Confusione
	Nasconde la relazione tra testo cifrato e chiave. Ogni bit del testo cifrato deve dipendere da più parti della chiave.
- Diffusione
	Nasconde la relazione tra testo cifrato e testo in chiaro. Se cambiamo un solo bit del testo in chiaro allora devono cambiare più bit del testo cifrato e viceversa.
# RSA
La forza di RSA si basa sulla matematica che ci sta dietro: essa ci fornisce come funzione trappola il logaritmo discreto. Una funzione trappola è una funzione che è invertibile con facilità solo se ho una determinata conoscenza, altrimenti è computazionalmente impossibile.
### Logaritmo discreto
Esso può essere definito nel modo seguente: sia $A$ l'insieme di primi contenuti in $[0,p]$ con $p$ primo; definiamo la potenza discreta tra $a$ e $b$ come $a\otimes b = a^b(mod\ p)$; il logaritmo discreto di $x$ in base $a$ è quel numero $b$ tale che $a^b(mod\ p)=x$.
La difficoltà della sua risoluzione sta nel fatto che non è una funzione continua: non si possono utilizzare quindi i metodi di bisezione e di approssimazioni successive. Per risolverlo si deve usare la forza bruta, cioè provare tutte le possibilità che abbiamo.
##### Complessità
Il calcolo del logaritmo discreto è un problema np-completo (fino a prova contraria), cioè non ci sono algoritmi che lo risolvano in tempo polinomiale. Il modo ci sarebbe se avessimo abbastanza bit quantistici: implementando l''algoritmo di shor (che trova i fattori primi di un intero in tempo polinomiale) su un computer quantistico.
Quando si parla della complessità lineare/polinomiale/esponenziale di un problema, si vuole sottolineare il fatto che a un aumento lineare dell'input, il costo per risolvere il problema cresce in modo lineare/polinomiale/esponenziale/. Questo implica che anche se la velocità per risolvere un problema diminuisce (tra il 1994 e il 1996 l'efficienza degli algoriti per rompere RS è migliorata del 20%), si può aumentare il nostro carico computazionale (in questo caso la dimensione della chiave), per far aumentare il costo di risolvere il problema più che linearmente.
# DIFFIE-HELLMAN-MERKLE (Diffie-Hellman)
È un algoritmo del 1996 basato sul logaritmo discreto. Non è un metodo di crittografia (non c'è alcuno scambio di messaggi), ma un metodo per decidere in modo sicuro un segreto condiviso; si basa sullo scambio di chiavi personali e non conosciute da altri.
### Funzionamento
Si sceglie in modo pubblico (conosciute da tutti) un primo $p$ molto grande e una radice primitiva $\alpha$ di $p$ (si può dimostrare che dato un primo esiste sempre una sua radice primitiva), cioè un numero $\alpha$ tale che $\alpha(mod\ p)$ ha un ordine moltiplicativo di $p-1$, ovvero tale che $\alpha(mod\ p)\ne\alpha^2(mod\ p)\ne\cdots\ne\alpha^{p-1}$.
- Alice sceglie un segreto $X_a$ e BOB $X_b$.
- Alice manda a Bob $Y_a=\alpha^{X_a}$ e BOB manda ad Alice $Y_b=\alpha^{X_b}$.
- Alice calcola $K_a=Y_b^{X_a} (mod\ p)=(\alpha^{X_b})^{X_a}(mod\ p)$.
- Bob calcola $K_b=Y_a^{X_b} (mod\ p)=(\alpha^{X_a})^{X_b}(mod\ p)$.
- Alice e Bob condividono lo stesso segreto $K=K_a=K_b$.
##### Osservazioni
- L'algoritmo deve essere affiancato a un metodo di identificazione in modo da essere sicuri che $Y_a$ e $Y_b$ siano mandati da Alice e Bob e non da un attaccante che si trova nel mezzo. È quindi soggetto all'attacco Man-In-The-Middle (MITM) (detto anche on-path attack), perché un attaccante potrebbe sostituire $Y_a$ e $Y_b$ con qualcos'altro.
- Se $\alpha$ non è una ridice primitiva allora esiste un $k<p$ tale che $\alpha^k(mod\ p)=\alpha(mod\ p)$; in questo modo l'attaccante non dovrà usare la forza bruta sull'insieme $[1,p-1]$ ma su uno spazio più piccolo.
# ONE TIME PAD
È l'algoritmo a chiave simmetrica considerato perfetto.
One time pad non contiene le due proprietà di confusione e diffusione; questo ci va bene lo stesso grazie al fatto che la chiave è perfettamente randomica.
### Funzionamento
- Sia dato un messaggio $m$ e una chiave $k$ perfettamente randomico e lunga quanto $m$. Non si può e non si deve usare $k$ per più di un messaggio.
- La cifratura è $c=m\oplus k$.
- La decrifratura è $m=c\oplus k$.
### Vantaggi
- Lo xor è invertibile, veloce e semplice da implementare.
- Per l'attaccante fare è impossibile fare cripto analisi, cioè dato $c$ ricavare o $m$ o $k$. Questo succede perché $c$ ha la massima entropia possibile (data da $k$). L'entropia è la quantità di informazione presente in un messaggio: se l'entropia è massima allora il messaggio non può essere rappresentato da meno bit di quelli di cui è composto. Nel caso di un messaggio randomico l'entropia è massima; nel nostro caso però questo è tutto rumore che l'attaccante non sa trattare.
### Svantaggi
- Il problema principale sta nel generare un $k$ in maniera perfettamente randomica. In generale un numero randomico si può fare in un due modi:
	- Algoritmicamente
		Se è un algoritmo a generare un numero randomico si parla di numeri pseudorandomici: un algoritmo ha uno stato sulla base del quale genera un numero. Partendo dallo stesso stato si ottiene sempre lo stesso numero; per quanto possa essere complica uno stato è fattibile prevedere il numero che verrà generato.
	- Fisicamente
		Si genera il numero dall'osservazioni di un fenomeno fisico non predicibile (una temperatura, il rumore nella ricezione di un'antenna). È il modo migliore, ma deve essere fatto in un ambiente protetto a causa del side attack: non si attacca direttamente il sistema, ma si attacca ciò su cui il sistema si basa (il consumo di corrente può essere indicativo di una qualche operazione, la temperatura attorno a un sensore può essere alterata).
- Si deve trovare un modo per condividere un segreto.
# SOSTITUZIONE
Si cambiano i bit di un messaggio tramite una mappa statica, cioè già decisa in precedenza. Un esempio semplice è il cifrario di cesare.
### Feistel Cipher
Siccome non contiene diffusione e confusione, una cripto analisi è sempre possibile. Per questo motivo c'è una versione che introduce confusione e diffusione.
![Feistel Cipher](Feistel%20Cipher.png)
##### Funzionamento
- Prendere il segreto $k$ e dividerlo in blocchi $k_1,\cdots,k_n$.
- Costruire una funzione di permutazione (le operazioni che possono essere svolte sono le sostituzioni, permutazioni e operazioni aritmetiche) che opera sulla base di un parametro.
- Il mssaggio è diviso in due parti uguali o diverse.
##### Osservazioni
- Sia la funzione che lo xor possono essere implementate via hw e non via sw, quindi sono molto veloci.
- La funzione F introduce la confusione.
- Lo swop delle due parti di messaggio introduce la diffusione.