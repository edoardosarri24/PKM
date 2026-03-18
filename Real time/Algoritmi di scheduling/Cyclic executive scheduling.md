È uno degli algoritmi più usati in ambito militare e per il controllo del traffico. Questo perché è molto predicibile e quindi molto adatto alle situazioni di hard real-time; di contro si riducono le prestazioni e l'efficienza.
# CARATTERISTICHE
- Usato per i task puramente periodici
- Offline
- Clock-driven
- Statico
- Non preemptive
### Vantaggi
- Implementazione semplice
	Si deve solo eseguire un piano già stabilito. Serve più il disparcher che lo scheduler.
- Pochissimo overhead a runtime.
### Svantaggi
- Non robusto durante l'overload
	Se si modifica di poco le caratteristiche dei task ci sta che si debba riprogettare lo scheduler da capo.
	Questo accade soprattutto se si cambia il periodo di un task, visto che incide sia sul minor che major cycle; il cambiamento del solo WCET è meno dannoso.
- Non gestibile con i task aperiodici
	Una possibile soluzione potrebbe essere riservare uno slot di tempo solo per questi.
# FUNZIONAMENTO
[Esempio](Cyclic%20executive%20scheduling%20timer.png)
- Il tempo è suddiviso in intervalli di tempo della stessa durata $\Delta$ detti time slot o minor cycle. Solitamente $\Delta=MCD(T_1,\cdots,T_n)$, con $T_i$ periodo del task $\tau_i$.
- Uno o più task sono associati a ogni time slot in modo che la somma dei WCET in ogni time slot non sia superiore a $\Delta$. Questo procedimento viene fatto a mano, solitamente partendo dal task con periodo minore e quindi anche deadline più vicina (perché i task sono puramente periodici).
- L'esecuzione in ogni time slot avviene tramite timer ogni $\Delta$ tempo. Il timer attiva il dispatcher che manda in esecuzione quanto deciso in modo statico.
- Lo schedule si ripete ogni intervallo di tempo $T$ detto major cycle. Solitamente $T=mcm(T_1,\cdots,T_n)$, con $T_i$ periodo del task $\tau_i$.