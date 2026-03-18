Per fare il building di un'applicazione [quarkus](quarkus.md) ci sono due opzioni: la compilazione JVM o GraalVM. La scelta tra le due avviene sulla base delle loro [conseguenze](conseguenze%20di%20building.png): la JVM ci mette più ad avviarsi e occupa più memoria, ma una volta in esecuzione è più ottimizzata e quindi ha un throughput più alto rispetto alla compilazione nativa; la GraalVM ha i benefici opposti.
##### Efficienza
A prescindere dal tipo di compilazione utilizzato, quarkus è molto più efficiente per avviare l'applicazione.
Di solito infatti la compilazione Java mette insieme tutto e produce il bytecode e il resto delle operazioni è eseguito a runtime. Con Quarkus invece la fase di building è più complessa, ma in memoria viene caricato solo ciò che è necessario per eseguire: il tempo di start è molto minore e l'occupazione di memoria è molto inferiore.
# JVM
Si tratta del classico metodo per le applicazioni java: tramite il comando $\texttt{quarkus build}$ si costruisce un $jar$ che finisce nella cartella $target/quarkus-app$ e si chiama $quarkus-run.jar$. Si esegue con il comando $\texttt{java -jar}$.
# GraalVM
![GraalVM building](GraalVM%20building.png)
La compilazione tramite compilatore [GraalVM](quarkus.md#OpenJDK%20HotSpot%20and%20GraalVM) permette di compilare il codice Java direttamente in codice binario eseguibile dalla macchina, invece che nel bytecode e poi usare la JVM per interpretarlo.
Quarkus utilizza Mandrel come distribuzione di GraalVM; questo tool è in grado di generare un binario che è eseguibile solo su Linux.
Il comando da utilizzare è $\texttt{quarkus build --native}$.
# Containerization
Se non siamo su una macchina Linux o vogliamo poi distribuire il nostro codice allora dobbiamo passare dai [container](container.md).
Quarkus permette di costruire un'[immagine docker](images.md) e di fare il push su un [registry](images.md#Registry) (Docker Hub di deafult) in modo molto semplice.
##### Tool
Per eseguire la build e fare il pushing si utilizza uno dei seguenti tool (con le relative estensioni). Tra uno e l'altro possono cambiare i concetti sottostanti, ma la loro manipolazione è la stessa.
- Jib
	Il tool [Jib](https://github.com/GoogleContainerTools/jib) è definito da Google e permette di creare immagini docker demonless, cioè senza che l'engine di Docker sia in esecuzione localmente. L'estensione è $quarkus-container-image-jib$.
- Docker
	L'estensione è $quarkus-container-image-docker$.
- Podman
	L'estensione è $quarkus-container-image-podman$.
- OpenShift
	Permette la costruzione binarie di un'immagine. L'estensione è $quarkus-container-image-openshift$.
##### Building image
Ci sono due modi di costruire le immagini:
- Quarkus CLI
	Si può costruire un'immagine tramite il Quarkus CLI senza installre nessuna estensione e scegliere di volta in volta il tool opportuno.
	Si usa il comando $\texttt{quarkus image build "tool"}$, dove $tool$ è uno di quelli sopra.
- Con estensioni
	Dopo aver installato l'estensione relativa al tool che si vuole utilizzare, si deve e semplicemente il comando $\texttt{quarkus image build}$.
	Se usiamo un'estensione si può anche [configurare](configurazioni.md) nell'$application.property$ il comportamento per cui durante ogni classica build (comando $\texttt{quarkus build}$) deve essere costruita anche l'immagine ed eventualmente fatto anche il push su un registry; questo viene fatto con dei flag posti a $true$ e delle configurazioni dell'account del registry. Solitamente questo non è bene farlo all'inizio per risparmiare tempo, visto che comunque ci mette di più.