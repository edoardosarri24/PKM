È la specifica [MicroProfile](MicroProfile.md) che permette di consumare servizi web che hanno API [REST](REST.md) direttamente dalla propria applicazione.
Non si segue un approccio imperativo ma [dichiarativa](Tipi%20di%20linguaggi.md#Dichiarativo).
# Funzionamento
L'idea è la seguente:
- Si crea un'interfaccia Java che funga da client per il servizio (server) che si vuole chiamare.
- Si annota l'interfaccia con $@RegisterRestClient$. In questo modo stiamo dicendo all'implementazione (e.g., [quarkus](quarkus.md)) di trasformare questa interfaccia in un client HTTP. L'url a cui contattare il servizio si può passare come parametro o specificare nel file $application.properties$.
- Si aggiunge all'interfaccia un metodo per ogni endpoint esposto dal server, annotandoli secondo la specifica [JAX-RS](JAX-RS.md).
# Injection
L'interfaccia creata deve essere [iniettata](injection.md) nella classe che poi la usera.
Questo viene fatto tramite la classica annotazione $@Inject$ del [CDI](Context%20and%20Depencency%20Injection%20(CDI).md) o tramite l'annotazione $@RestClient$ di MicroProfile.