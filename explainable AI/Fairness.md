I [bias](Bias.md) possono portare a decisioni non fair. La fairness è l'assenza di pregiudizi verso individui o gruppi basati sul fatto di appartenere al gruppo stesso.
# Quantità
Le quantità per misurare la fairness si basano su un modello di ML che mette in relazione la probabilità $P$ di una classe positiva (siamo quindi nel caso della classificazione binaria) e un attributo protetto (o sensibile, che può essere anche un gruppo di attributi).
##### Equalized odd (quete pareggiate)
$P[\hat{Y}=1|Y=y,A=1]=P[\hat{Y}=1|Y=y,A=0]$, con $y\in\{0,1\}$.
La probabilità che un sample positivo sia classificato correttamente come positivo (TP) e la probabilità che un sample negativo sia classificato non correttamente come positivo deve essere la stessa a prescindere dalla classe di appartenenza.
Vuol dire anche che il tasso di TP e FP deve essere lo stesso per entrambi i gruppi.
##### Equal opportunity (Pari opportunità)
$P[\hat{Y}=1|Y=1,A=1]=P[\hat{Y}=1|Y=1,A=0]$.
La probabilità di essere classificati correttamente come positivi deve essere la stessa a prescindere dalla classe di appartenenza.
Vuol dire anche che il tasso di TP deve essere lo stesso per entrambi i gruppi.
##### Statistical paraty (parità statistica)
$P[\hat{Y}|A=1]=P[\hat{Y}|A=0]$.
La distribuzione dei sample rispetto alla predizione dovrebbe essere la stessa a prescindere dal gruppo di appartenenza.
##### Fairness through awareness
Un algoritmo è fair se da predizioni simili per individui simili.
Vuol dire che se consideriamo lo spazio dei samples, allora per vettori simili in questo spazio abbiamo risultati simili.
##### Fairness through unawareness
Un algoritmo è fair se gli attributi protetti non sono usati nel processo di decisione.
# Migliramenti
La fairness può essere migliorata in tre modi:
- Pre-processing
	Si lavora sui dati per rimuovere bias.
- In-processing
	Si addestra il modello per minimizzare sia la loss classica che un regolarizzatore definito da una [metrica](Fairness.md#Quantità) di fairness.
- Post-processing
	Sono tecniche adatte per identificare il problema, ma non per risolvere o mitigarlo in qualche modo.
# Librerie
Due librerie molto famose per misurare la fairness sono:
- [Fairlearn](https://fairlearn.org)
	Ottima per le metriche.
- [Fairlens](https://www.synthesized.io/fairlens)