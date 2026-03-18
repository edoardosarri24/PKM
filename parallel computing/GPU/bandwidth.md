In una [GPU](GPU.md) abbiamo una larghezza di banda molto maggiore rispetto a quella della CPU per portare dati dalla [memoria globale](architettura.md#Globale) ai core.
Il vantaggio di questo è che possiamo avere meno memoria globale ma più veloce.
# Numeri
Vediamo qualche numero sulle differenze:
- CPU
	Possiamo portare circa 50-100GB/s di dati dalla RAM al core.
- GPU
	Possiamo portare circa 150-250GB/s di dati dalla RAM al core. Oggi siamo sui 1500GB/s.
- PCI
	Solitamente si portano 8-16GB/s di dati.
# Bottleneck
Il collo di bottiglia principale quando usiamo una GPU è dato dal PCI bus, cioè il bus che collega la CPU alla GPU.
##### Soluzione
Per risolvere questo problema si sta andando (e.g., Apple) verso System on Chip (SoC), cioè architetture che hanno sullo stesso chip sia CPU che GPU: questo comporta che spostare i dati non devono essere spostati dalla ram del devide host alla global memory del device.