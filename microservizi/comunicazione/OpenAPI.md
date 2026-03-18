Prima noto come Swagger, OpenAPI è un specifica standard open per descrivere API [RESTfull](REST.md) tramite un docuemnto JSON o YAML.
# Contenuto
Nel documento sono descritti endpoint disponibili, parametri richiesti, formati di input output e sicurezza.
# Utilità
Evita la documentazione verbale, può essere letto e capito sia dalle macchine sia dagli umani.
Serve sia al backend che al frontend:
- Backend
	Viene solitamente generato automaticamente. Ad esempio per Quarkus c'è la relativa [estensione](estensioni.md#OpenAPI).
- Frontend
	Utilizza la documentazione per capire che servizi il backend espone e come chiamarli.