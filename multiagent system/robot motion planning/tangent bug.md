È un algoritmo in stile [reattivo](paradigmi%20di%20agenti.md#Reattivo) per risolvere il problema del [robot motion planning](robot%20motion%20planning.md).
##### Sensori
Si richiede la conoscenza globale del goal e la conoscenza locale dell'ambiente acquisita tramite range sensor con una sensibilità di $\rho_{max}$: si può vedere l'ostacolo anche a distanza se questo si trova in $[0,\rho_{max}]$.
Solitamente si usa il LIDAR, che girando su se stesso permette di definire il [grafico](LIDAR%20graph.png) della distanza di un oggetto rispetto all'angolo in cui tale misurazione è stata acquisita.
# Comportamenti
Ci sono due comportamenti che vengono alternati.
##### Move to goal
- Se la strada è libera, cioè se il sensore non percepisce ostacoli nella direzione del goal, allora ci si muove verso il goal.
- Se il sensore percepisce un ostacolo allora questo avrà due estremi, cioè due punti dopo i quali non viene percepito nulla.
	In questo caso ci si muove verso l'estremo $O_k$ più vicino. Questa decisione serve per minimizzare la distanza euristica $h_k(q)=d(q,O_k)+d(O_k,q_G)$ da dove siamo ora fino al goal $q_G$; euristica perché è la minor distanza che possiamo supporre dal goal con le informazioni locali attuali, ma non possiamo saperlo in realtà perché ci mancano informazioni globali.
- Cambia comportamento quando la distanza euristica inizia ad aumentare. Questo indica che si è trovato un ostacolo non convesso e che stiamo tornando indietro per aggirarlo.
##### Segui l'ostacolo
Si segue l'ostacolo nella direzione di $O_k$.
Si cambia comportamento quando $d_{reach}<d_{followed}$:
- $d_{followed}$
	È la distanza minima percepita dall'agente dal goal da quando ha iniziato ad aggirare l'ostacolo.
- $d_{reach}$
	È la distanza da un punto dell'ostacolo (percepito dai sensori) o del robot (se non si sono più ostacoli percepiti) al gol attualmente percepito.
# Conseguenze
- Completezza
	È completo considerando un sensore perfetto. Nella pratica può non terminare.
- Bug 2
	Più il range del sensore è piccolo più il comportamento si avvicina a quello di [Bug 2](bug%20algorithms.md#Bug%202). Infatti se essa fosse 0 minimizzare la distanza euristica sarebbe equivalente a seguire la goal line.
- Continuità
	Sarebbe impossibile far valutare la distanza dell'agente dai due punti di estremo degli ostacoli in modo continuo. Infatti questa valutazione viene fatta in modo discreto ogni intervallo di tempo.