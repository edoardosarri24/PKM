Le misure nella software engineering riguardano la quantificazione di un qualche attributo. Sono delle caratteristiche (e.g., complessità o affidabilità) del sistema, della documentazione o del processo di sviluppo che possono essere effettivamente misurate.
Si può dire che una metrica è una funzione che prende in input un pezzo di codice e restituisce un valore in outut.
# Tipi
In generale ci possono essere due tipi di misure:
- Controllo
	Servono a misurare i processi. Si dividono in tre tipi: tempo di completamento; risorse richieste; occorrenze di un evento.
- Previsione
	Servono a misurare le caratteristiche del software. Si differenziano in statiche e dinamica: le prima sono ottenute dall'analisi statica del codice, del sistema o della documentazione; le secondo da un'esecuzione del codice (e.g., test).
# Attributi
Un QA (Quality Attribute) è un qualcosa su cui basarsi per ottenere una misura. Si differenziano in:
- Interni
	Sono relativi al codice e sono utilizzati per valutare quelli esterni.
	I più studiati e importanti sono:
	- Profondità dell'albero di ereditarietà
	- [Cyclomatic complexity](cyclomatic%20complexity.md)
	- Line Of Code (LOC)
	- Tasso di errori.
	- [Cognitive complexity](cognitive%20complexity.md)
	- [commenti](commenti.md)
- Esterni
	Sono correlati all'usabilità del sistema da parte dei programmatori e degli utenti.
	Esempi sono la manutenibilità, l'affidabilità e l'usabilità e l'understandability.
# Tool
Ci sono pochi tool che possiamo usare
- [PMD](https://pmd.github.io)
	Usabile da linea di comando.
- [SonarSource](https://www.sonarsource.com)
	È l'inventore della [cognitive complexity](cognitive%20complexity.md) ed è usabile sia da interfaccia che come estensione di qualche IDE.
- [checkstyle](https://checkstyle.sourceforge.io)
	Simile a PDM.
- [sourcemeter](https://sourcemeter.com)
	Simile a PDM.