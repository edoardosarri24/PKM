Per cyber security si intende la protezione di un sistema e della rete da danni hw, sw e dei servizi che forniscono.
# TRIADE CIA
Si parla di triade CIA per intende confidenzialità, integrità e avability. Questa è stata poi espansa con altri tre principi: l'autenticazione, l'autorizzazione e la non-repudiation.
# THREAT
- Di tipo fisico, come la clonazione, la sostituzione e la rottura.
- Di comunicazione, come gli attacchi man in the middle e DoS. In una rete mesh non possiamo avere una sicurezza E2E perché non abbiamo la certezza che i nodi intermedi siano affidabili.
- Di privacy
# SOLUZIONI
Aggiungere sicurezza vuol dire aumentare il numero di informazioni trasmette; questo porta a un overhead nel numero di byte trasmessi, che sappiamo ad esempio da [802.15.4](LRWPAN.md#802.15.4) che sono pochi (128 byte).
Per risolvere questo problemi si può:
- Minimizzare la ridondanza di sicurezza tra layer.
- Usare algoritmi di crittografia leggeri.
- Usare chiavi corte. Ovviamente questo porta a una perdita di sicurezza.
# NETWORK LAYER
A livello di rete il protocollo standard è IPsec di IPv6, il quale permette la confidenzialità, l'integrità, l'autenticazione in modo indipendente dalla scelta del livello di trasporto.
Per fare questo IPsec usa header aggiuntivi e questo porta a overhead.
# TRANSPORT LAYER
Il protocollo più usato è TLS. Questo sta sopra TCP e quindi volgiamo una soluzione più leggera che si basi su UDP.
La soluzione è DTLS che sta sopra UDP e lavoro con pacchetti più piccoli. È lo standard quando l'applicazione layer è [CoAP](CoAP.md).
- Vantaggi
	- Permette di riusare molte implementazioni di TLS.
	- Permette un controllo degli accessi.
	- Garantisce la sicurezza con un singolo hop.
- Svantaggi
	- Introduce un overhead.
	- È limitato dalla dimensione dei pacchetti 802.15.4.
	- Non si garantisce la sicurezza E2E in una rete mesh a causa dei multi hop.
# MECCANISMI
### Chiave simmetrica
Si sua la stessa chiave per la cifratura e la decifratura; la chiave deve essere condivisa.
Solitamente si usano funzioni semplici e veloci come lo xor o lo shift.
- Stream-based
	Funziona bene su una sequenza lunga di byte (teoricamente infinita), come l’OTP. Per avere uns corretta cifraturs devo avere uno stream (cioè sapere dove si trovano i pacchetti nella sequenza); posso farlo sul TCP, ma non sull’UDP perché non ho uno stream ma ho dei pacchetti isolati.  
- Block-based
	Funziona bene su un blocco di byte. Funziona bene quindi su UDP perché ogni pacchetto è un blocco.
### Chiave asimmetrica
L'esempio più importante è RSA, che però non si usa perché troppo pensante. Si usa invece la crittografia a curva ellittica, che permette di ottenere gli atessi vantaggi ma con chiavi più corte.