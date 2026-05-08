L'idea è di avere un manager dell'infrastruttura che ha una visione su:
- Risorse, sia fisiche che virtuali.
- VM.
Ha dei template per la loro creazione.
- Container.
Ha dei template per la loro creazione.
- Risorse di rete.
Per la migrazione è necessario capire il percorso per l'invio del file di configurazione.

Chi utilizza il datacenter (e.g., chi compra l'utilizzo di n server (virtuali)) lo vorrà monitorare. Per fare questo abbiamo un manger esterno che interagisce con quello interno per monitorare i server virtuali.

MONITORAGGIO
Chi compra un servizio definisce con il venditore uno SLA (service Level Agreement) e si stabiliscono delle meetriche. A questo punto abbiamo un software che campiona il traffico ed estrae delle metriche. A questo punto si confrontano i target definiti dal SLA con quelli ottenuti dal monitoraggio ed eventualmente si avvisa il client e il provider; questo monitoraggio è fatto da un manager. Nel caso in cui queste metriche di monitoraggio non rispettino gli SLA, si deve scalare il numero di container (o VM), magari usando kubernetes.