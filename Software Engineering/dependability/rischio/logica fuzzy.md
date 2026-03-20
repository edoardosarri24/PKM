È un'ottimizzazione che possiamo fare durante l'[analisi del rischio](analisi%20del%20rischio.md) che permette di migliorare la tecnica a cui viene applicata.
È un'estensione della logica boolena, la quale si basa sulla sola membership function $\mu_{A}(x)=\begin{cases}1&\text{ if }x\in A\\0&\text{ if } x\not\in A\end{cases}$; essa introduce una funzione che restituisce un valore in $[0,1]$, cioè che definisce dei livelli intermedi.
È usata in tutte quelle situazioni dove ci sono dei confini più soft sull'insieme di appetenza di una variabile $x$ o abbiamo un certo grado di incertezza.
# Definizione
Un insieme fuzzy $A$ è definito da coppie: $A=\{(x,\mu_{A}(x))|x\in X,\mu_{A}(x):X\to[0,1]\}$, dove $x$ è un elemento e $\mu_{A}(x)$ è una funzione di appartenenza che restituisce un valore in $[0,1]$.
##### Funzioni di appartenenza
Ci sono più funzioni di appartenenza:
- Tringolare
	La funzione [triangolare](fuzzy%20tringalolare.png) è definita come $\mu(x)=\begin{cases}0&\text{ if }x<a,x>b\\\frac{x-a}{m-a}&\text{ if } a\le x\le m\\\frac{x-m}{b-m}&\text{ if } m\le x\le b\end{cases}$.
	Non viene fatta nessuna ipotesi sul fatto che $[a,m]=[m,b]$, cioè sul fatto che il triangolo sia isoscele. Può anche essere che il valore 1 non sia mai raggiunto, cioè che $\mu(m)\ne 1$.
- Trapezoidale
	La funzione [trapezoidale](fuzzy%20trapezoidale.png) è definita come $\mu(x)=\begin{cases}0&\text{ if }x<a,x>d\\\frac{x-a}{b-a}&\text{ if } a\le x\le b\\\frac{x-c}{d-c}&\text{ if } c\le x\le d\\1&\text{ if }b\le x\le c\end{cases}$.
	Rispetto alla triangolare il numero di elementi $x$ tali che $\mu(x)=1$ ha una cardinalità maggiore di 1.
##### Variabili linguistiche
Nella logica fuzzy le variabili sono linguistiche e non numeriche.
Una variabile linguistica rappresenta una categoria (e.g., età) e ha associati diversi valori linguistici (e.g., giovane, adulto e anziano). Ogni valore linguistico ha la sua funzione di appartenenza, che determina quanto un valore numerico appartiene a quella categoria; in pratica le funzioni di appartenenza permettono di tradurre un numero di un insieme di probabilità; il numero di queste probabilità è pari al numero di valori linguistici per quella data variabile linguistica.
# Operazioni algebriche
La logica fuzzy può essere estesa alle operazioni algebriche.
- Somma
	Si sommano le funzioni di appartenenza di $A$ e $B$ e si sottrae il loro prodotto.
- Prodotto
	Si fa il prodotto delle due funzioni di appartenenza.
- Operazioni chiuse
	Si è osservato che la logica fuzzy è chiusa rispetto alle operazioni di somma e prodotto: se sommiamo/moltiplichiamo due variabili triangolari/trapezoidali otteniamo ancora variabili triangolari/trapezoidali.
- Intersezione
	È il minimo delle due funzioni di appartenenza.
- Unione
	È il massimo delle due funzioni di appartenenza.
- Complemento
	È il complemento a 1 (i.e., $1-\mu(x)$) della funzione di appartenenza.
# Step
Gli step ella logica fuzzy sono i seguenti:
- Fuzzifier
	Traduce una variabile numerica in una variabile linguistica. L'idea è di proiettare il valore numerico su tutte le curve (i.e., sulle funzioni di appartenenza dei valori linguistici) della variabile linguistica per ottenere un insieme di probabilità.
	Per esempio da $età=22$ si ottiene $[\text{giovane}=0.3, \text{ adulto}=0.4, \text{anziano}=0.1]$.
- Inferenza
	Prende in input più variabili e utilizza gli operatori logici AND e OR (i.e., intersezione e unione) per produrre una nuova forma geometrica. Si basa sul concetto di $if (and/or) then$.
- Defuzzifier
	È il passaggio opposto e mi serve per ottenere nuovamente un valore numerico.
	Il metodo più usato è quello del centroide che restituisce il centroide della funzione di appartenenza ottenuta dopo l'inferenza.