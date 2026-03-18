Si tratta di una soluzione per la [mappatura](mapping.md) online in sistemi multi agente in ambienti statici (in ambienti dinamici richiede modifiche) che introduce un'informazione semantica.
L'ambiente è rappresentato tramite un insieme di features come $M=\{f_1,\cdots,f_K\}$, dove le features in questo caso sono i punti salienti, cioè quelli che definiscono l'ambiente (e.g., per un bosco gli alberi).
# Algoritmo
Per eseguire la mappatura l'agente mantiene un insieme di features $M(t)=\{f_1,\cdots,f_{K(t)}\}$: nel tempo (i.e., $t\to\infty$) il numero di features $k(t)$ aumenterà per via delle rilevazioni successive.
In ogni iterazione $t$ l'agente deve seguire vari step: detection, matching, updateing e initializzation.
##### Features detection
Si devono estrarre le features tramite degli algoritmi di ML a partire dalla rilevazione $Z(t)=\{z_1\cdots,z_{D(t)}\}$. Questo per noi è una black box: servono algoritmi di image recognition.
##### Features matching (o data association)
Si devono mappare le informazioni della rilevazione $Z(t)=\{z_1,\cdots,z_D(t)\}$ con le features esistenti $M(t-1)=\{f_1,\cdots,f_K(t-1)\}$. È il compito più complesso per vari motivi: le features non sono etichettate e si devono associare solo sulla base delle rilevazioni e di quanto memorizzato; più features diverse possono essere simili; i sensori sono rumorosi.
Una soluzione può essere quella del linear assignment:
- Si ha l'insieme delle features $M(t-1)$ e la rilevazione $Z(t)$.
- Si calcola la matrice di costo $C$, dove l'elemento $C_{kd}$ rappresenta il costo (i.e., la distanza) tra la features $f_k$ e la detection $z_d$.
- Si definisce la matrice binaria $X$, dove l'elemento $X_{kd}=1$ se $f_k$ $e$ $z_d$ sono associate, $X_{kd}=0$ altrimenti.
- L'obiettivo è trovare l'assegnamento $X$ che minimizza la funzione di costo $J(X)=\displaystyle\sum_{k=1}^K\sum_{d=1}^DC_{kd}X_{kd}$, con i vicoli $\displaystyle\sum_{d=1}^DX_{kd}\le1$ (i.e., a ogni features è assegnata al più una detection) e $\displaystyle\sum_{k=1}^KX_{kd}\le1$ (i.e., ogni detection è assegnata al massimo a una features). Questo assegnamento può essere fatto con un costo cubico con l'algoritmo ungarico.
##### Features update
Si devono aggiornare quelle features $f_k$ che sono state mappate con delle nuove rilevazioni $z_d$.
Per risolvere questo problema si possono assegnare due valori a ogni features $f_k$: $r_k$ indica la probabilità dell'esistenza di $f_k$, visto che magari è un po' che non la vediamo o il sensore è impreciso; $p(f_k)$ è la distribuzione di probabilità sulla posizione della features nello spazio, dove una scelta può essere la [gaussiana](distribuzione%20gaussiana.md) $N(f_k$,$\hat{f}_k$,$\Sigma_k$) con media $\hat{f}_k$ e varianza $\Sigma_k$.
L'aggiornamento è fatto con la regola di bayes tramite $p(f_k)=\tfrac{p(z_d|f_k)p(f_k)}{p(z_d)}$.
##### Features initialization
Le rilevazioni $z_d$ che non sono state mappate e quindi non corrispondono a nessuna vecchia features devono essere inizializzate come nuove features.
# Conseguenze
- Vantaggi
	- L'ambiente può essere rappresentato in modo denso non come nel caso delle celle.
	- Le informazioni possono provenire da sensori diversi. In questo caso si useranno algoritmi di features detection diversi per ottenere delle informazioni coerenti.
- Limiti
	- La fase di features detection può essere onerosa dal punto di vista computazionale.
	- Se l'ambiente è dinamico allora servono strategie ulteriori per mapparlo con rapidità.