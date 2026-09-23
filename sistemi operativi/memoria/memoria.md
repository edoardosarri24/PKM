La memoria può essere vista come un vettore di celle consecutive che possono essere trattate singolarmente o a gruppi.
# Cache
Oggi siamo in un paradigma dove ogni processore ha più core, dove ognuno di questi è composto ALU e cache (e altro). Stiamo seguendo il [CTA model](memory%20model.md#CTA%20model): tra la cache più vicina al core e la RAM, ci sono altri livelli di memoria (solitamente 3 totali più la RAM).
# Famiglie
Ci sono tre classi di memorie: RAM, ROM e hybrid.
![famiglie](famiglie.png)
##### RAM
Sono le Random Access Memory: il costo per accedere a ogni loro locazione è costante. Sono memorie sia in lettura che scrittura.
Le due varianti differiscono per il ciclo di vita dei dati salvati:
- SRAM
	La Static RAM memorizza i dati finchè è alimentata.
	- Acesso 4 volte più veloce della DRAM.
	- Più costosa.
- DRAM
	La Dynamic RAM memorizza i dati finché è elimanetata e per pochi centinaia di $ms$. C'è un dispositivo integrato nei chip con DRAM, il DRAM controller, che fa un refresh dei bit in modo da allungare il ciclo di vita dei dati.
	- Meno costosa. Utile quando c'è bisogno di tanta RAM.
	- Accesso più lento di circa 4 volte.
##### ROM
Sono le Read Only Memory dove i dati possono essere mantenuti per sempre, anche in assenza di corrente.
- Masked ROM
	Sono le memorie scritte dal produttore e che non possono essere più riscritte.
	- Costo molto basso.
- PROM
	Sono le memorie One Time Programmable (OTP): si possono scrivere una volta e i dati contenuti al suo interno dopo non possono essere modificati.
- EPROM
	Sono le Erasable and Programmable ROM: possono essere cancellate e riscritte più volte. La cancellazione avviene tramite luce ultravioletta e non può essere fatrta in-place (i.e., durante l'esecuzione o in HW).
##### Hybrid
Sono memorie che prendono il meglio delle RAM e delle ROM.
- EEPROM
	Sono le Elettrical, Erasable and Programmable ROM: sono delle EPROM che possono essere cancellate (byte per byte) tramite segnali elettrici.
	- Hanno un costo alto.
	- Il costo di scrittutura è alto rispetto a una RAM.
- Flash
	Sono come le EEPROM ma la cancellazione avviene a settori e quindi più veloce.
- NVRAM
	Sono le Non Volatile RAM e sono delle SRAM con una batterie che permette al dato di stare in vita anche senza carica.
	- Hanno un costo alto.