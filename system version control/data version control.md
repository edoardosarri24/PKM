[Github](github.md) non è costruito per i ospitare i dati: i file devono avere una dimensione inferiore ai 100 MB e l'intero repository inferiore ai 5 GB. Se il nostro progetto lavora con dati molto grandi allora ci serve un DVC (Data Version Control).
# Idea
L'idea si basa sul fatto di separare i metadati del dataset e i dati reali.
- Metadati
	Sono salvati su git e solitamente caricati su github e sono piccoli file con estensione $\texttt{.dvc}$.
- Dati
	Vengono salvati in un qualche storage. Questo può essere personale (e.g., Google Drive) o pubblico (e.g., Hugging Face) ed è agnostico rispetto al sistema di versionamento di git.