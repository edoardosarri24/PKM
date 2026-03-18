MetodoGli approcci degli [algoritmi di scheduling](algoritmi%20di%20scheduling.md) sono detti analitici. Infatti si assume che i task abbiano un WCET e si cerca di dare un risultato valido per un taskset sotto determinate assunzioni. Queste assunzioni sono però molto rigide, ad esempio si considerano task periodici o puramente periodici, indipendenti (che non condividono risorse). In questo modo si ottengono risultati (i test di schedulabilità) facili da analizzare, ma che sono pessimistici e molto rigidi, cioè validi solo in alcuni contesti con restrizioni stringenti.
Tramite metodi state space based si vuole supportare la modellazione e l'analisi di un'ampia classe di sistemi real-time e fornire un risultato valido per ogni taskset che possa essere modellato. L'aspetto negativo è la complessità rispetto agli approcci analitici.
# Reti di petri
[[Petri Net (PN)]]
[[Time Petri Net (TPN)]]
[Preemptive Time Petri Nets (PTPN)](Preemptive%20Time%20Petri%20Nets%20(PTPN).md)
[Timed Automata](Timed%20Automata.pdf)
# Tool
[[ORIS 2]]