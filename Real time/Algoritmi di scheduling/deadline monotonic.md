L'algoritmo di scheduling Deadline Monotonic (DM) è un [algoritmo di scheduling di task periodici](Scheduling%20di%20task%20periodici.md) che assegna la priorità ai task in modo inverso rispetto alla loro deadline relativa, come in questo [esempio](DM%20example.png).
# Caratteristiche
- Preemptive
- Statico
- Online
- Estensione di [Rate monotonic (RM)](Rate%20monotonic%20(RM).md) a task non puramente periodici.
- Priority-driven
	Questi algoritmi assegnano una priorità a ogni task e su questa priorità viene implementato lo schedule.
# Ottimalità
Il deadline monotonic è ottimo rispetto alla proprietà di feasible tra tutti gli algoritmi basati su priorità fissa per lo scheduling di task periodici.
Questo vuol dire che valgono contemporaneamente:
- Se uno schedule a priorità fissa è feasible per un task set $\Gamma$ allora anche lo schedule di DM è feasible per $\Gamma$.
- Se lo schedule del DM non è feasible per un task set $\Gamma$ allora non esiste uno schedule a priorità fissa per $\Gamma$.