## STATICA/NOMINALE
È tipica dei linguaggi compilati, come Java, C e C++. Il tipo degli oggetti si dichiara staticamente, cioè in fase di compilazione.
- Vantaggi
	- Affidabilità
		Il compilatore può controllare ed evitare errori di tipo a run-time.
	- Efficienza
		Le conversioni di tipo vengono fatte in fase di compilazione; in fase di esecuzione non ci sono conversioni di tipo superflue.
	- Leggibilità
		È chiaro chi sta facendo cosa.
## DINAMICA 
È tipica dei linguaggi interpretati, come Python e JavaScript. Il tipo è definito in fase di esecuzione.
- Vantaggi
	- Flessibilità
		Si ha codice riusabile in più contesti. Per esempio ad una variabile può essere assegnato prima un tipo e poi un altro.
	- Complessità
		Si ha bisogno di meno codice e meno relazioni tra oggetti.
- Svantaggi
	- Affidabilità
		In fase di compilazione è solo il programmatore che controlla di assegnare e passare parametri di tipo corretto. In fase di esecuzione ci saranno delle conversioni di tipo (Cioè il programma dovrà interpretare le varibaili al momento corrente, visto che il loro tipo potrebbe essere cambiato rispetto al passato) che potrebbero fallire nel caso i tipi non siano compatibili.
	- Efficienza
		Le conversioni sono fatte in fase di esecuzione e quindi si può avere un po’ di perdita di tempo.