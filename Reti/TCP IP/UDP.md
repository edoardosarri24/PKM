È un protocollo per l'implementazione del livello [trasporto](TCP%20IP.md#TRANSPORT%20-%20[UDP](UDP.md)%20-%20[TCP](TCP.md)) della pila protocollare [TCP/IP](TCP%20IP.md).
Trasporta i dati in pacchetti chiamati datagram e composti da header e payload. Si usa la [commutazione di pacchetto a datagramma](Trasmissione.md#Datagramma): questo implica che il protocollo UDP offra solo un servizio [inaffidabile](Architettura%20a%20livelli.md#SERVIZIO) e [connectionless](Architettura%20a%20livelli.md#SERVIZIO). Non aggiunge funzionalità rispetto al livello [internet](TCP%20IP.md#INTERNET%20(IP)%20-%20[IPv4](IPv4.md)%20-%20[IPv6](IPv6.md)), ma permette solo la comunicazione tra processi remoti. L'affidabilità viene risolta basandoci sul concetto che la probabilità di trovare errori cresce al crescere del numero di bit trasportati; per questo motivo i datagram nel protocollo UDP sono di piccole dimensioni.
È un protocollo veloce e snello che si adatta alle situazioni in cui si privilegia la velocità di trasmissione all’affidabilità (Trasmissione video e audio) e nelle situazioni in cui non c’è molta interazione client-server.
## HEADER
L’header ([Figura 4.17](Figura%204.17.png)) ha una lunghezza fissa di 8 byte. I suoi campi sono:
- Numero di porta sorgente/destinazione
	Indica il numero di porta usato dal processo mittente/ricevente, cioè i processi che si devono interfacciare.
- Lunghezza
	Indica la lunghezza del payload.
- Checksum
	È usato per controllare la correttezza dell'header; se questo controllo non ha esito positivo il datagram viene scartato (Servizio inaffidabile).