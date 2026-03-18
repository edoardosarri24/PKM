È una semplificazione della game theory (con alcune [differenze](Decision%20e%20matching%20theory.md#Differenze)): risolve lo stesso problema ma non è così restrittiva e quindi nella pratica si hanno meno problemi.
Si tratta di un framework per instaurare relazioni mutuamente vantaggiose (entrambe le parti hanno vantaggi) tra elementi appartenenti a insiemi opposti ma della stessa cardinalità.
Ogni elemento ha una lista ordinata di elementi dell'insieme opposto; l'ordinamento rappresenta il ranking, cioè l'ordine di preferenza degli accoppiamenti.
##### Notazione
- La funzione di matching è una funzione $\mu:A\times B\to A\times B$; nell'ingegneria diventa una funzione $\mu:A\to B$.
- $a>_kb$ indica che il soggetto $k$ preferisce essere accoppiato con $a$ rispetto a essere accoppiato con $b$.
# Stable matching
Il gol principale è raggiungere uno stable matching. Un matching $\mu$ non è stable, cioè è unstable (detto anche blocked), se si verifica una delle seguenti condizioni:
- Se esiste un elemento $k$ che preferisce essere single invece che essere accoppiato con il partner assegnato. Formalmente si indica con $k>_k\mu(k)$. Questa condizione è solo teorica; nell'ingegneria la lista di preferenza di un elemento non contiene l'elemento stesso, ma solo elementi dell'insieme opposto.
- Se c'è almeno una blocking pair $(a,b)$, cioè una coppia dove $a$ preferisce $b$ a $\mu(a)$ e $b$ preferisce a $rispetto$ a $\mu(b)$. Usando la notazione di prima $b>_a\mu(a)$ e $a>_b\mu(b)$.
# Algortimo di Gale-Shapley
È un algoritmo usato in moltissime applicazioni (es: tinder, università a numero chiuso) che trova sempre uno stable matching tra due insiemi A e B della stessa cardinalità se preferenze non cambiano durante il gioco (cioè, l'esecuzione dell'algoritmo).
##### Fasi
Ci sono 4 fasi:
- Inizializzazione
	I player non sono associati e si creano le liste di preferenza.
- Proposta
	I player di A approcciano al proprio player preferito di B da cui non sono stati rifiutati.
- Accetazione/Rifiuto
	Ogni player di B valuta la/le proposte che riceve: se il player non è associato a nessun player di A allora si accoppia con quello che ha il ranking maggiore; se è già associato con un player di A ma riceve una proposta da un player con ranking maggiore allora può cambaire accopiamento.
- Iterazione
	Il processo continua finchè non tutti gli elementi non matchati.
##### Osservazioni
Come si osserva dai due punti sotto è un tool sbilanciato rispetto all'ottimalità. Questo deriva dal fatto che gli elementi di A fanno proposte seguendo la propria lista di ranking, mentre B accettano il meno peggio da cui ricevono una proposta.
- È un algoritmo ottimo per A, cioè non esiste un algoritmo che produce un matching stabile dove A in media si accoppia con elementi di B che hanno ranking maggiore.
- Non è ottimo per B. In media ogni elementi di B viene associato con un elemento di A che non ha una buona posizione nel ranking.