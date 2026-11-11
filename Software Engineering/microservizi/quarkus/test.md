Nell'organizzazione del progetto [quarkus](quarkus.md), considerando che probabilmente usiamo Maven, si trovano in $src/test/java$.
I test sono in Quarkus sono basati su test JUnit.
# Tipologie
Ci sono due tipi di test contrassegnati con due annotazioni diverse:
- $@QuarkusTest$
	Sono i classici JUnit test. I test e il codice vengono eseguiti sulla stessa istanza della JVM. Per questo motivo i test hanno accesso diretto al codice che devono chiamare.
- $@QuarkusIntegrationTest$
	I test e il codice vengono testati in ambienti diversi: i test all'interno di JVM; il codice viene buildato ed eseguito. I test non possono accedere direttamente al codice, ma lo faranno tramite internet.
	È il modo per eseguire i test veri e proprio del prodotto finito; è inoltre l'unico modo per testare l'immagine nativa o il [container](container.md).
# Continuos testing
Nella [dev mode](development%20mode.md), oltre a ricaricare l'applicazione quando la modifichiamo, abbiamo la possibilità di eseguire i test dopo ogni modifica al codice o a una parte dell'applicazione.
Attivando la modalità continuous testing con il comando $\texttt{r}$ nella dev mode, quando facciamo una modifica all'applicazione, codice o configurazione che sia, i testi vengono rieseguiti.
##### JVM
I test vengono eseguiti nella stessa JVM dell'applicazione, ma hanno un class loader di verso e quindi sono isolati dal resto dell'applicazione. Per questo motivo non è possibile che lo stato dell'applicazione, che è in esecuzione visto che siamo nella dev mode, possa essere invalidato dai test.
Inoltre anche se i test ci mettono molte tempo a eseguire non ci sono problemi, l'applicazione sarà raggiunbile da browser.
##### Dev UI
Se ci serve analizzare i log dei test abbiamo due possibilità: o usare il comando $\texttt{o}$ dal terminale della dev mode o usare la dev UI.
Alla dev UI si accede con il comando $\texttt{d}$ dal terminale della dev mode. Qui è molto facile vedere i log e gestire i problemi.