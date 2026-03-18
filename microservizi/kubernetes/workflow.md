Il workflow di [kubernetes](microservizi/kubernetes/kubernetes.md) si basa sul concetto di stato desiderato. Non diciamo a Kubernetes di avviare un container in modo imperativo, ma più tosto quale è lo stato che vogliamo raggiungere; questo stato desiderato è definito all'interno del manifesto.
# Control pattern
Kubernetes utilizza il control pattern: ogni intervallo di tempo confronta lo stato attuale con quello desiderato; se ci sono da fare dei cambiamento allora li applica, altrimenti lascia tutto invariato.
# Manifest
Lo stato desiderato viene definito all'interno di un file YAML detto manifesto. Una volta definito, per variare lo stato si può cambiare il manifesto e applicarlo nuovamente; usare $\texttt{kubectl}$ con dei parametri, che cambiarà il manifesto.