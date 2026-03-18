Nella cartella $src/main/resources$ (path di deafult, ma modificabile) di un progetto [quarkus](quarkus.md) è presente un file $application.properties$; si può anche avere un file YAML se si usa l'estensione $quarkus-config-yaml$. Questo file diventa un unico punto di accesso per tutte le configurazioni, il che è molto più semplice rispetto ad altri framework.
Un modo semplice per capire le configurazioni è vederle come delle variabili globali a livello di applicazione: ogni parte dell'applicazione può leggere da questo file e usare il valore della chiave specificata.
##### Tipi di configurazioni
- Comportamento
	Sono quelle cofigurazioni che sono necessarie a Quarkus per capire come comportarsi.
	Il nome della loro chiave inizia con $quarkus.$ come prefisso.
	Esempi sono la porta utilizzata, che si può specificare con $quakus.http.port=\#porta$.
- Applicazione
	Abbiamo lo possibilità di specificare anche delle configurazioni specifiche per l'applicazione. Il nome non può iniziare con il prefisso $quarkus.$, altirmenti si potrebbe avere un comportamento indefinito.
	In questo modo quello che facciamo è non avere dei valori hard-coded, ma questi possono essere cambiati in un solo punto ed essere usati in più parti del programma.
##### Utilizzo
Consideriamo le configurazioni specifiche per l'applicazione.
Queste vengono utilizzate tramite l'injection: si definisce una variabile e la si annota con $@ConfigProperty(name="configurationName")$.
Il tipo viene inferito in automatico; se ci sono degli errori di tipo vengono segnalati e l'applicazione non viene buildata.
##### Profili
Può succedere che durante il ciclo di vita dell'applicazione ci possano servire valori di configurazione diversi. Ad esempio possiamo voler usare database diversi durante la fase di testing e nella fase di produzione.
Per permettere Quarkus ammette dei profili di configurazione. Di default ci sono tre profili, prod, dev e test; possiamo però definirne anche altri.
Per specificare valori diversi ci sono due possibilità:
- All'interno del file $application.preperies$ si usa la notazione $\%profilo.nome=valore$. In questo caso quando siamo in un dato profilo Quarkus darà priorità prima a le configurazioni relative a quel profilo e poi alle altre.
- Usare file diverso. Nella cartella $src/main/resources$ si definiscono tanti file quanti sono i profili. La notazione è $application-profilo.properties$.
##### Overrinding
Quando compiliamo l'applicazione ciò che è scritto nel file $application.properties$ finisce nel jar e quindi non è più modificabile. Ci sono però degli ambienti (e.g., [kubernetes](microservizi/kubernetes/kubernetes.md)) che richiedono una configurazione dinamica, cioè di poter cambaire i valori presenti in questo file.
Questo è possibile grazie al fatto che le configurazioni vengono lette secondo una precisa priorità, se hanno lo stesso nome. In ordine:
- System property
	Il valore è quello dei parametri specificati da temrinale.
- Variabili d'ambiente
	Sono quelle specificate nel sistema operativo o ne container. Devono essere scritte in maiuscolo e i punti $.$ nella chiave sono sostiutiti da $\_$.
- $application.property$
	Ha la priorità minore.