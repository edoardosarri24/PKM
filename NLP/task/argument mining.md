Quando si parla di argument nell'[NLP](NLP.md) ci riferiamo all'argomentazione, la pratica del parlare e discutere. Un'argomento è definito da un claim, delle ipotesi a supporto e da una tesi finale.
Per argoument mining si intende l'estrazione di argomenti da testo non strutturato, come trovare articoli che sono correlati a una tesi o a un claim.
# Tipi
Ci sono due tipi di argomenti su cui possiamo fare mining:
- Astratti
	Gli argomenti e le loro relazioni sono rappresentate in un grafo, dove gli argomenti sono i nodi e le relazioni gli archi. Ci interessa studiare le relazioni e le proprietà che questo grafo definisce; non ci interessa scendere nel merito dell'argomento.
- Strutturati
	Si scende nel dettaglio di un argomento preciso: abbiamo un claim e delle ipotesi a supporto.
# Soluzione
Per risolvere questi tipi di problemi servono tecniche di addestramento supervisionato di modelli di [LLM](LLM.md).
Per risolvere problemi di addestramento supervisionato dobbiamo avere dataset, più o meno grandi a seconda se l'addestramento è fatto from skretch oppure un fine tuning, [labeling](labeling.md) specifici per il dominio.