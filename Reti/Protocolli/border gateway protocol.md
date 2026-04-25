Il Border Gateway Protocol (BGP) è un [exterior Router Protocol](routing.md#Exterior%20Router%20Protocol) utilizzato per connettere due provider diversi che gestiscono parti di rete diverse.
# Accordi commerciali
L'idea è che una volta che due provider $A$ e $B$ fanno un accordo, se una di queste due parti fa un nuovo accordo con $C$ allora i tre sono tutti connessi.
I provider possono aggiungere anche metriche per favore o non favorire l'instradamento del traffico: per un provider potrebbe essere più o meno costoso far passare il traffico da un provider rispetto a un altro. Questo è il motivo per cui la maggior del traffico passa per l'America.
# Funzionamento
Se abbiamo due AS che devono essere interconessi tramite due router (i border router), allora il funzionamento è il seguente:
- I due router inizieranno a conoscersi tramite IP. Smetteranno di conoscersi solo quando il contratto è terminato.
- I router si mandando delle informazioni, dette NLRI (Network Layer Reachability Information), che contengono delle tabelle con le sotto reti raggiungibili che ogni router può raggiungere.
- Queste informazioni sono poi passate ai router di ogni AS in modo che i nuovi indirizzi siano resi disponibili e che essi possano aggiornare i proprio RIB (Routing Information Base).
# SDNi
È l'estensione di BGP, definita da IETF, alle reti [sdn](sdn.md) che permette di capire quali sono i flussi scambiati.
Oltre al classico comportamento di BGP, questa estensione permette di traferire, oltre alle informazioni sulle destinazioni raggiungibili (in questo caso flussi che possono passare da quel AS), anche altre due informazioni:
- Costo
	Conoscendo anche il costo dei percorsi, si passa da un approccio basato sul numero di salti a un approccio basato sulle performance e sul [QoS](Reti/misure.md#QoS).
- Capacità
	Versioni dei software utilizzati e del tipo di serivizio che può passare dalla rete (e.g., VPN si o no).
##### Wrapper