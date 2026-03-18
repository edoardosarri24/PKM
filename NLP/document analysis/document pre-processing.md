Nell'[analisi dei documenti](document%20analysis.md) il primo step dopo l'acquisizione del documento è il pre-processing.
L'obiettivo è modificare il documento in modo da ottenere una forma che durante l'analisi minimizza il costo e l'accuratezza.
# Filtraggio
È un'operazione di basso livello in cui si lavora su ogni pixel per migliorare la successiva velocità di computazione.
##### Binarizzazione
L'idea è di trasformare in bianco e nero un'immagine in scala di grigi.
- Una soluzione naive è porre una soglia: se il pixel ha un valore maggiore allora è bianco altrimenti è nero.
- Una tecnica più avanzata per decidere la soglia è produrre un istogramma della scala di grigi: la curva che si ottiene dovrebbe avere due massimi, uno sui valori vicini a 255 (i.e., il bianco) e uno su valori vicino a 0 (i.e., il testo); si pone la soglia a metà tra questi due massimi. Per migliorare possiamo adottare questa tecnica localmente.
# Componenti connesse
Si vuole identificare caratteri, parole e frasi in base alle componenti connesse.
##### Approccio
Con questo approccio si riescono ad identificare caratteri. Per le parole servono delle estensioni.
- Dato un pixel i suoi vicini sono o quattro od otto: nel primo caso sono quelli sotto e quelli sopra; nel secondo si considerano anche quelli in diagonale.
- Un percorso è una sequenza $P_1,\cdots,P_n$ di pixel neri dove $P_{k-1}$ è un vicino di $P_k$ e $P_k$ è un vicino di $P_{k+1}$.
- Un insieme di pixel $P$ è connesso se per ogni coppia esiste un percorso che li connette.
- Una componente connessa è un insieme di pixel neri che sono connessi.
# Rotazione
Sono delle tecniche per capire se il testo in un documento è ruotato rispetto all'asse orizzontale.
##### Hough transform
È una trasformata che permette di rilevare forme regolari in un'immagine.
In uno spazio cartesiano una retta è definita da $y=mx+q$; possiamo rappresentare una retta invece in coordinate polari come $\rho=xcos\theta+ysin\theta$, dove $\rho$ è la distanza dall'origini e $\theta$ è l'angolo tra l'orizzonte e la normale alla retta passante per l'origine.
Per ogni pixel nero, se nello spazio dell'immagine teniamo fermi $x$ e $y$ (i.e., il nostro pixel nero) e variamo $\theta$ otteniamo una curva sinusoide (i.e., curva descritta dalla funzione seno): ogni punto diventa una curva sinusoidale nello spazio dei parametri $\theta$.
Si definisce una matrice dove le righe rappresentano tutti i valori di $\theta$ e le colonne le distanze $\rho$. Per ogni pixel nero si incrementa la cella relativa alle coppie ($\theta$, $\rho$) che soddisfano l'equazione $\rho=xcos\theta+ysin\theta$ per quel punto. Se tre punti sono all'allineati, le loro sinusoidi di intersecheranno sempre nello stesso punto ($\theta_0$, $\rho_0$); se facciamo la stessa cosa dovremmo vedere che ci sono celle con valori molto più alti in prossimità dell'inclinazione del documento $\theta_0$.