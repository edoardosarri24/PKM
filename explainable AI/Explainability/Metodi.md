I metodi di [Explainability](Explainability.md) si suddividono di solito in tre famiglie. Questa tassonomia non è una partizione vera e propria, ma ci possono essere anche delle sovrapposizioni.
- [explainability of attention](explainability%20of%20attention.md)
- [Explainability from plots](Explainability%20from%20plots.md)
- [concept-base model](concept-base%20model.md)
##### Basi
Una tecnica di explainability si basa su vari aspetti:
- Il tipo di problema.
	Ad esempio se è regressione o classificazione.
- Il tipo di explanator che vogliamo. I posibili explanator di solito sono:
	- [Decision tree](Decision%20tree.md)
	- [Decision rules](Rule-based%20system.md)
	- Importanza delle features
	- Salience masks
	- Sensitivity analysis
		Deriva dalla statistica.
		Vuol dire studiare la variazione dell'output rispetto alla variazione dell'input: se vario di poco una features e l'output cambia molto allora quella è molto importante.
	- Partial dependency plot
		Sono dei plot che mi mostrano come cambia l'uscita in funzione di una o più variabili.
	- Prototipi
		Spiegano la mia decisione rispetto a un qualche esempio già esistente. Ad esempio [ProtoPNet](ProtoPNet.md).
	- Activation maximization
		Si osservano quali percorsi di una rete neurale si attivano di più in corrispondenza dell'input.
- Il tipo di black box
	In questo contesto ci sono due tipi di tecniche:
	- Agnostiche (es: [LIME](LIME.md), [SHAP](SHAP.md)) se non dipende dal modello usato.
	- Specifiche se dipendono dal modello black box.
- Il tipo di dato
	Di solito si distingue tra testo, dati multimediali o dati tabulari.
# Global model explanation
È una spiegazione globale: vogliamo trovare una spiegazione interpretabile nel suo complesso, nonostante il modello non sia spiegabile per natura.
È utile quando si vuole spiegare il fenomeno in generale, ad esempio nel campo della fisica.
- [Trepan](Trepan.md)
- [Palm](Palm.md)
##### Formalmente
Data una blackbox $b$ e un dataset $X$, il model explaination problem consiste nel trovare una spiegazione $E$ che appartiene a un dominio $\mathcal{E}$ (cioè $E\in\mathcal{E}$ ) tramite un global interpretable predictor $C_g=f(b,X)$. Una spiegazione $E$ si ottiene da $C_g$ se $E=\mathcal{E}_g(C_g,X)$ per qualche logica $\mathcal{E}_g$.
- Il predittore $C_g$ è in funzione dei dati e della blackbox.
	Possiamo osservare che trovare un modello interpretabile che spiega globalmente cosa fa una blackbox addestrata su un dataset non è equivalente ad addestrare un modello interpretabile su quel dataset. Questo perché il predittore dipende da $b$, e quindi non dipende solo dal dataset $X$. Inoltre ache se così fosse può succedere di non avere i dati di addestramento a disposizione: ad esempio se non esistono proprio più o se siamo degli utenti che possono solo fare query.
- La spiegazione $E$, una volta che ho il predittore $C_g$ e i dati $X$, viene trovato tramite un qualche meccanismo logico. Questo vuol dire che $\mathcal{E}_g$ estrae le regole che spiegano la black box.
##### Osservazione
Si tratta del metodo più complesso e solitamente ci si riduce sempre alla stessa idea generale:
- Si fanno delle query alla black box per ottenere un dataset.
- Si addestra un predittore a partire dal dateset ottenuto.
- Si valuta se il predittore addestrato ha un'alta fidelity con la blackbox, cioè se $C_g\approx b$. Vuol dire che le decisioni del predittore devono essere il più possibile allineate con quelle della black box.
##### Limiti
- Trade off
	Se vogliamo un modello interpretabile (es: albero) che si deve adattare molto alla black box, allora questo rischia di diventare troppo complesso e quindi poco interpretabile. Se invece costruiamo un modello interpretabile molto facile da capire questo rischia di non essere fedele alla black box. C'è sempre un trade off tra questi due concetti.
- Dati di training
	L'addestramento del modello interpretabile dipende molto dai dati che usiamo. Non è sempre facile scegliere il numero e quali dati usare per il training.
# Outcam explaination
È una spiegazione locale: vogliamo spiegare un singolo dato input e non di tutta la black box.
È la tecnica più utile e usata in pratica. Ad esempio se un mutuo viene rifiutato ci interessa capire il perché di quel singolo evento, e non sapere come funziona il modello.
##### Formalmente
Data una blackbox $b$ e una singola istanza $x$, l'outcame explaination problem consiste nel trovare una spiegazione $e$ che appartiene a un dominio $\mathcal{E}$ (cioè $e\in\mathcal{E}$) tramite un local interpretable predictor $C_l=f(b,x)$. Una spiegazione $E$ si ottiene da $C_l$ se $e=\mathcal{E}_l(C_l,x)$ per qualche logica $\mathcal{E}_l$.
- Se voglio spiegare più istanze mi serve trovare tre $C_l$ diversi.
##### Tecniche
Ci sono più famiglie di tecniche che ricadono nell'outcame explaination.
- Local surrogate models
	Si cerca un modello interpretabile nel vicinato di quella istanza: mi costruisco un modello interpretabile che valga solo in un neighborhead di quella istanza.
	- [LIME](LIME.md)
- Features perturbation
	Si osserva cosa succede se si perturba (fino a rimuovere) una o più features: se l'output cambia molto allora quella features è importante, altrimenti il suo contributo all'output è limitato.
	- [Incremental feature deletion](Incremental%20feature%20deletion.md)
	- [Prediction Difference](Prediction%20Difference%20(Pred%20Dif).md)
	- [SHAP](SHAP.md)
- Saliency masks
	Si vuole mostrare (es: in modo visivo) quale porzione dell'input è stato più importante per produrre il risultato. Ad esempio si potrebbero mostrare dei pixel in un'immagine o quali parole in una frase.
	- [Gradient-base explainability](Gradient-base%20explainability.md)
# Model inspection
È una spiegazione ibrida che ha delle somiglianze con entrambi i metodi di cui sopra.
Il metodo restituisce delle caratteriste generali del modello (per questo globale); si usano spesso le stesse tecniche usate per le outcame explaination (per questo locale).
Esempi sono l'osservazione delle features per capire quali sono più importanti o se alcune sono correlate.
##### Formalmente
Data una blackbox $b$ e un dataset $X$, il model inspection problem consiste nel fornire una rappresentazione (es: testuale o grafica) $r=f(b,X)$ di qualche proprietà di $b$.
##### Tecniche
- [SHAP](SHAP.md)
- [Permutation Features Importance](Permutation%20Features%20Importance.md)