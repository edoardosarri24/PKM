Si tratta di un [obiettivo](multi%20obiettivo%20in%20MAS.md) in un sistema multi agente in cui si vuole mappare uno spazio molto più grande rispetto al sensore che si occupa di acquisire le informazioni (altrimenti il task è di [coverage](coverage.md)).
# SLAM
Per portare a termine questo compito gli agenti devono continuare a muoversi; per questo solitamente si parla di [SLAM](SLAM.md), cioè si uniscono i compiti di mappatura ed esplorazione.
# Tecniche
Per risolvere questo problema esistono soluzioni offline e online: nelle prime gli agenti raccolgono le informazioni e poi aggiornano la mappa in differita; nelle seconde gli agenti aggiornano la mappa appena raccolgono le informazioni.
##### Quantitative
Sono le mappe che danno informazioni geometriche: la distanza tra punti, gli angoli e le coordinate.
- [griglia di occupazione](griglia%20di%20occupazione.md)
- [feature-based map](feature-based%20map.md)
- [point-cloud map](point-cloud%20map.md)
##### Qualitative
Sono dette anche simboliche e rappresentano informazioni topologiche e semantiche.
Sono utili quando ci servono informazioni sull'ambiente di alto livello e non informazioni geometriche di basso livello.
- Topologiche
	Rappresentano informazioni sulla relazione tra luoghi della mappa.
	Sono robuste ai cambiamenti dell'ambiente perché si occupano della connettività tra gli elementi della mappa e non della sua geometria.
- Semantiche
	Includono informazioni su cosa sia rappresentato nella mappa.
	Hanno lo svantaggio si essere computazionalmente complesse visto l'utilizzo di reti neurali.
##### Ibride
Sono mappe che includo informazioni sia quantitative che qualitative.