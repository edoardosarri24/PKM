È un [pattern architetturale](software%20engineering/pattern/Pattern.md) nato per gestire quelle situazioni dove le classi hanno molti attributi complessi.
Un caso d'uso molto frequente è quando si ha un osservazione legata a più misure.
![Observation and measurement](Observation%20and%20measurement.png)
- Observation
	Un'osservazione è un osservazione. Questa può essere di due tipi:
	- CategoricalObservation
		È un'osservazione qualitativa. È descritta dall'osservazione stessa (CategoryObservation) (es: high fever); l'osservazione sarà relativa a un fenomeno (Phenomenon) (es: high); il fenomeno sarà legato a un certo tipo (PhenomenonType) (es: fever).
	- Measurement
		È un'osservazione quantitativa. È descritta da una quantità (Quantity) e dal fenomeno che è stato osservato (PhenomenonType); la quantità sarà descritta da un valore, che è un semplice campo, e da un'unità (Unit) che sarà una classe perché complessa e ci possono essere più tipi.
- All'oservazione possono essere legati altri parametri, come ad esempio la persona, il TimeRecordo o il Protocollo (come è avvenuta l'osservazione).
# Osservazioni
Questo tipo di pattern è utile quando siamo alla ricerca di modularità, cioè di poter cambiare le cose (es: tipi e casi di misure). Non è ottimo però per quanto riguarda le performance, in quanto mappare questo in un DB non è molto comodo.