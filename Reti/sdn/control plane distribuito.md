Il controller del [control plane](control%20plane.md) è un processo software [virtualizzato](virtualizzazione.md) in modo che sia indipendente dalla macchina su cui gira.
Quando la rete scala troppo, cioè quando il numero di switch che il controller deve gestire aumenta più di quanto le risorse (e.g., memoria, NIB e numero di comunicazioni) del controller possano supportare, allora anche il control plane deve scalare. Quello che succede è far scalare il numero di processi che gestiscono il data plane, ognuno dei quali è responsabile di una partizione (detta isola) di tutti gli switch: è importante che le isole non siano in overlapping perché uno switch deve essere gestito da un solo controller, perché la comunicazione tramite [OpenFlow](OpenFlow.md) è un canale permanente e non rimovibile.
Questo vuol dire che il controller logicamente ha una vista globale su tutta la rete, ma questa vista globale è implementa con un [algoritmo del consenso](legge%20del%20consenso.md) tra nodi distribuiti. Per implementare questo serve un'interfaccia east/west in modo che i controller possano comunicare tra loro.
# Vantaggi
I vantaggi di distribuire il control plane sono:
- Scalabilità
	Si passa da una relazione uno-molti a una molti-molti.
- Affidabilità
	Il singolo controller non diventa un SPF.
- Privacy
	Ogni partizione della rete può essere gestita le informazioni con una policy di sicurezza diversa.
- Incremental deployment
	I dispositivi di rete possono essere legacy (i.e., vecchi) oppure di nuova generazione. Questo permette di usare la rete vecchia con il nuovo paradigma.
# Gerarchia
Dal momento che abbiamo molti controller dobbiamo dare un'architettura anche a questi. Abbiamo due possibilità:
##### Un livello
I controller sono tutti sullo stesso livello che comunicano tramite interfaccai west/est.
Si usa questa architettura quando l'unica informazione che i controller devono gestire è il NIB; se la rete cresce troppo allora l'algoritmo del consenso per lo scambio di informazioni potrebbe saturare tutta la banda.
##### Due livelli
Abbiamo un controller di controller che comunicano tramite interfaccai est/west.
Si usa questa architettura quando vogliamo avere un controller con una visione globale inter-domionio; in questo caso il controller root non ha idea di come funziona lo scambio di pacchetti.
# Master-slaves
Per aumentare l'affidabilità all'interno di un dominio (i.e., di un'isola) possiamo implementare un'architettura [master-slave](reliability%20block%20diagram.md#Stand-by): il master ($OFPCR\_ROLE\_MASTER$) ha permessi di lettura e scrittura sugli switch ed è sempre attivo; lo slave ($OFPCR\_ROLE\_SLAVE$) ha solo permessi di lettura ed entra in gioco se il master fallisce. C'è in realtà un terzo ruolo, quello di equal (i.e., $OFPCR\_ROLE\_EQUAL$), per cui più controller hanno permessi in lettura e scrittura; questa soluzione è usata quando abbiamo più controller (i.e., nel classico caso distribuito).
##### Switch
Lo slave monitora il traffico dell'interfaccia sud e nord del master; se rileva che qualcosa non funziona allora entra in gioco e inizia a comportarsi da master. Questo funziona perché l'indirizzo IP (virtuale) del master e dello slave sono gli stessi e quindi sono autorizzati dallo switch alla scrittura (i.e., alla riprogrammazione delle tabelle di routing).
##### Hot stand-by
Lo slave è in hot standby perché quando non è attivo il suo compito è interrogare gli switch e ricevere informazioni sul NIB. Questo gli permette, quando deve entrare in gioco, di avere la maggior parte delle informazioni (alcune mancano perché il master potrebbe aver cambiato qualcosa dall'ultima interrogazione).