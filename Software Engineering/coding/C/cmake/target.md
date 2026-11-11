Un target in [cmake](cmake.md) è un contenitore di informazioni che descrive come deve essere compilato l'artefatto.
Per creare un target $MyProgram$ si deve aggiungere nel CML $add\_executable(MyProgram)$.
# Proprietà
Un target può contenere diverse proprietà. In generale il comando per settare una proprietà è $\texttt{set\_property(TARGET target PROPERTY property)}$.
- Tipo
	Se è un eseguibile, una libreria o una collezione di header.
- Codice sorgente
	Per associare il codice sorgente a un target si usa $target\_sources(MyProgram\ PRIVATE\ main.cxx)$, dove le $xx$ sono o nulla (nel caso del C) o $pp$ per il C++.
- Include sub directories
	Per includere i CML presenti nelle subdirectory si usa il comando $add\_subdirectory$. I percorsi relativi nei CML delle subdirectory sono relativi a quella subdirectory e non vedono cosa nella directory padre.
- Nome di output
- Dipendenze
- Flag per il compiltore e linker
##### Visibilità
Quando aggiungiamo una proprietà a un target possiamo definire chi usa quella proprietà:
- Private
	Solo questo target. Se il nostro target è un eseguibile allora nessuno dipenderà da esso.
- Public
	Questo target e quelli che dipendono da esso. Nel caso in cui il target è una libreria chi importa tale libreria.
- Interface
	Solo i target che dipendono da esso.