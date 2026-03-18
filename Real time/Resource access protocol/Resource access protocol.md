# DEFINZIONI
- Risorsa
	Ogni struttura sw necessaria al task per avanzare nell'esecuzione. Può essere:
	- Privata
		Dedicata a un particolare task. Non crea problemi lato condivisione, perché non è condivisa.
	- Condivisa
		Può essere usata da più task. Non crea problemi lato condivisione finché non è esclusiva, cioè che deve essere protetta da accessi concorrenti.
- Resource access protocol
	Meccanismo che garantisce la mutua esclusione di una risorsa.
- Critical section (CS)
	Parte di codice che deve essere eseguito in mutua esclusione. Solitamente gestise una risorsa.
# PRIORY INVERSION
È il fenomeno per cui un task a priorità più alta è bloccato da uno a priorità più bassa per un tempo indefinito.
Il blocked time è il tempo per cui il task a priorità più alta è bloccato. È fondamentale che questo sia indefinito (altrimenti non si parla di priority inversion).
- Se un task a priorità più alta è bloccato da un task a priorità più bassa perché quest'ultimo è in CS allora va bene e non si parla di priority inversion. In questo caso il blocked time è in funzione solo del tempo che il task a priorità più bassa trascorre nella CS. [Esempio](not%20priority%20inversion.png).
- Per essere priority inversion ci deve essere almeno un terzo task a priorità media (e di solito più di uno) che ha il controllo della CPU mentre quello a priorità più alta è bloccato, ma senza che questo sia dovuto a una CS. In questo caso il blocked time è in funzione sia del tempo della CS sia da quanti task a priorità intermedia arrivano. [Esempio](priority%20inversion.png).
# FORMULAZIONE DEL PROBLEMA
- Un set di $n$ [task](Real%20time/task.md) periodici $\Gamma\{\tau_1,\cdots,\tau_n\}$, dove ogni $\tau_i$ dei è caratterizzato da:
	- Fase $\Phi_i$, WCET $C_i$, periodo $T_i$ e deadline relativa $D_i\le Ti$.
	- Priorità $P_i$ nominale assegnata dal progettista.
	- Priorità dinamica $p_i\ge P_i$ inizializzata a $P_i$.
- Un set di $m$ risorse condivise $\Psi=\{R_1,\cdots,R_m\}$, dove ogni risorsa $R_i$ è gestita da un semaforo binario $S_i$.
- Assunzioni
	- I task sono rilasciati al loro arrivo.
	- Il context swith non ha overhead.
	- I task hanno priorità nominale diversa.
	- I task sono elencati in ordine decrescente di priorità nominale.
	- I task si possono auto sospendere solo se hanno fino il loro compito (idle time) o se vogliono accedere a un semaforo bloccato.
	- I semafori sono correttamente innestati: si rilasciano in ordine invrso rispetto a come sono stati acquisiti.
# ASPETTI CHAVE DEI PROTOCOLLI
- Accesso
	Si decide se bloccare un task e quando.
- Progresso
	Si decide come un task si comporta in CS.
- Rilascio
	Si decide come un processo si comporta quando rilascia una risorsa.
# Protocolli
- [Priority Inheritance protocol (PIP)](Priority%20Inheritance%20protocol%20(PIP).md)
- [Priority ceiling protocol (PCP)](Priority%20ceiling%20protocol%20(PCP).md)