CMake è un meta sistema di build il cui scopo è generare un [build system](generators.md) di un progetto basandosi sulle configurazioni dell'utente.
# Build
Una volta che siamo nella root directory per fare la build, in ordine, si deve:
- $cmake\ -B\ build$
	Ecita l'in-source builds. I file generati durante la build saranno messi nella cartella $build$; se la cartella non esiste (nella root directory) allora viene creata.
- $cmake\ --build\ build$
	Esegue la build dalla e nella cartella $build$.
- $cmake\ --build\ build\ --clean-first$
	Questo è un'alternativa a quello sopra e permette di pulire quello che non serve più.
# Topic
- [generators](generators.md)