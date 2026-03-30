Mentre la [fault masking](fault%20masking.md) utilizza la [ridondanza](ridondanza.md) statica per implementare la [fault tollerance](fault%20tollerance.md), la dinamic fault masking tende a riconfigurare un sistema quando si rileva un errore per evitare che l'errore in un componente si propaghi all'esterno.
# NMR
Ci sono due possibilità.
##### Reconfigurable Duplication
In un sistema con due moduli ridondati in modo statici si riesce a capire che c'è un errore ma non sappiamo dire in quale si verifica. Con la versione riconfigurabile si aggiungono due capacità: determinale quale dei due moduli è guasto; scollegare quello guasto e procedere con uno solo.
Per decidere quale modulo escludere, si possono usare:
- Programmi di diagnostica, cioè test interni al componente che rilevano se lo stesso è guasto. 
- Self-checking, dove ogni modula ha dei circuiti interni che permettono di rilevare l'errore.
- Watch dog timer.
##### Reconfigurable NMR
In questo caso aggiungiamo moduli e il concetto rimane lo stesso: scollegare i moduli guasti in modo che non possano battere sul voto quelli sani.
Le principali tecniche sono:
- Hybrid Redundancy
	Quando un modulo fallisce, cioè quando il suo output è diverso da quello della maggioranza viene sostituito con una riserva pronta.
- Adapting voting
	I voti assegnati ai vari componenti non hanno lo stesso peso: se un modulo sbaglia spesso il suo peso si abbassa.
# Reliability
Nei sistemi ridondanti e riconfigurabili la [reliability](attributi.md#Reliability) può essere definita come $R_{sys}​=[R_{m}^2+2CR_m​(1−R_m​)]R_k​$, dove $R_{m}$ è la realiability del modulo, $R_{k}$ è la reliability dei circuiti di controllo e/o switch e $C$ è la [coverage](fault%20tollerance.md#Coverage).