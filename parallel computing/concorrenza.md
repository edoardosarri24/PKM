Un processo è formato da più thrad (o [task](Real%20time/task.md)). Un sistema concorrente, a differenza di un [parallelo](parallel%20pragramming.md), esegue un solo thread alla volta a prescindere da quanti siano pronti per l'esecuzione.
# Coordinatore
In un sistema concorrente è fondamentale avere un processo coordinatore, il quale deve assegnare la CPU a un task alla volta, eventualmente interrompere un task e riprenderlo dal punto in cui è stato interrato.
Questi controllori possono eventualmente eseguire in parallelo oppure prendere il controllo della singola CPU per coordinare il tutto.