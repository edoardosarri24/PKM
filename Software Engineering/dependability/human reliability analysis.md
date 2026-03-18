Nei sistemi complessi anche l'umano rientra nel ciclo di vita: più è complesso il sistema più la probabilità di errore dovuto all'uomo deve essere bassa. La human reliability fa parte del campo della [dependability](dependability.md) e si può definire come la probabilità che una persona esegua un'attività nel modo corretto (i.e., che rispetti i requisiti) in un certo istante di tempo senza eseguire attività estranee che possano danneggiare il sistema. L'obiettivo è quello di incrementare le performance e diminuire la probabilità d'errore: si devono creare delle procedure che limitino il numero di situazioni in cui l'umano può prendere decisioni in modo inconsapevole.
# Errore umano
##### Tipi
Ci sono vari tipi di errore umano:
- Non intenzionale
	Azioni errate eseguite senza volontà. Sono la cattiva esecuzione di un buon piano.
- Intenzionale
	Azioni pianificate per degradare il sistema. Sono solitamente la violazione di regole.
- Latente
	Errori dovute a una cattiva progettazione, scarsa qualità o errata manutenzione.
- Near miss
	È il quasi infortunio, cioè un evento che avrebbe potuto causare un danno alla salute dalla persona ma per caso non lo ha prodotto. Solitamente sono trattati come [rischi](rischio.md).
##### Influenze
Un errore può essere influenzato da:
- Esterni
	Sono le influenze derivante dall'ambiente, dagli spazi a disposizione, dalle attrezzature, dai dispositivi di protezione individuali, dalle procedure e dal carico di lavoro.
- Interni
	Sono fattori sociali in un'azienda, le condizioni psicofisiche, l'esperienza e la formazione.
# Norma
La norma di riferimento è la IEC 62508. Sono concetti molto generali che consentono solo di inquadrare il problema.
# Tecniche
Le tecniche si basano tutte sulla stessa pipeline: definizione del problema, analisi dei compiti del sistema, identificazione dell'errore, rappresentazione dell'errore, quantificazione tramite misure e gestione dell'errore.
In generale queste tecniche sono molto soggettive e sono difficili da quantificare. In generale il loro funzionamento è sempre lo stesso: si definiscono situazioni generiche, gli si assegna una probabilità e poi questa viene pesata con dei fattori di correzione.
##### THERP
La Technique for Human Error rate Prediction (THERP) è una delle tecniche più usate oggi e che ha ispirato anche le altre. L'idea è di decomporre il compito dell'umano in sotto compiti più semplici e a ciascuno di essi si assegna una probabilità di errore umano; si crea un albero binario dove la radice è l'evento e i figli sono etichettati con la probabilità di succcesso e di fallimento. A queste probabilità viene poi dato un peso dalla norma tramite un Performance Shaping factor, i.e. 7 valori divisi in tre categorie.
##### TESEO
La Tecnica Empirica per la Stima degli Errori degli Operatori (TESEO) si basa (come la [THERP](#THERP)) sulla scomposizione dei compiti degli umani in sotto compiti più semplici. A ognuno viene assegnata una probabilità di fallimento e di successo sulla base di cinque fattori: tipo di attività; fattore di stress per svolgere l'attività; tipo di operatore che svolge l'attività; ansia relativa alla situazione lavorativa del lavoratore; condizioni ambientali.
##### HCR
La Human Cognitive Reliability (HCR) correla la probabilità di errore con il tempo: l'errore non dipende solo da azioni errate ma anche da attività svolte troppo lentamente quanto l'attività viene portata a termine.
Stima il tempo che ci vuole a svolgere un'attività e poi stima la probabilità che sia stato commesso un errore in base al tempo che l'umano ci ha messo.
##### CREAM
Nella Cognitive Reliability and Error Analysis Method (CREAM) la probabilità di errore viene stimata sulla base di fattori come il tempo a disposizione dell'umano (più lungo è meno probabilità d'errore ci sarà), se le azioni sono state pianificate, la competenza del personale e se l'operatore non ha controllo su un'azione da compiere. Questa stima viene corretta con 9 valori detti Common Performance Conditions, simili ai PSF della [THERP](#THERP).
##### RARA
La Railway Action Reliability Assessment (RARA) è specifica per il settore ferroviario.
Le attività dell'operatore vengono descritte in modo generico e a ognuno di esse viene assegnata una stima della probabilità di errore. La probabilità viene poi corretta dagli Error Producing Conditions, fattori simili a quelli di altre tecniche.