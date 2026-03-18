L'utilizzo di [cmake](cmake.md) ruota attorno a al file $CmakeLists.txt$ (CML). Sicuramente avremo un CML nella root directory del progetto, ma potremmo voler specificare delle configurazioni particolari per le sotto directory.
# Obbligatori
Questi due comandi ci devono essere all'inzio del CML nella root directory:
- $cmake\_minimum\_required(VERSION\ x)$
	È utile per scopi di compatibilità. Siccome CMake garantisce la back compability allora si richiede che ci sia almeno una data versione, in questo caso la $x$.
- $project(MyProjectName)$$
	Definisce il nome del progetto. È un comando semplice che al suo interno nasconde tante funzioni complesse di controllo del sistema.
# Path file
I percorsi nel CML sono sia relativi che assoluti. Se li intendiamo come relativi allora si intende dall directory che contiene il CML.