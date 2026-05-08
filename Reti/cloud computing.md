Il cloud computing non è solo mettere insieme molti calcolatori, ma si tratta di avere una [Rete](Reti%20di%20telecomunicazioni.md) che li collega.
Con cloud computing intendiamo un modello che garantisce l'accesso remoto a risorse di calcolo riconfigurabili (i.e., che variano nel tempo) e condivise.
# Storia
All'inizio avevamo grandi calcolatori che occupavano stanze molto grandi. Per utilizzare questi calcolatori si doveva andare in presenza e aspettare il proprio turno.
Successivamente con le LAN si osno create reti dove più persone avevano accesso al (singolo) calcolatore dallo stesso edificio.
Con i PC e TCP/IP avevamo più computer collegati tra loro.
Poi è arrivato il paradigma del cloud: abbiamo più risorse di calcolo remote con cui possiamo interagire. Questo ha permesso di non possedere più l'unità di calcolo ma di utilizzarla on-demand.
# Caratteristiche
Le caratteristiche principali del cloud computing sono:
- Broad network access
	Le risorse sono accessibili tramite rete in modo remoto da client eterogenei.
- Rapid elasticy
	Le risorse scalano in quantità (memoria) e qualità (voglio una gpu invece che una cpu).
- Measurement service
	Tutti i servizi erogati devono essere misurati (e.g., [monitoring](monitoring.md)) per verificare che i contratti siano rispettati
- On-Demand self-service
	Le risorse sono utilizzabili su chiesta e tramite un contratto stabilito su riesta, i.e. senza che ci sia un qualche consenso.
- Resource polling
	Le risorse sono astratte e condivise tra tutti: le risorse hw sono quelle e poi è l'OS che si occupa di gestirle.
# Service Model
Ci sono più modalità di servizio:
##### IaaS
I subscriber (i.e., chi compra il servizio) vedono risorse di computing, di memoria o di networking. Il publisher lavora in modo virtuale: le richieste sono di macchine vere, quello che in realtà gli danno sono VM o container. Questo permette di non comprare le risorse di rete ma prenderle in prestito.
L'idea è quello che il subscriber possa utilizzare le risorse privatamente oppure farci girare un software e venderlo. 
##### PaaS
 Si compra la piafforma per sviluppare testare e distribuire applicazioni. Esempi sono: un ambiente di sviluppo C++ con le librerie opportune; un sistema operativo.
 Non si lavora sull'hw: il subscriber non si occupa di OS, server e middleware.
##### SaaS
Il subscriber compra il software finito. Si tratta di un'applicazione che gira in un qualche server incapsulata con API rest che permettono l'accesso massivo. Chi lo utilizza non deve installare nulla, ma utilizza l'applicazione tramite un browser.
# Deployment model
Ci sono più modalità con cui un'azienda può esporre il proprio cloud:
- Public
	Sono risorse messe a disposizione per chiunque.
- Private
	Si definisce un gruppo di oggetti che può usare quelle risorse. Un esempio è il cloud di tutti gli ospedali d'italia.
- Hybrid
	Un'azienda ha le proprie risorse per i propri dipendenti.
- Community
	Sono quelli che usano le risorse private per le cose più sensibili e un server esterno per i dati meno sensibili.