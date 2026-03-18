Nel self-supervised learning la supervisione (le etichette) è derivata dai dati. È una tecnica molto utile per quei problemi in cui non abbiamo molti dati annotati oppure annotarli sarebbe estremamente costoso.
La sfida è raggiungere i livelli del supervised learning nonostante non si abbiano molti dati annotati.
##### Idea
L'idea è quella di definire due task, uno detto pretext (o proxy) task e il secondo detto downstream task.
- Il proxy task è usato addestrare il modello sui dati non annotati. Non rappresenta l'obiettivo vero e proprio, ma è un obiettivo fittizio per arrivare a una rappresentazione semantica utile che sarà alla base del downstream.
- Il downstream task utilizza i pochi dati annotati e l'obiettivo è fare fine tuning a partire dai pesi derivati dal proxy task.
##### Cheating
Il vero compito del self suervised learning è quello di stabilire e impostare correttamente il proxy task.
In generale la rete cercare di risolverlo nel modo più semplice possibile e questo può portare a non apprendere i dettagli semantici ma solo a risolvere il task.
# Genaral purpose
Vediamo dei lavori che sono utili per capire come impostare il proxy task in modo che la rete impari dettagli semantici da immagini non annotate utili per scopi generici.
- [Spatial contetx prediction](Spatial%20contetx%20prediction.md)
- [Counting low-level](Counting%20low-level.md)
- [[Semantica dall'arco temporale]]
# Niche problems
Ci sono molti problemi che non possono essere risolti basandosi sulla semantica imparata da contesti generali.
In molti di questi casi avere delle annotazioni è praticamente impossibile o comunque molto difficile. Esempi di questi casi sono medical image recognition dove le annotazioni devono essere fatte da più esperti e quindi troppo costose, face recognition dove il problema riguarda la privacy.
- [Dense crowd counting](Dense%20crowd%20counting.md)
- [Image Quality Assessment (IQA)](Image%20Quality%20Assessment%20(IQA).md)