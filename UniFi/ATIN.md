- [misure](Reti/misure.md)
- [tipi di traffico](tipi%20di%20traffico.md)
- [congestione di rete](congestione%20di%20rete.md)
	Non le tecniche
- [routing](routing.md)
	No classificazione
- [sdn](sdn.md)
- [data plane](data%20plane.md)
- [control plane](control%20plane.md)
- [control plane distribuito](control%20plane%20distribuito.md)
- [OpenFlow](OpenFlow.md)
- [network configuration protocol](network%20configuration%20protocol.md)
- [border gateway protocol](border%20gateway%20protocol.md)
- [OpenDaylight](OpenDaylight.md)
- [ONOS](ONOS.md)
- [REST](REST.md)
	No risorse
- [application plane](application%20plane.md)
- [monitoring](monitoring.md)
- [traffic engeneering](traffic%20engeneering.md)
- [cloud computing](cloud%20computing.md)
- [information comunication network](information%20comunication%20network.md)
- [network function virtualizzation](network%20function%20virtualizzation.md)
# Esame
Come si fa:
- Capire che servizio di rete si deve implementare.
	Questo si può guardare dal punto di vista di un client: cosa ottiene con la nostra implementazione?
- Flussi
	Capire quali sono i flussi che il servizio deve fornire: un flusso è un passaggio di dati.
	Ad esempio in un NAT abbiamo un flusso di registrazione, uno di richiesta e uno di risposta.
- Capire dove i flussi transitano, cioè capire dove mettere gli switch sdn.
- Definire il control plane.
	Può essere centralizzato o distributo.
- Capire come il controllore controlla il data plane tramite openflow table e openflow rules.
	Mettere qualche tabella o qualche regola: ogni regola avrà un match e un action; si possono usare i contatore, i timer.