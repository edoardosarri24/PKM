Nell'architettura [sdn](sdn.md) il data plane è il layer che si occupa dell'inoltro dei pacchetti basandosi sulle regole (i.e., le tabelle di inolro) definite dal [control plane](control%20plane.md) e astrae l'implementazione dei dispositivi di rete.
# Dispositivi
Con il decoupling di sdn, i dispositivi di rete devono implementare (quasi) solamente la data forwarding function: accettano pacchetti da altri dispositivi di rete; utilizzano le forwarding tables per decidere dove mandare il pacchetto ricevuto. Il forwarding si basa su header IP e utilizza TCP e UDP come layer sovrastanti.
Durante questo processo possono alterare l'header del pacchetto, scartarlo, metterlo in una coda per aspettare un altro pacchetto e poi processarli insieme.
##### Porte
I dispositivi di rete usando doverse porte:
- Pacchetti
	Porte di input e ouput per la ricezione e l'inoltro dei pacchetti.
- Interfaccia sud
	Tramite l'[interfaccia sud](control%20plane.md#Sud) il data plane può parlare con il control plane. Solitamente questa interfaccia è implementa da [OpenFlow](OpenFlow.md) e/o [NETCONF](network%20configuration%20protocol.md).
##### CPU
All'interno della rete tradizionale, i dispositivi di rete svolgevano la maggior parte delle loro funzionalità usando la CPU: questa eseguiva l'algoritmo di [routing](routing.md) per decidere dove inoltrare il pacchetto.
Adesso la maggior parte dell'esecuzione viene svolta dal ASIC forwarding, un processore ottimizzato e non general purpose, che svolge la funzione di matching tra il pacchetto in ingresso e quanto continuto dalla tabella di matching. In questo modo la CPU diventa importante solo per gestire le situazioni limite (e.g., pacchetto di controllo, di gestione ed eccezioni).






##### Riprogrammazione
Nelle vecchie reti il processore ASIC era chiuso, cioè il suo comportamento veniva scritto nel momento della produzione del dispositivo. Adesso con [OpenFlow](OpenFlow.md) abbiamo la possibilità di modificare il comportamento di questo chip e permettere al [NOS](control%20plane.md#Network%20Operating%20System) di modificare la tabella di inoltro.
