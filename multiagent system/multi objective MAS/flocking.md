Si tratta di un [obiettivo](multi%20obiettivo%20in%20MAS.md) in un sistema multi agente per cui l'insieme di agenti si deve muovere con la stessa velocità (in senso vettoriale); si può vedere come un problema di [sincronizzazione](multiagent%20system/multi%20objective%20MAS/coordination/task.md#Sincronizzazione) sulla velocità e non sulla posizione.
# Soluzione centralizzata
Una soluzione centralizzata del problema è il leader following: definito un leader nel sistema che è indipendente dagli altri (non riceve informazioni dagli altri), tutti gli agenti dovono seguire tale leader.
##### Grafo diretto
Siccome il leader è indipendente dagli altri, cioè non ascolta il parare di altri agenti, il grafo che definisce il sistema diventa in questo modo un [grafo diretto](grafi%20diretti.md). Si trasforma il grafo indiretto facendo in modo che il leader abbia solo archi uscenti: non ci interessa che l'informazione vada da un nodo non leader verso il leader.
##### Policy
La policy è semplicemente $u_i(t)=v_0$, dove $v_0$ è la velocità imposta dal leader. L'unica restrizione è che $v_0$ deve essere realizzabile dagli altri agenti.
# Soluzione distribuita
Una soluzione distribuita del problema è usare [APF](artificial%20potential%20field.md) con il [double integrator dynamics](double%20integrator%20dynamics.md), dove possiamo usare l'informazione sulla velocità.
Il potenziale che dobbiamo definire è a seconda dell'obiettivo:
- Sincronizzazione della velocità a zero
	Ci serve una funzione potenziale che abbia un minimo in 0. La più semplice può essere $J_{vel}=\displaystyle\sum_{i=1}^N||v_i||^2$.
- Sincronizzazione della velocità a $v_0$
	Ci serve una funzione potenziale che abbia un minimo in $v_0$. La più semplice può essere $J_{vel}=\displaystyle\sum_{i=1}^N||v_i-v_0||^2$.
- Sincronizzazione della velocità qualunque
	Un possibile potenziale è $J_{vel}=\displaystyle\sum_{\{i,j\}\in E}||v_i-v_j||^2$.