La Fault Tree Analysis (FTA) è una tecnica di [analisi del rischio](analisi%20del%20rischio.md) deduttiva (parte dalle avarie) e previsionale descritta nella norma 61025.
L'obiettivo è partire da un guasto (i.e., una non conformità (NC)) possibilmente non ancora avvenuto, (i.e., la manifestazioni di una NC) e cercare le sue cause; rispetto alle altre tecniche, che suppongono guasti indipendenti, in questo si cercano le combinazioni di cause che hanno portato al guasto.
# Grafica
È una tecnica grafica: si individua il top event tramite [RBD](reliability%20block%20diagram.md) e [FMECA](FMECA.md) e si scompone usando determinate porte che mettono in relazione componenti.
##### Porte
- AND: gli input si devono verificare tutti.
- OR: si deve verificare un solo input.
- AND prioritario: l'ouput si verifica se gli input accadono e in un dato ordine.
- OR prioritario: l'output si verifica se uno degli input accade condizionatamente a uno stato del sistema.
##### Componenti
La composizione utilizza i sequenti componenti:
- Cerchi: sono i basic event, eventi che non posso essere ulteriormente scomposti.
- Rombi: sono eventi che non possono essere scomposti per mancanza di informazioni.
- Rettangolo: è un evento intermedio, un evento che non è la causa primaria del guasto ma che lo ha provocato.
# Probabilità
Si deve definire la probabilità di inaffidabilità (si ragiona in termini di inaffidabilità e non di affidabilità) dei basic event e poi combinarle tramite le porte logiche:
- La probabilità dell'output di un OR è la somma delle probabilità degli input.
- La probabilità dell'output di un AND è il prodotto delle probabilità degli input.
##### Categorie
I basic event (i.e., cerchi) si dividono i 5 categorie:
- Non riparabili
	Siccome l'unità non è riparabile allora si deve conoscere bene il suo tasso di guasto $\lambda$. Dato il tasso di guasto la probabilità di inaffidabilità è data da $q_{i}(t)=1-e^{-\lambda_{i}t}\approx\lambda_{i}t$, dove l'approssimazione è data dallo sviluppo di Taylor.
- Riparabili
	Se l'unità è riparabile allora $q_{i}(t)=\lambda_{i}\cdot MTTR_{i}$.
- Testata periodicamente
	Si suppone che dopo il test l'unità sia come nuova, ma ovviamente avrà un po' di usura. La probabilità di inaffidabilità è data da $q_{i}(t)\approx \tfrac{\lambda_{i}\tau_{i}}{2}$, dove $\tau_{i}$ è l'intervallo di test.
- Frequenza
- On demand
	(lo rivedremo parlando di Functional safety). La probabilità di guasto non è più l’inaffidabilità ma si parla di ‘guasto su richiesta’ (PFD, Probability of Failure on Demand).
# Utilizzi
- Permette di mappare molto bene un [reliability block diagram](reliability%20block%20diagram.md) e usarlo per l'analisi del rischio:
	- AND: rappresenta bene il fallimento di un parallelo dell'RBD.
	- OR: rappresenta bene il fallimento di una serie dell'RBD.
	- AND/OR: la combinazione rappresenta una KooN.
- Si usa in combinazione con altre tecniche. Solitamente si usa in fase avanzata di progettazione (richiede ad esempio il RBD e FMECA).
 
# Conseguenze
- I cambiamenti del sistema richiedono variazioni piccole.
- Non ha una struttura ad albero e quindi è meno restrittiva.
- Richiede più conoscenze e quindi serve personale competente.