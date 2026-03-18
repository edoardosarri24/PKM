È un [processo](system%20design.md) per lo sviluppo di sw.
- Le attività di sviluppo devono essere seguite molto rigidamente: si passa alla fase successiva solo dopo aver termina quella attuale.
- Ogni fase deve essere ben documentata e aggiornata in caso di cambiamenti; se ci si accorge di un errore in una delle fasi precedenti si torna ad essa e si inizia nuovamente tutto da capo.
- È usano in ambiti dove si vuole massimizzare la safety (ferrovie e aerei) o si hanno progetti molto grandi. In questi casi si riesce a strutturare il lavoro correttamente e si ha una strada da seguire.
##### Prototipo
È una versione iniziale e grezza per dimostrare al cliente quello che andremo a fare.
Il prototipo deve essere sviluppato velocemente e a basso costo: si può usare un linguaggio provvisorio e magari di altissimo livello; si possono non avere test e includere delle inefficienze; si può anche pensare di buttar via il prototipo una volta che il cliente l'ha accettato.
- Aiuta il cliente a definire i requisiti, che nel plan-driven process sono fondamentali e devono essere stabili e sicuri.
- Back-to-back tests
	I test sul prototipo che il cliente ha accettato devono funzionare anche sul prodotto finale.
##### Conseguenze
- Non è uno sviluppo SW flessibile in fase di realizzazione. I requisiti devono essere ben fissati e non dovrebbero cambiare.
- Al cliente arriva il prodotto solo una volta che è totalmente terminato.
- I costi, soprattutto in caso di errori, possono essere più elevati rispetto ad altri tipi di processi.
- Una volta terminata una fase si ha (O si dovrebbe) avere la certezza di non doverci tornare e che essa sia corretta.
# Modelli
### Waterfall
![Waterflow](Waterflow.png)
È il primo modello si sviluppo SW ed è noto per la sua estrema rigidità.
### V-Model
![[V-Model.jpg]]
È successivo al waterfall e permette di ridurre l'introduzione di errori.
La sua caratteristica è introdurre una progettazione di test ad ogni fase; il test può anche non essere eseguito e validato subito ma il doverlo comunque pensare ci spinge a chiederci se stiamo svolgendo le cose nel modo corretto (Se non si riesce a pensare a come i test saranno fatti allora sicuramente stiamo sbagliando approccio).
Una volta raggiunta la fine della catena di attività la si risale validando i test precedentemente implementati.