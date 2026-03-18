L'Artifical Potential Field (APF) è un algoritmo in stile [reattivo](paradigmi%20di%20agenti.md#Reattivo) per risolvere il problema del [robot motion planning](robot%20motion%20planning.md).
##### Casi d'uso
È utile quando:
- Lo spazio delle configurazioni ha una qualunque dimensionalità, eventualmente molto alta.
- Non richiede che il mondo sia poligonale.
##### Idea
L'idea è quella di definire il moto dell'agente sulla base di una forza $J(q)$ con due termini: una attrattiva verso il goal $J_{att}(q)$ e una repulsiva contro gli ostacoli $J_{rep}(q)$. Questo permette di seguire contemporaneamente due behavoir: muoviti verso il goal; evita gli ostacoli.
La somma delle due forze, $J(q)=k_aJ_{att}(q)+k_rJ_{rep}(q)$ (con $k_a$ e $k_r$ iperparametri), è detta forza potenziale; il robot si deve muovere nella direzione dell'antigradiente del potenziale $-\tfrac{\partial J}{\partial q}$, cioè si deve muovere verso il minimo del potenziale.
# Forza attrattiva
Dato che $q_G$ è il goal, vorremmo definire la forza attrattiva come una funzione che ha un minimo globale nel goal, visto che vogliamo muoverci nella direzione dell'antigradiente.
![forza attrattiva](forza%20attrattiva.png)
##### Quadratica
La funzione più semplice che ha un minimo locale nel goal è la quadratica, definita come $J_{att}(q)=\tfrac{1}{2}||q-q_G||^2$. In questo modo il vettore che mi definisce lo spostamento verso il goal è $-\tfrac{\partial J_{att}}{\partial q}=q_G-q$.
Il problema è che lo spostamento in questo modo è tanto maggiore quanto sono più lontano dal goal. Teoricamente ha senso, ma in pratica il robot ha delle limitazioni fisiche.
##### Lasso
Per risolvere il problema della funzione quadratica si può usa una funzione lineare che ha minimo nel goal, cioè $J_{att}(q)=||q-q_G||$. In questo modo lo spostamento è $\tfrac{\partial J_{att}}{\partial q}=\tfrac{q_G-q}{||q_G-q||}$: questo è lo stesso vettore precedente normalizzato per la distanza, cioè un vettore di modulo unitario.
Risolve il problema della funzione quadratica: l'intensità è la stessa a prescindere dalla distanza dal goal.
Il problema è che non è derivabile in $q_G$: quando mi avvicino al goal non riesco mai a raggiungerlo perché faccio un po' avanti e indietro.
##### Huber
Vuole unire i vantaggi degli approcci quadratici e lasso: lo spostamento è lo stesso a prescindere dalla distanza dal goal, ma la funzione è derivabile nel goal.
La funzione di Huber è definita $J_{att}(q)=\begin{cases}\tfrac{1}{2}\lVert q - q_G \rVert^2 & \text{if } \lVert q-q_G\rVert\le\delta\\\delta\lVert q-q_G\rVert-\tfrac{1}{2}\delta^2& \text{if } \lVert q-q_G\rVert>\delta\end{cases}$: è quindi quadratica vicino al goal e lineare con un fattore dipendente dalla distanza lontano dal gol.
# Forza repulsiva
Definendo $O_k$ come l'ostacolo $k$-esimo, la forza repulsiva è definita sulla base di due principi:
- Raggio di influenza
	Siccome l'approccio che stiamo usando è locale (i.e., non abbiamo informazioni sull'intero ambiente ma solo quelle che ci vengono dai sensori) non ci interessa che gli ostacoli lontani contribuiscano alla forza repulsiva.
	Tale soglia è definita come raggio di influenza ed è nota come $\delta_0$.
- Distanza di sicurezza
	Voglio garantire che il robot non si avvicini troppo agli ostacoli. Questo mi impedisce sia di sbattere contro gli ostacoli ma anche di avvicinarmi; aumentando tale distanza posso anche sopperire alla precisione non perfetta del sensore.
	Tale soglia è definita come distanza di sicurezza ed è nota come $\delta$.
