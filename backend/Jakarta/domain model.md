È il layer di [Jakarta](Jakarta.md) che serve per modellare la rappresentazione delle entità coinvolte nel processo, la loro relazione e i ruoli; permette quindi di astrarre il contesto in cui l'applicazione lavora.
# Modalità
Ci sono due modi per costruire un domain model:
- Anemico
	Contiene solo le classi che rappresentano le entità del contesto con i loro attributi.
	Le classi del domain model in questo caso rappresentano fedelmente le relazioni presenti nel data base.
- Ricco
	Le classi contengono e implementano anche le funzionalità e gli algoritmi. In questo caso è meglio parlare di domain logic.
	Oltre alle classi che rappresnetano le relazione del data base ci possono essere anche altre classi per implementare alcuni [design pattern](Design%20pattern.md).
# Tecnologie
La specifica usata all'interno del domain model è [JPA](JPA.md) con le sue annotazioni.