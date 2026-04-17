In una [rete di telecomunicazioni](Reti%20di%20telecomunicazioni.md) ci possono essere diversi tipi di traffico.
# Elastico
Può adattare il suo throughtput e delay. Si tratta del classico traffico che utilizza [TCP IP](TCP%20IP.md) o [UDP](UDP.md) come mezzo ed è quindi il traffico del primo internet.
Alcuni casi d'uso sono: le mail, il trasferimento di file tra utenti. A seconda dell'applicazione il traffico può cambiare in efficienza, ad esempio per utenti diversi con [QoS](Reti/misure.md#QoS) diversi.
# Anelastico
Si tratta del trasferimento delle informazioni per cui non possiamo variare i delay o throughtput. Sono richiesti in questo caso delle garanzie su delay, throughtput, jitter e packet loss.
Alcuni casi d'uso sono: comunicazione voce, intrattenimento dove è presente interazione, multimedia e scenari real-time.
##### Reat-time
Siamo nello scenario [real time](real%20time.md) dove abbiamo vincoli temporali.
I pacchetti possono essee spediti con modalità diverse:
- Continuo
	I pacchetti hanno una dimnsione fissa e sono generati a intervalli regolari.
- On/Off
	I pacchetti sono sempre di dimensione fissa, ma si alternano invii a intervalli regolari e periodi di non invio.
- Variabile
	Dimensione variabile e intervalli di invio regolari.