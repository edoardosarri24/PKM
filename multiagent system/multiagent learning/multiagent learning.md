In sistemi multi [agente](agenti.md) ci sono delle situazioni in cui vogliamo apprendere un modello di [machine learning](Machine%20learning.md) (link da non da sapere. si deve sapere [Apprendimento supervisionato](Apprendimento%20supervisionato.md)) in modo distribuito. Possiamo vederla anche come un task di [coordinazione](multiagent%20system/multi%20objective%20MAS/coordination/task.md) dove lo stato goal sono i pesi globali.
# Motivazioni
Alcune motivazioni possono essere:
- Ci sono più agenti che hanno dati diversi che non devono essere condivisi.
- Utilizzare un apprendimento centralizzato magari è troppo costoso.
- Per ragioni di privacy i dati possono essere usati solo da chi li possiede.
# Formalmente
Se abbiamo $N$ agenti e un insieme di dati partizionato in $N$ sottoinsiemi $K_i$, allora vogliamo che ogni agenti ottimizzi i propri pesi locali $w_i$ in funzione dei propri dati $K_i$ e poi scambi informazioni (qualcosa che non siano i dati) con i suoi vicini $j\in N_i$; l'obiettivo è far [sincronizzare](multiagent%20system/multi%20objective%20MAS/coordination/task.md#Ottimizzazione%20distribuita) i pesi a un valore comune che minimizza una funzione di costo globale $J(w)$, cioè che $\displaystyle\lim_{t\to\infty}w_i(t)=w^o=\arg\min_wJ(w),\ \forall i$.
# Modelli
- [distributed linear least-squares](distributed%20linear%20least-squares.md)
- [distributed consensus learning](distributed%20consensus%20learning.md)