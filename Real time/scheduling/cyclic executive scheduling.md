È uno degli algoritmi di [scheduling](scheduling.md) [real-time](real%20time.md) più usati in ambito militare e per il controllo del traffico.
# Requirements
- Usato per i task puramente periodici
- Offline
- Clock-driven
- Statico
- Non preemptive
# Algoritmo
Come si vede da questo [esempio](Cyclic%20executive%20scheduling%20timer.png) l'algoritmo è il sequente:
- Il tempo è suddiviso in intervalli di tempo della stessa durata $\Delta$ detti time slot o minor cycle. Solitamente $\Delta=mcd(T_1,\cdots,T_n)$, con $T_i$ periodo del task $\tau_i$.
- Uno o più task sono associati a ogni time slot in modo che la somma dei WCET in ogni time slot non sia superiore a $\Delta$. Questo procedimento viene fatto a mano, solitamente partendo dal task con periodo minore e quindi anche deadline più vicina (perché i task sono puramente periodici).
- L'esecuzione in ogni time slot avviene tramite timer ogni $\Delta$ tempo. Il timer attiva il dispatcher che manda in esecuzione quanto deciso in modo statico.
- Lo schedule si ripete ogni intervallo di tempo $T$ detto major cycle. Solitamente $T=mcm(T_1,\cdots,T_n)$, con $T_i$ periodo del task $\tau_i$.
# Conseguenze
Le consegue di utilizzare questo algoritmo sono:
- È molto predicibile e perfetto per le situazioni di hard real-time.
- È conservativo dal punto di vista delle prestazioni (e.g., [utilizzo CPU](scheduling%20analysis.md#Utilizzo%20CPU)).
- Non robusto durante l'overload
	Se si modifica di poco le caratteristiche dei task ci sta che si debba riprogettare lo scheduler da capo.
	Questo accade soprattutto se si cambia il periodo di un task, visto che incide sia sul minor che major cycle; il cambiamento del solo WCET è meno dannoso.
- Non gestibile con i task aperiodici
	Una possibile soluzione potrebbe essere riservare uno slot di tempo solo per questi.
- Pochissimo overhead a runtime.
	Permette di non avere un RTOS che esegue lo scheduling: possiamo eseguire tutto sul bare metal.