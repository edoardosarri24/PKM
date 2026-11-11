## RECORD
Quando si generano entità che hanno campi privati, $getter$, $equals$, $hashcode$, $tostring$ il codice è sempre lo stesso. Tutto quel codice con i record è sotto inteso, nel senso che non va scritto.
Si creano usando la keyword $record$ al posto di class. Dopo il nome della classe si deve mettere tipo e nome dei campi.
- Estendono implicitamente $java.lang.Record$; non si può quindi usare con l'inheritance.
- Possono implementare interfacce.
- Possono definire metodi di istanza.
- Sono implicitamente $final$.
## SEALED
Serve per dire quali tipi possono estendere una classe (Prima erano o $final$ o infiniti sottotipi)
- Si usa $sealed$ prima di $class$ o $interface$. Dopo il nome si usa $permits$ e i tipi permessi.