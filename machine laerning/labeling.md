Quando si lavora con l'[Apprendimento supervisionato](Apprendimento%20supervisionato.md) ci serve un dataset annotato.
Annotare dataset utili per questo tipo, come per l'annotazione di dataset utili per l'NLP (e.g., [anonymization](anonymization.md#Tagging)), può essere molto difficile anche per un umano: ci sono compiti che sono poco oggettivi e molto soggettivi.
# Inter-Annotation Agreement
L'IAA è una misura di accordo sulla qualità delle etichette per capire quanto possiamo fidarci di tali annotazioni.
##### Cohen’s kappa
Una misura è il Cohen’s kappa. Essa richiede l'annotazione di un dataset da parte di più annotatori.
È definita come $k=\tfrac{p_o-p_e}{1-p_e}$, dove
- $p_o$ è l'accordo osservato, cioè la percentuale di volte in cui gli annotatori hanno data la stessa etichetta.
- $p_e$ è l'accordo per caso, cioè la percentuale di volte in cui due etichette sono uguali se date a caso.
- Risultato
	- Se $k=1$ allora abbiamo l'accordo perfetto.
	- Se $k=0$ allora l'accordo è dovuto al caso.
	- Se $k\in(0,1)$ è il range in cui si può avere accordo.
# Tool
Per mitigare le difficoltà delle annotazioni ci sono degli esperti che hanno creato delle linee guida seguire, con definizioni che possiamo seguire.
Ci sono dei tool che ci aiutano, come ad esempio Brat, INCEpTION, Gloss.