##### Grafico
Possiamo [osservare](forza%20repulsiva%20APF.png) come la forza repulsiva esercitata da un ostacolo va a 0 se la sua distanza è maggiore di $\delta_0$ e va a infinito se la sua distanza si avvicina a $\delta$.
##### Implementazione
Una possibile implementazione di quanto espresso sopra è $J_{att,k}(q)=\begin{cases}0&\text{if } d(q,O_k)\ge\delta_0\\\tfrac{1}{\beta}(\tfrac{1}{d(q,O_k)-\delta}-\tfrac{1}{\delta_0-\delta})^\beta&\text{if } d(q,O_k)<\delta_0\end{cases}$, dove un valore tipico per $\beta$ è 2. La forza repulsiva totale diventa $J_{att}(q)=\displaystyle\sum_kJ_{att,k}(q)$.
Non è l'unica e non va saputa.
# Single Integrator Dynamics
Usando la [dinamica del singolo integratore](single%20integrator%20dynamic.md) si hanno risultati interessanti per l'APF.
##### Policy
Siccome siamo in un algoritmo APF la policy è definita dall'antigradiente di $J(q)=k_aJ_{att}(q)+k_rJ_{rep}(q)$; usando il single integrator dynamics la policy diventa poi $u(t)=-K_a\tfrac{\partial J_{att}}{\partial q}|_{q=q(t)}-K_r\tfrac{\partial J_{rep}}{\partial q}|_{q=q(t)}=-K_P\tfrac{\partial J}{\partial q}|_{q=q(t)}$, dove $K_P$ costante che unisce $k_a$ e $k_r$.
Osserviamo che in questo modo $u(t)$ ha dei problemi di applicabilità in contesti di agenti fisici: ci servono dei limiti, in quanto un'entità fisica non può avere una velocità maggiore di una soglia. Per questo si può imporre una saturazione, definendo $u_i(t)=\tfrac{v_i}{||v_i||}min\{||v_i||,V_{max}\}$ e $v_i=-K_P\tfrac{\partial J}{\partial q}|_{q=q(t)}$; in questo modo stiamo preservando la direzione ma limitando la velocità.
##### Dinamica del robot
La dinamica del robot, cioè la sua posizione e velocità nel tempo, mette in evidenza che ci stiamo muovendo nella direzione dove il potenziale è minimo. Considerando la [sigle integrator dynamic](single%20integrator%20dynamic.md#Definizione), la dinamica diventa:
- Un sistema di gradienti $\dot{q}=-K_P\tfrac{\partial J}{\partial q}|_{q=q(t)}$ nel caso continuo.
- Una discesa del gradiente $q(t+1)=q(t)-\epsilon\tfrac{\partial J}{\partial q}|_{q=q(t)}$, con $\epsilon=T_sK_P$ il passo nel caso discreto.
##### Conseguenze
- Siccome siamo in GD, diventa chiaro che ci stiamo muovendo in modo da minimizzare il potenziale.
- Se il goal $q_G$ non è influenzato da ostacoli, allora diventa un minimo globale.
- Il punto $q_G$ è un punto stazionario assumendo che il passo $\epsilon$ sia piccolo e il potenziale $J(q)$ sia smooth.
##### Completezza
È possibile raggiungere minimi locali in punti influenzati dagli ostacoli in cui la forza attrattiva e quella repulsiva si annullano, cioè punti in cui $J_{att}+J_{rep}=0$. Questo fa si che l'algoritmo non sia completo.
Per rendere l'algoritmo compleso, sotto determinate assunzioni, si può:
- Usare un approccio ibrido
	Si unisce APF con una tecnica [deliberativa](paradigmi%20di%20agenti.md#Deliberato). Si esegue un algoritmo basato su mappe (e.g., [PRM](probabilistic%20road%20map.md), [RRT](rapidly-exploring%20random%20trees.md)) calcolando dei sub goal (algoritmo di alto livello); si raggiungono i sub goal in sequenza usando APF (algoritmo di basso livello).
	Se si incappa nuovamente in un minimo locale allora si riesegue l'algoritmo di alto livello includendo nella mappa i nuovi ostacoli e poi si esegue APF.
	Questo metodo gerarchico garantisce la completezza.
- Cambiare comportamento
	Quando si raggiunge questi punti di minimo locale si può passare a un altro comportamento come seguire il bordo o una random walk.
- Ostacolo virtuale
	Quando si arriva in un minimo locale $q_m$ si aggiunge un ostacolo in $q_m$.
	Questo può funzionare in casi semplici, ma non da garanzie di ottimalità.