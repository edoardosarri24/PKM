Per valutare il clustering si possono usare delle [Metriche](machine%20laerning/Metriche.md#Non%20supervisionato) per i modello non supervisionati.
# Coesione
Implementando la [coesione](machine%20laerning/Metriche.md#Coesione) per il cluster $i$-esimo con la Sum of Squered Error (SSE) si arriva a definire $SSE(C_i)=\sum_i\sum_{x\in C}(x-m_i)^2$, dove $m_i$ è il centroide del cluster.
Usando l'errore quadrico stiamo penalizzando maggiormente gli elementi che sono più lontani dal centroide.
# Separazione
Implementando la [separazione](machine%20laerning/Metriche.md#Separazione) per il cluster $i$-esimo con la Sum of Squered Error (SSE) si arriva a definire $SSE(C_i)=\sum_i|C_i|(m-m_i)^2$, dove $m_i$ è il centroide del cluster. Dobbiamo pesare la cardinalità del cluster perché volgiamo che cluster più grandi contribuiscano maggiormente all'errore.
Usando l'errore quadrico stiamo penalizzando maggiormente gli elementi che sono più lontani dal centroide.
# Silhouette Coeﬃcient
Vedi [silhouette coeﬃcient](machine%20laerning/Metriche.md#Silhouette%20Coeﬃcient).