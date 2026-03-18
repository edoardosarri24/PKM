I [microservizi](microservizi.md) hanno bisogno di molta comunicazione tra loro per portare a termini i loro compiti.
# Tipi
La comunicazione può essere:
- Sincrona
	Chi chiede aspetta la risposta e poi prosegue.
- Asincrona
	Chi chiede riceve una notifica quando la risposta è pronta; nel frattempo può fare altre cose.
# Protocolli
- HTTP e REST
	È il metodo più usato, sia perché è semplice sia perché è il più noto.
	In [quarkus](quarkus.md) si può usare l'estensione per [Rest](estensioni.md#Rest) sia lato server che lato client.
- GraphQL
	[GraphQL](GraphQL.md) è un'alternativa a [REST](REST.md) e permette al client di specificare cosa vuole ricevere, permettendo ricevere più o meno dati in una sola richeista.
- gRPC
	È un framework ad alte performance che permette le RPC.
# Annotazioni
Si possono usare tutte le annotazioni [JAX-RS](JAX-RS.md), ma ne sono introdotte di più efficiente e fanno parte dello stack RESTEasy Reactive di Quarkus.
Lo svantaggio è che in questo moto si perde un po' di portabilità.
- $@RestPath$
	Ha lo stesso obiettivo di [@PathParam](JAX-RS.md#Metodi) di JAX-RS.
- $@RestQuery$
	Ha lo stesso obiettivo di [@QueryParam](JAX-RS.md#Metodi) di JSX-RS.