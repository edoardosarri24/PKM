Nell'apprendimento supervisionato ci sono delle metriche classiche e delle metriche più specializzate.
# Classificazione
Sono metriche che ormai sono un standard de facto: in un qualche modo si usano sempre per valutare un modello.
##### Accuracy
È la probabilità che un modello faccia una previsione corretta: $\text{Accuracy} = \frac{\text{True Positives} + \text{True Negatives}}{\text{Total Samples}}$.
- Indica quanto spesso il modello classifica correttamente le istanze.
- Si usa quando un dataset è bilanciato. Non è una buona misura per dataset non bilanciati.
##### Precision
Misura il rapporto tra le istanze correttamente classificate come positive sul numero di istanze classificate positive: $\text{Precision} = \frac{\text{True Positives}}{\text{True Positives} + \text{False Positives}}$.
- Indica la fiducia che possiamo dare al modello quando ci dice che un elemento è positivo:
	- Se abbiamo una precisione alta allora per quelle istanze che il modello classifica come positive possiamo essere sicuri che sia giusto.
	- Magari però ci sono altri elementi che dovevano essere classificati come positivi che il modello ha clclassificato negativi perché non era molto sicuro.
- Ad esempio non vogliamo che una mail buona sia classificata come spam, ma ci può andare bene se una spam è classificata come buona.
##### Recall
Misura il rapporto tra le istanze correttamente classificate come positive sul numero di istanze effettivamente positive: $\text{Recall} = \frac{\text{True Positives}}{\text{True Positives} + \text{False Negatives}}$.
- Indica quanto bene il modello riesce a trovare le istanze positive:
	- Se abbiamo una recall alta allora possiamo essere sicuri che il positivi siano stati classificati correttamente.
	- Però potrebbero essere classificati come positivi anche alcuni negativi per cui il modello non era sicuro che fossero negativi.
- Ad esempio non vogliamo che un paziente malato sia classificato come sano.
##### F-measure
È solitamente una media armonica tra precisione e recall: $\text{F1} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$.
- Indica una metrica che è un compromesso tra le due.
- È utile quando abbiamo dataset sbilanciati, dove devono essere considerati sia i false positive che i false negative.
##### ROC curve
Mette in relazione il False Positive Rate (FPR) con True Positive Rate (TPR), cioè la sensibilità.
Si vuole ottenere una AUC (Area Under Curve) grande, cioè una quasi funzione a scalino, dove in 0 è molto alto.
Si usa nella classificazione sbilanciata.
# Regressione
In questo campo è più complesso perché molto dipende dalla natura del problema. Spesso per problemi non banali metriche custom sono la cosa migliore.
##### Le classiche
Tra le più usate abbiamo la [MAE](loss%20function.md#Mean%20Absolute%20Error%20(MAE)) e la [MSE](loss%20function.md#Mean%20Squared%20Erroe%20(MSE)) (o la sua versione sotto radice). Spesso ha senso confrontarle tra loro (e.g. tramite il rapport $\frac{RMSE}{MAE}$):
- Se $MAE\approx RMSE$ allora vuol dire che gli errori sono distributi uniformemente tra tutti i campioni.
- Se $MAE < RMSE$ allora ci sono momenti in cui il modello predice bene, ma ci sono dati per cui fa più fatica. Questo è dovuto alla qudraticità della RMSE.
##### Coefficiente di determinazione
È detto anche R-quadro, da cui il nome $R^2$ e si definisce come $R^2 = 1 - \frac{\sum_{i=1}^{n} (y_i - \hat{y}_i)^2}{\sum_{i=1}^{n} (y_i - \bar{y})^2}\in(-\infty,1]$, dove: $N$ è il numero di campioni; $y_{i}$ è i target vero; $\hat{y_{i}}$ è la predizione; $\bar{y}$ è la media su tutti i campioni $N$.
- Risultato
	- $R^2=1$: Il nostro modello predice tutti i campioni perfettamente.
	- $R^2=0$: Il nostro modello funziona come un predittore banale che calcola solo la media dei target.
	- $R^2<0$: Il nostro modello fa peggio di un predittore banale che calcola solo la media dei target.
- Adimensioale
	La metrica è di fatto una MSE normalizzata per la varianza dei campioni. Questo permette che non abbia dimensione e che possa essere usata per qualunque misure fisica.
- Globale vs locale
	Si può parlare di $R^2$ locale o $R^2$ globale.
	- È locale misura quanto è bravo il modello a prevedere una sola variabile (e.g., una features o un asse nei sensori).
	- È invece globale se si misura quanto è bravo il modello su tutte le features o variabili.
- Dataset vs sample
	Può essere calcolata su un batch grande ma anche sul singolo dataset.
- Osservazioni
	- Se usata con la normalizzazione [Z-Score](machine%20laerning/normalizzazione.md#Z-Score) allora $\bar{y}\approx 0$ e di conseguenza si ha che $R^2\approx 1-MSE(y,\hat{y})$.
	- Se stiamo analizzando un insieme di dati con varianza molto bassa allora il denominatore porta ad avere valori molto grandi anche se l'errore del modello è molto piccolo.