Un container è una delle due tecniche per implementare la [virtualizzazione](virtualizzazione.md). È un'astrazione a livello di sistema operativo: il container parla direttamente con il kernel dell'OS host tramite l'engine (e.g., [docker](docker.md)).
Ogni applicazione che gira nel proprio container gira in un [user space](Operating%20System%20(OS).md#User%20space) dell'OS host diverso, ma condivide lo stesso kernel. Non percepisce di avere un'intera macchina a disposizione, ma è isolato rispetto ai processi, alle reti e al file system.
# Conseguenze
- Sono più efficienti di una VM perché sono più leggeri e richiedono quindi meno risorse.
- L'avvio di un container è solitamente molto veloce.
# Principi
Un principio base dei container è quello relativo al [single responsability principle](single%20responsability%20principle.md): un container deve fare una cosa e farla bene. Ci sono delle eccezioni a questa regola, ma in generale quando si devono fare cose più complesse si utilizzano più container insieme, ad esempio tramite [Docker compose](docker.md#Docker%20compose).
# Vantaggi
I container offrono numerosi vantaggi:
- Self-contained
	Un container contiene tutto ciò che è necessario per essere eseguito. Non sono necessarie altre installazioni oltre al runner di Container (e.g., [docker](docker.md)).
- Isolato
	L'esecuzione di un container non interferisce con altri container o con i processi della macchina host. Eliminare un container non ha implicazioni sugli atri.
- Portabile
	Distribuire un container è molto facile. Per eseguirlo basta avere installato il runner di Container (e.g., [docker](docker.md)).
- Componibili
	Visto il SRP, per fare cose complesse si compongono contianer (e.g., [docker compose](docker%20compose.md)).