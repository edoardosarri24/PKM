# PUNTI CHIAVE
I punti chiave della security sono:
- Confidenzialità
	Non permettere l'accesso alle informazioni a chi non ha l'autorizzazione.
- Integrità
	Le informazioni non devono essere alterate da chi non è autorizzato senza che chi è autorizzato se ne possa accorgere.
- Autenticazione
	Le persone devono essere chi dicono di essere.
# COSTI
- Un sistema sicuro aumenta in complessità.
- Costi effettivi per l'implementazione e l'esecuzione.
- Cambia il workflow: alcune cose non possono essere fatte o comunque sono limitate. In questo dobbiamo essere bravi perché gli utenti se limitati troppo tenderanno a violare il sistema (se le policy di sicurezza non sono rispettate allora sono inutili).
# ENTERPRISE ARCHITECTURE FRAMEWORK (EAF)
È un framework, cioè un modello formale, che descrive un'azienda (assimilabile all'UML per la descrizione del software). È un qualcosa relativo alla gestione aziendale, ma noi dobbiamo capirlo e utilizzarlo.
![EAF](EAF.png)
Solitamente ogni EAF usa un modello iterativo: al passo successivo si specializza la descrizione includendo nuovi dettagli. Ogni passo definisce dei requirements, che saranno presi in input dal passo successivo; se il passo corrente non può definire un output dato l'input allora si torna indietro e si reitera.
- Framework and principles
	Sono i principi dell'azienda, la mission.
- Architecture vision
	È una specializzazione del punto precedente.
- Business architecture
	Da dove vengono le risorse e come vengono spese. Possono essere soldi come aspetti più generali (cibo del caso di una caritas).
- Information system architecture
	Tipi e dati maneggiati dall'azienda.
- Technology architecture
	È la parte IT.
- Altro
	Da qui in poi non ci interessa. Sono aspetti legati ad altri professionisti relative al cambiamento dell'azienda nel futuro.
	- Opportunities and solution
	- Migration planning
	- Implmentation governance
	- Architecture change management
	- Architecture vision
# ASSET
Qualsiasi cosa sia importante per l'azienda, materiale e non; è l'elemento centrale da proteggere. L'obiettivo della security è descrivere questi asset in base ai [punti chiave](Network%20security.md#Punti%20chiave) della sucurity.
Per noi un asset è un dato, un componente HW o uno SW.
### Requisiti
Se un asset ha un requisito (=proprietà) allora tutti gli asset che esso gestisce eredita tale proprietà (se A ha la proprietà P e B usa A, allora B ha R). Questo porta a problemi nel caso in cui due asset abbiamo proprietà che sono in contrasto.
I requisiti di un asset possono essere imposti da:
- Legge (GDPR).
- Azienda, cioè una regola interna all'azienda (proteggere i dati del clinete).
- Regole, cioè qualcosa che ho firmato (non disclosure agreements).
### Gruppi
- Primari
	Qualcosa senza cui l'azienda non può operare, cioè si ferma.
- Secondari
	Qualcosa senza cui la produttività dell'azienda diminuisce.
- Terziari
	Nel caso siao violati a nessuno interessa veramente.
# RISCHIO
La gestione del rischio segue quattro fasi:
- Identificazione
	Per capire cosa è rischio si deve guardare al passato, e quindi prendere quello che già è presente in letteratura, e pensare anche al futuro, cioè pensare a quello che può succedere anche se al momento non è probabile.
- Valutazione
	Ci sono due valutazioni:
	- Qualitativa
		Mette in relazione quanto è probabile che un evento succeda rispetto a quanto è dannoso. [Matrice](Qualitative%20risk%20evaluation.png)
		Si deve trovare un compromesso in base al costo e all'importanza dell'asset: se abbiamo un sistema troppo sicuro allora abbiamo speso troppo; se il sistema è poco sicuro allora dobbiamo fare qualcosa di più.
		Per migliorare la situazione si deve abbassare l'occorrenza o abbassare il rischio.
	- Quantitativa
		Viene eseguita tramite formule. Dati l'asset value AV (facile da calcolare per l'HW, mentre per il SW si può usare i danni che farebbero un suo danneggiamento), l'annual rate of occurency ARO (i problemi variano di anno in anno ed è difficile da prevedere) e l'exposure factore EF, si può calcolare la single loss expectancy SLE=AX * EF oppure l'annual loss expectancy ALE = SLE * ARO.
		Il punto è che non si dovrebbero usare questi risultati come percentuali: un aereo non cade l'1% ogni anno ma cade una volta ogni 100 anni, e quando cade il risultato è un disastro.
- Implementazione
- Manutenzione
# RISK ANALYSIS MODEL
![risk analysis model](risk%20analysis%20model.png)
- Sensitivity level
	È una proprietà dell'asset che non può essere cambiata.
- Protection level
	È quello che è necessario e che dobbiamo raggiungere per proteggere l'asset.
- Doppia cosa
	Si sceglie quale delle due ha un costo minore. Spessa la soluzione è applicarle entrambe.
	- Security policy
		Soluzioni che hanno l'obiettivo di far si che l'evento dannoso non accada (i.e. antivirus, firewall).
	- Countermeasure
		Non tocca il rischio, ma quanto esso ha effetto sull'obiettivo: non si cerca di non far accadere l'evento rischioso ma si vuole che questo non abbia effetto.
- Vulnerability
	È una minaccia potenziale: senza un threat (cioè che l'agente malevolo sfrutta) non succede nulla.
	- Vulnerability analysis
		Ci dice se chi è sotto analisi è soggetto alla vulnerability. Per le cose conosciute è facile: si guarda se l'oggetto è soggetto a quella vulnerabilità. Per le cose sconosciute è come cercare una cosa sconosciuta senza sapere se esiste; spesso si guarda nei posti dove le persone sbagliano sempre.
- Relieability
	Si tratta di eventi che accadano naturalmente.
# THREAT MODEL
Il threat model (descritto nel RFC 3552, un documento redatto dall'Internet Engineering Task Force (IETF)) ci descrive le capacità che un attaccante deve avere per attaccare una risorsa. È indispensabile per definire la probabilità di attacco. Dovrebbe contenere le informazioni sulla:
- Conoscenza
	È riferita alla conoscenza del sistema e si deve sempre supporre che ne sia totalmente a conoscenza (Kerckhoffs’s principle). Nella security ci si deve basare sulla privacy solo se l'oggetto di interesse è facilmente modificabile (e un sistema, soprattutto nel suo design non lo è).
- Capacità computazionali
	Si deve supporre che l'attaccante abbia le milgiori risorse presenti sul mercato. In alcuni casi, come quando l'attaccante è uno stato, si deve pensare che abbia anche di meglio.
- Controllo del sistema
	Nelle analisi si parte da un end point sicuro e una rete che è inaffidabile, cioè non sicura. Un attaccante, rispetto al flusso informativo, può essere:
	- On-path (posizione pevilegiata)
		Riceve e si vede i pacchetti che passano sulla rete. Può modificarli e cancellarli, ma questo espliciterebbe la sua presenza; solitamente acquisisce i pacchetti, li analizza offline, e poi fa un attacco passivo (i.g. violazione di confidenzialità password).
	- Off-path
		L'unica cosa che può fare è fare qualcosa per influenzare la situazione, cioè un attacco attivo (i.g. inserimento, cancellazione e modifica di messaggi).
	- Link-local
		Si tratta della rete a un hop dai dispositivi. È la situazione peggiore; l'unico modo per proteggersi è impedire che un attaccante arrivi alla rete link-local.