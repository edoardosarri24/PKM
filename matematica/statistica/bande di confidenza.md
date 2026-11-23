Quando eseguiamo degli esperimenti, abbiamo in mano solo dati e non la vera distribuzione da cui questi dati derivano. Per trattare in modo statistico i dati e trarre precise conferme (e.g., performance analysis) ci serve trattarli come distribuzioni.
Le bande di confidenza sono un modo per capire quanto una distribuzione campionaria (i.e., la [CDF](random%20variable.md#CDF) ottenuta dai dati) sia affidabile; possiamo quindi verificare, con una fissata confidenza $1-\alpha$, quanto è accurata la distribuzione ottenuta e se è necessario aumentare i campioni dell'esperimento.
Quello che vogliamo trovare è un intervallo che contenga la CDF campionaria.
# Pointwise band
Le bande di confidenza sono definite analizzando ogni puno della CDF in modo isolato.
Sono tecniche valide quando ci interessa capire l'affidabilità di un dato valore (e.g., quale è la probabilità che il mio esperimento restituisca un valore minore di $x$), oppure quando si vuole analizzare i percentili. In questi contesti sono meno conservative e più affidabili.
##### Approssimazione normale
La maggior parte delle tecniche utilizza la [distribuzione binomiale](distribuzione%20binomiale.md) per determinare gli intervallie di confidenza di un punto della CDF. La più citata in letteratura è l'approssimazione normale.
Questa approssima la distribuzione a una gaussiana (normale, da cui il nome); per una binomiale questo vuol dire [supporre](distribuzione%20binomiale.md#Osservazioni) che il numero di esperimenti tenda a infinito o che la probabilità sia $p=0.5$. Questo fa si che questa tecnica sia buona quando il numero di dati è molto elevato.
I problemi principali che si possono incontrare sono:
- Copertura a salti
	Se chiediamo un grado di confidenza di $1-\alpha\%$ può succedere che per alcuni punti questo sia rispettato, per altri più alto e per altri ancora più basso.
- Asimmetria
	Per eventi che hanno una probabilità molto bassa o molto alta la distribuzione diventa molto [asimmetrica](distribuzione%20binomiale.md#Osservazioni), ma i metodi standard si assumono che essa sia una campana simmetrica.
- Numero di successi limite
	Se osserviamo valore di k=0 o K=N allora otteniamo i metodi standard portano a divisioni per zero e altri problemi.
##### Clopper-Pearson
Si basa sulla [distribuzione binomiale](distribuzione%20binomiale.md) e risolve i problemi base che si hanno con l'[approssimazione normale](#Approssimazione%20normale).
L'idea è cercare tutti i valori di $p$ per cui le formule sotto funzionano. Questo porta a: avere costo computazionale alto, per cui si usa la relazione tra distribuzione binomiale e distribuzione beta; essere conservativo; garantire una copertura completa.
Gli intervalli sono definiti da due valori:
- $p_{inf}$
	È il valore di $p$ della binomiale per cui la probabilità di osservare almeno $k$ successi è esattamente $\alpha/2$, cioè $\displaystyle\sum_{i=k}^n\binom{n}{i}p_{inf}^i(1−p_{inf})^{n−i}=\tfrac{\alpha}{2}$. Usando la distribuzione beta si ha poi $B(\alpha/2, X, n-X+1)$.
- $p_{sup}$
	È il valore di $p$ della binomiale per cui la probabilità di osservare al più $k$ successi è esattamente $\alpha/2$, cioè $\displaystyle\sum_{i=0}^k\binom{n}{i}p_{inf}^i(1−p_{sup})^{n−i}=\tfrac{\alpha}{2}$. Usando la distribuzione beta si ha poi $B(1-\alpha/2, X+1, n-X)$.
# Simultaneus band
Fornisco delle bande probabilistiche di confidenza della CDF della distribuzione da cui derivano i dati e garantiscono che la vera curva della CDF resti all'interno delle bande con confidenza $1-\alpha$. Permettono quindi di ottenere delle garanzie sull'intera popolazione senza sapere nulla della vera distribuzione.
##### Dvoretzky–Kiefer–Wolfowitz (DKW)
È una tecnica che definsice delle bande di ampiezza $\epsilon$ (dove $\epsilon=\sqrt{\tfrac{ln\tfrac{2}{\alpha}}{2n}}$) parallele alla CDF.
Siccome ha un'ampiezza costante tende a essere troppo conservativa; viene usato quando si lavora in contesti di sicurezza.
La matematica ci dice che la vera CDF $F(x)$ è in $L(x)\le F(x)\le U(x)$, dove $U(x)=\min(F_n​(x)+\epsilon,1)$ e $L(x)=\min(F_n​(x)-\epsilon,0)$, con $F_n(x)$ che è la CDF campionaria.
# Bootstrapping
Il [bootstrapping](bootstrapping.md) permette di ottenere delle bande di confidenza in due modi; le bande ottenute sono meno conservative di quelle derivate dai metodi Pearson-Clopper e DKW.
- Pointwise
	Se abbiamo intervallo di confidenza del 95\% allora prendiamo il 2.5° percentile e il 97.5° percentile dei valori assunti da tutte le $F^*(x)$ ottenute. Avremo in questo modo una banda di confidenza che varia in funzione di $x$.
- Simultaneus
	Si condiera il valore massimo di $|F(x)-F^*(x)|$ per ogni $x$. Si prendono poi i percentili di questi valori massimi. Avremo in questo modo una banda di confidenza di ampiezza costante.