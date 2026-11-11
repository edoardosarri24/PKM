Nella Inductive Logic Programming (ILP) vogliamo unire le tecniche di machine learning con la [programmazione logica](Programmazione%20logica.md). È una tecnica usata nell'ambito dell'[interpretability](Interpretability.md): il conseguente di una regola scoperta può essere facilmente spiegato dall'antecedente.
Tramite un [ILP system](ILP%20system.md) si vuole cercare di scoprire delle [regole (clausole)](Programmazione%20logica.md#Sintassi) logiche (ipotesi $H$) a partire da una conoscenza di base, in modo che queste regole spieghino degli esempi positivi e non spieghino degli esempi negativi.
##### Vantaggi
- Ci sono delle situazioni dove non è possibile utilizzare il classico apprendimento supervisionato: spiegare una situazione reale tramite il classico approccio alle features, porterebbe a far esplodere il numero di features.
- Abbiamo pochi esempi: le tecniche di ML classiche permettono di imparare qualcosa se abbiamo molti dati.
##### Esempio obiettivo
Un esempio sono i [treni di Michalski](Inductive%20Logic%20Programming%20(ILP).md#Treni%20di%20Michalski).
# Learning
L'obiettivo nel learning è imparare delle regole (clausole), cioè una teoria logica che spieghi il concetto target a partire da una conoscenza di base. Su queste regole imparate poi posso anche fare inferenza.
In generale dati $B$,$E^+$, $E^-$ voglio quindi trovare $H\in\mathbb{H}$ (dove $H$ è la teoria e $\mathbb{H}$ è la spazio di ipotesi, cioè l'insieme delle possibili teorie) tale che:
- $H$ completa
	Si indica con $\forall e\in E^+, H\cup B\vDash e$; vuol dire che la teoria copre tutti gli esempi positivi.
- $H$ compatta
	Si indica con $\forall e\in E^-, H\cup B\not\vDash e$; vuol dire che la teoria non copre nessun esempio negativo.
##### Learning from entailtment (LFE)
È il metodo più usato. Si basa su i seguenti aspetti:
- Knowledge base $B$
	È qualcosa di dato che si conosce e solitamente $B$ è composta da un insieme di predicati ground (e.g., $healty(alice)$).
	Se facciamo una relazione con l'apprendimento supervisionato allora questi sono i dati nel train set; nel contesto della logica invece queste compongono l'antecedente delle regole (clausole) che dobbiamo scoprire.
- Positive e negative samples $E^+$ e $E^-$
	$E^+$ è l'insieme dei fatti ground che vogliamo derivare da $B$; $E^-$ è l'insieme dei fatti ground che non vogliamo derivare da $B$.
	Si usa un insieme negativo e uno positivo e non un solo insieme $E$ composto da istanze positive e negative perché negare un predicato va contro la CWA.
	Se facciamo una relazione con l'apprendimento supervisionato allora questi sono i target del train set; nel contesto della logica invece sono ciò che costituiscono il conseguente in una regola scoperta grazie a $B$ (e.g., in relazione all'esempio sopra $happy(alice)$, vuol dire che il sistema dovrà trovare una regola $happy(X)\vdash healty(X)$).
##### Learning from interpretation (LFI)
È il metodo meno usato e più complesso. Si basa su i seguenti aspetti:
- Knowledge base $B$
	È lo stesso concetto visto per la LFE.
- Positive e negative samples $E^+$ e $E^-$
	Rispetto alla LFE, che contene una sola interpretazione, in questo contesto $E^+$ e $E^-$ possono contenere più interpretazioni.
	Questo è comodo quando dobbiamo modellare un problema in cui ogni interpretazione contiene fatti che devono valere (o non valere se stanno in $E^-$), ma abbiamo più interpretazioni indipendenti l'una dell'altra.
	Ad esempio se lavoriamo con documenti potrebbero valere certe cose per dei documenti e altre per altri, e non vogliamo mischiare queste informazioni.
	Dovremmo in questo caso scoprire una teoria che soddisfa tutte le interpretazioni in $E^+$ e non soddisfa tutte le interpretazioni in $E^-$.
##### Leaning From Failure (LFF)
Si basa sul LFE. Assume un meta linguaggio $\mathcal{L}$ che descrive uno spazio di ipotesi; nella pratica $\mathcal{L}$ è un insieme di vincoli usati per restringere lo spazio delle ipotesi $\mathbb{H}$ in cui cercare le regole.
- input
	L'input nell'LFF è una tupla $(E^+, E−, B, \mathbb{H}, C)$, dove, oltre a quanto detto per LFE, $C$ rappresenta sulle ipotesi ed è espresso tramite il meta linguaggio $\mathcal{L}$.
	Chiamiamo $\mathbb{H}_C$ il sottoinsieme dello spazio di ipotesi $\mathbb{H}$ che soddisfa $C$.
- soluzione
	Dato l'input $(E^+, E−, B, \mathbb{H}, C)$, una soluzione LFF è un'ipotesi $H\in\mathbb{H}_C$ tale che $H$ è completa e compatta.
	Siccome ci sono tante soluzioni (o teorie) $H$, associamo a ognuna di queste un costo e ritorniamo quella ottima secondo tale funzione di costo.
- fallimento
	Un fallimento è una soluzione che non soddisfa i vincoli $C$. Nell'LFF i fallimenti servono quindi per ridurre lo spazio in cui cercare le soluzioni.