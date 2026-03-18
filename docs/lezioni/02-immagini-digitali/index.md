---
layout: default
title: Lezione 02 - Immagini Digitali
parent: Lezioni
nav_order: 3
has_children: false
description: "Rappresentazione digitale, campionamento, quantizzazione, spazi colore, filtraggio e processing di immagini"
---

# Lezione 02: Immagini Digitali

**Docente:** Prof. Annalisa Franco, UniBo
**Argomenti:** Rappresentazione, acquisizione digitale, colore, filtraggio e processing

---

## Overview

In questa lezione affrontiamo come le immagini continue del mondo reale vengono trasformate in rappresentazioni digitali discrete:

- **Formazione dell'immagine**: dal continuo al discreto mediante campionamento e quantizzazione
- **Immagini digitali**: rappresentazione matriciale, pixel, valori di intensità
- **Colore**: sintesi additiva (RGB) e sottrattiva (CMYK), spazi colore perceptuali (HSV, YCbCr)
- **Operazioni su immagini**: trasformazioni, convoluzione, filtraggio
- **Filtraggio lineare**: blur, sharpening, edge detection, kernel Gaussiano
- **Estrazione di edge**: gradiente, operatori di gradient (Roberts, Prewitt, Sobel)
- **Filtraggio frequenziale**: trasformata di Fourier, FFT, filtri passa-basso/alto/banda

---

## 1 Formazione e Rappresentazione dell'Immagine

### Dal Continuo al Discreto

Una **immagine continua** è una funzione f: R² → R che assegna un valore di intensità luminosa a ogni punto dello spazio.

**Immagine digitale** è il risultato di due processi fondamentali:

1. **Campionamento (Sampling):** acquisizione di campioni del segnale analogico a intervalli uniformi su una griglia con w x h celle
2. **Quantizzazione (Quantization):** approssimazione di ciascun campione con uno dei valori appartenenti a un range predefinito (finito)

### Immagine come Funzione

Un'immagine può essere interpretata matematicamente come una funzione:

**f: [a, b] × [c, d] → [0, 1] (oppure [0, 255])**

Dove:
- **(x, y):** coordinate spaziali (posizione nel piano immagine)
- **f(x, y):** valore di intensità luminosa (luminanza) in quella posizione
- **Range:** [0, 1] per immagini normalizzate, [0, 255] per 8-bit, ecc.

**Immagine a colori:** semplice composizione di tre funzioni, una per canale (Red, Green, Blue):

```
f(x, y) = [ r(x, y) ]
          [ g(x, y) ]
          [ b(x, y) ]
```

---

## 2 Immagini Digitali: Campionamento e Quantizzazione

### Rappresentazione Matriciale

Un'**immagine digitale** di dimensioni w × h è un **segnale discreto** (insieme finito di valori), ottenuto attraverso due processi:

- **Campionamento:** acquisizione di campioni del segnale analogico su intervalli uniformi (griglia con w × h celle)
- **Quantizzazione:** approssimazione di ciascun campione con uno dei valori appartenenti a un range predefinito (finito)

L'immagine è rappresentabile come una **matrice** (con w colonne e h righe); ciascun elemento rappresenta un **pixel** (picture element) ottenuto tramite campionamento e quantizzazione del segnale originale.

### Immagine Grayscale

Un'immagine grayscale w × h può essere rappresentata come una matrice con valori codificati nel range [0, 255]:

```
Immagine originale  →  Campionamento+Quantizzazione  →  Matrice di valori
(continua)             (griglia w x h pixel)          (discreti [0,255])
```

### Immagine a Colori

Un'immagine a colori è la **composizione di tre canali** indipendenti (Red, Green, Blue):

```
Immagine RGB = [R(x,y)]
               [G(x,y)]
               [B(x,y)]
```

Ogni canale è una matrice w × h con valori [0, 255]. La percezione del colore umano sfrutta tre tipi di coni dell'occhio sensibili a rosso, verde e blu. I colori misti si vedono come somma delle loro componenti RGB.

---

## 3 Colore e Spazi Colore

### Sintesi Additiva (RGB)

I tre colori fondamentali per la **sintesi additiva** sono quelli a cui sono sensibili i coni dell'occhio umano: **rosso (Red), verde (Green), blu (Blue)**. I colori misti si vedono come somma delle loro componenti RGB.

**Sintesi additiva usata da:**
- Fotocamere digitali
- Monitor e display (schermo luminoso)
- Sorgenti luminose (LED, laser che emettono colori diversi)

**Disco di Newton:** Facendo ruotare velocemente un disco con settori circolari di colori diversi, i colori vengono mescolati e si ottiene bianco.

### Sintesi Sottrattiva (CMY/CMYK)

I tre colori fondamentali in **sottrattiva** sono i **complementari** dei tre colori fondamentali della sintesi additiva: **giallo (Yellow)** complementare del blu, **magenta (Magenta)** complementare del verde, **ciano (Cyan)** complementare del rosso (Cyan, Magenta, Yellow, **CMY**).

**L'esempio più semplice di sintesi sottrattiva:** filtri colorati (ogni filtro sottrae una parte della luce che lo attraversa). Ogni filtro sottrae una parte della luce che lo attraversa; il colore che giunge al nostro occhio è quello che riesce a passare per tutti i filtri.

**Altri esempi di sintesi sottrattiva:**
- **Mescolanza di colori in pittura:** ogni colore è un filtro colorato
- **Pellicole foto e cinematografiche a colori:** pellicola in effetti ricoperta di tre strati sovrapposti (uno giallo, uno magenta, uno ciano)

### Spazi Colore Percettivi

Uno **spazio colore** è un **modello multidimensionale** utilizzato per la **rappresentazione dei colori**. I diversi spazi colore differiscono per la **scelta degli assi**.

**Aspetti fondamentali per la descrizione di un colore:**

- **Tinta (Hue):** il colore (rosso, verde, giallo, blu, ...)
- **Saturazione (Saturation):** la "purezza" di uno stesso colore brillante (alta saturazione) o desaturato/grigiastro (bassa saturazione)
- **Luminosità/Valore (Luminance/Value):** la presenza in un colore di bianco/nero (versione chiara, grigia, scura)

### RGB vs HSV/HSL vs YCbCr

**RGB:**
- È lo spazio RGB più semplice
- Poco adatto per raggruppare spazialmente colori simili dal punto di vista della percezione umana

**HSV e HSL:**
- Codificano le **principali caratteristiche del colore**: tinta (Hue), saturazione e luminosità/valore
- Sono **più adatti per l'individuazione di pattern di interesse** con specifiche caratteristiche cromatiche
- Nello spazio **HSV** l'asse verticale rappresenta V (Value, brightness; cilindro)
- Nello spazio **HSL** la dimensione verticale codifica L (Lightness, intesa normalmente come media dei colori; cono)

**YCbCr:**
- Codifica il colore attraverso tre componenti: Luma (Y) che rappresenta la luminosità, Cb e Cr che sono le due componenti cromatiche (blue-difference e red-difference, rispettivamente)
- È stato proposto in relazione alla **trasmissione digitale di immagini** (nel mondo analogico si chiama YUV)
- Lo spazio RGB è poco efficiente in quanto contiene un elevato grado di ridondanza
- L'occhio umano è più sensibile a cambiamenti di **luminosità** rispetto a cambiamenti di **cromaticità**
- Per ridurre la quantità di informazioni trasmesse il canale Y viene codificato a "risoluzione maggiore"

---

## 4 Conversioni tra Spazi Colore

### RGB ↔ HSV, HSL (Formule)

Per passare da RGB (con R, G, B ∈ [0,1]) a HSV:

```
MAX = max(R, G, B)
MIN = min(R, G, B)

H := { 0°,                           if MAX = MIN ⟺ R = G = B
     { 60° · (0 + (G-B)/(MAX-MIN)),  if MAX = R
     { 60° · (2 + (B-R)/(MAX-MIN)),  if MAX = G
     { 60° · (4 + (R-G)/(MAX-MIN)),  if MAX = B

if H < 0° then H := H + 360°

S_HSV := { 0,           if MAX = 0 ⟺ R = G = B = 0
         { (MAX-MIN)/MAX, otherwise

V := MAX
```

Per HSL:
```
L := (MAX + MIN) / 2

S_HSL := { 0,           if MIN = 1 ⟺ R = G = B = 1
         { (MAX-MIN)/(1-|MAX+MIN-1|), otherwise
```

### RGB ↔ YCbCr (Formule)

I segnali Y, C_b, C_r prima di essere processati per ottenere un segnale in forma digitale, sono chiamati Y'PbPr, e sono creati dai corrispondenti primari RGB corretti in gamma usando due costanti Kr e Kr come segue:

```
Y' = Kr × R' + (1 - Kr - Kb) × G' + Kb × B'
Pb = 0.5 × (B' - Y') / (1 - Kb)
Pr = 0.5 × (R' - Y') / (1 - Kr)
```

Per passare **direttamente da RGB a YCbCr:**

```
Y = 0.299(R - G) + G + 0.114(B - G)
Cb = 0.564(B - Y)
Cr = 0.713(R - Y)
```

In realtà sono disponibili codifiche diverse che differiscono per piccoli variazioni nei coefficienti utilizzati.

---

## 5 Operazioni su Immagini

### Problematiche Comuni

Le immagini digitali possono essere affette da diverse **problematiche**:

| Problema | Descrizione | Causa |
|----------|------------|-------|
| **Sfocatura (Blur)** | Immagine non nitida, contorni sfuocati | Movimento, messa a fuoco errata |
| **Rumore** | Variazioni casuali di intensità | Sensore difettoso, bassa illuminazione |
| **Basso contrasto** | Differenze di intensità ridotte tra oggetti | Scarsa illuminazione, esposizione errata |
| **Aliasing** | Artefatti dovuti a undersampling | Risoluzione insufficiente |
| **Edge** | Contorni/bordi da enfatizzare | Applicazione di sharpening |

### Trasformazioni Elementari

Come per qualsiasi altra funzione, è possibile applicare a un'immagine degli **operatori** che siano in grado di realizzare alcune trasformazioni:

```
g(x, y) = f(x, y) + 20    (aumento contrasto/luminosità)
g(x, y) = f(-x, y)        (mirroring orizzontale)
```

Tra le possibili trasformazioni riveste **particolare importanza l'operazione di convoluzione** con **filtri digitali** in quanto permette di realizzare numerose operazioni interessanti (sharpening, blurring, edge extraction, etc.)

---

## 6 Filtraggio Digitale e Convoluzione

### Principio Fondamentale

**Applicare un filtro digitale** significa modificare il valore di ciascun pixel dell'immagine sostituendolo con un valore calcolato a partire dai valori dei pixel dell'intorno.

Una trasformazione molto semplice sostituisce il valore di ciascun pixel con una **combinazione lineare** dei suoi vicini (filtraggio lineare, convoluzione).

La modalità usata per combinare i pixel (i **pesi** nella combinazione lineare) determina il **kernel** (maschera, filtro): kernel diversi producono **effetti diversi** sull'immagine.

### Cross-Correlation e Convoluzione

Siano:
- **f:** un'immagine digitale
- **h:** un kernel di dimensioni 2k + 1 × 2k + 1

Il risultato dell'operazione di **cross-correlation** g = h ⊗ f è dato da:

```
g(x, y) = sum_{u=-k}^{k} sum_{v=-k}^{k} h(u, v) · f(x+u, y+v)
```

Se il kernel viene **capovolto orizzontalmente e verticalmente** si ha l'operazione di **convoluzione** g = h * f:

```
g(x, y) = sum_{u=-k}^{k} sum_{v=-k}^{k} h(u, v) · f(x-u, y-v)
```

---

## 7 Filtri Lineari (Esempi Pratici)

### Identity Filter (Identita)

Kernel con singolo 1 al centro, resto 0:
```
h = [0 0 0]
    [0 1 0]
    [0 0 0]
```
**Effetto:** immagine identica (non viene modificata)

### Shift Filter

Kernel con 1 a sinistra del centro:
```
h = [0 0 0]
    [1 0 0]
    [0 0 0]
```
**Effetto:** shift dell'immagine a sinistra di 1 pixel

### Blur Filter (Low-Pass)

Kernel con valori uguali, normalizzati (1/9):
```
h = 1/9 [1 1 1]
        [1 1 1]
        [1 1 1]
```
**Effetto:** sfocatura (blur), riduzione del rumore, perdita di dettagli

### Sharpening Filter

Kernel che amplifica il pixel centrale:
```
h = [0  0  0]   -  1/9 [1 1 1]
    [0  2  0]          [1 1 1]
    [0  0  0]          [1 1 1]
```
**Effetto:** esaltazione dei dettagli, aumento del contrasto, evidenzia edge

### Kernel Gaussiano

Un kernel **gaussiano** è un filtro passa-basso (radiale) nel quale la riduzione delle alte frequenze avviene gradualmente (e non con un brusco scalino).

**Formula matematica:**
```
g_σ = (1 / (2πσ²)) * exp(-(x²+y²) / (2σ²))
```

Dove σ (sigma) controlla la larghezza della gaussiana:
- **σ piccola (1-5 pixel):** blur leggero, dettagli preservati
- **σ grande (10-30 pixel):** blur forte, immagine molto sfocata

**Applicazione:** riduzione rumore gaussiano, smoothing, preprocessing per estrazione feature

---

## 8 Estrazione di Edge e Gradiente

### Edge e Bordi

Molte **tecniche di estrazione di feature** si basano sulla **codifica di informazioni** relative agli **edge** (bordi) presenti nell'immagine.

Un **edge è una zona dell'immagine** caratterizzata da una **repentina variazione dell'intensità**.

**Relazione tra edge e derivata prima:**

```
Immagine              Intensità osservata         Derivata prima
(continua)            lungo la linea              (massimi/minimi)
```

Gli **edge corrispondono ai punti estremi** (massimi e minimi) della derivata prima.

### Il Gradiente dell'Immagine

**Problema:** come differenziare un'immagine discreta?

**Soluzione:** approssimare le derivate parziali:
```
∂f/∂x [x,y] ≈ f(x+1, y) - f(x, y)
∂f/∂y [x,y] ≈ f(x, y+1) - f(x, y)
```

Il **gradiente** di un'immagine è dato da:
```
∇f = [∂f/∂x]
     [∂f/∂y]
```

**Intensità dell'edge** (misurata dalla magnitudine del gradiente):
```
||∇f|| = sqrt((∂f/∂x)² + (∂f/∂y)²)
```

**Direzione del gradiente:**
```
θ = tan⁻¹(∂f/∂y / ∂f/∂x)
```

Il gradiente indica la **direzione di massima variazione di intensità**:
- Magnitudine alta → edge forte
- Direzione del vettore → direzione dell'edge (normale ai contorni)

### Operatori di Gradient

Diversi operatori approssimano il gradiente con kernel diversi:

| Operatore | Descrizione | Caratteristiche |
|-----------|------------|-----------------|
| **Roberts** | Misura il gradiente lungo assi ruotati di 45° | Molto sensibile al rumore |
| **Prewitt** | Filtro 3×3 meno sensibile a variazioni di luce e rumore | Calcolo del gradiente lungo una direzione e smoothing nella direzione ortogonale |
| **Sobel** | Peso maggiore al pixel centrale | Più robusto a rumore; molto usato in pratica |

**Kernel Sobel:**
```
Sx = 1/8 [1  0  -1]    Sy = 1/8 [1  2  1]
         [2  0  -2]           [0  0  0]
         [1  0  -1]           [-1 -2 -1]
```

Magnitudine: ||∇f|| = sqrt(Sx² + Sy²)
Direzione: arctan(Sy / Sx)

---

## 9 Filtraggio nel Dominio della Frequenza

### Teoria di Fourier

La **teoria di Fourier** ci insegna che ogni segnale può essere **scomposto in una serie** (eventualmente infinita) di funzioni seno e coseno di **varie frequenze**.

Questo tipo di rappresentazione consente di **valutare la velocità di cambiamento** del segnale (immagine) in **diverse direzioni**.

Un segnale generico può essere rappresentato come:

```
A · sin(ωx + φ)

dove:
A  = ampiezza
ω  = frequenza
φ  = fase
```

### Trasformata di Fourier 1D

Una funzione (segnale, immagine) viene rappresentata in **relazione alle frequenze** che lo caratterizzano e **non in termini spaziali** (o temporali).

```
f(x) → [FFT] → F(ω) = A·sin(ωx + φ)
```

Per ogni valore di ω dobbiamo rappresentare il corrispondente valore di **magnitudine (A)** e **fase (φ)**:
```
F(ω) è una funzione a valori complessi.

F(ω) = R(ω) + iI(ω)

A = ±√[R(ω)² + I(ω)²]   (magnitudine)
φ = tan⁻¹(I(ω) / R(ω))  (fase)
```

### 2D Fourier Transform

Le **immagini sono segnali 2D discreti**, quindi si adopta in questo caso la **2D discreta FT** (immagine N × N):

```
F(kx, ky) = (1/N) Σ_{x=0}^{N-1} Σ_{y=0}^{N-1} f(x,y) e^{-i2π(kx·x+ky·y)/N}

dove kx, ky rappresentano i valori discreti di frequenza
```

**Spettro di frequenza:** mostra il contenuto frequenziale dell'immagine. Frequenze basse (centro dello spettro) corrispondono alle componenti uniformi dell'immagine; frequenze alte (bordi dello spettro) corrispondono ai dettagli fini e ai bordi.

### Filtraggio nel Dominio Frequenza

Il **filtraggio dell'immagine nel dominio delle frequenze** viene normalmente eseguito:

1. Eseguendo la **FFT** dell'immagine e calcolando Spettro e Fase
2. Modificando lo **Spettro** (Passa basso, Passa alto, Passa banda) e/o la **Fase** (potenziamento/attenuazione di determinate orientazioni)
3. Eseguendo **InvFFT**

**Filtri comuni:**

| Filtro | Effetto | Formula/Caratteristiche |
|--------|--------|------------------------|
| **Passa Basso** | Riduce le alte frequenze (blur) | Preserva uniformità, riduce rumore |
| **Passa Alto** | Riduce le basse frequenze (edge emphasis) | Enfatizza dettagli e bordi |
| **Passa Banda** | Preserva una banda di frequenze | Utile per pattern con frequenze specifiche |

### Filtro di Butterworth

È un **filtro passa basso (radiale)**, nel quale la **riduzione delle alte frequenze avviene gradualmente** (e non con un brusco scalino):

```
H(ω) = 1 / (1 + (ω/ωc)^(2n))

dove:
ωc = frequenza di taglio (cutoff frequency)
n  = ordine del filtro (determina la ripidità della transizione)
```

Il filtro di Butterworth ha una risposta in frequenza molto dolce e uniforme.

### Filtro Angolare Passa Banda

Un **filtro passa banda (angolare)** consente di **preservare/rimuovere** dall'immagine **dettagli caratterizzati da ben definite orientazioni**.

```
H(φ) = { cos²(π(φ - φc)/(2φBW))    se |φ - φc| < φBW
       { 0,                          altrimenti
```

Dove:
- **φc:** orientazione centrale
- **φBW:** larghezza di banda angolare

---

## 10 Glossario

| Termine | Definizione | Esempio |
|---------|------------|---------|
| **Pixel (Picture Element)** | Unita minima di un'immagine digitale, rappresentato da un valore di intensita | Una foto 1920x1080 ha 1920x1080 = 2,073,600 pixel |
| **Campionamento (Sampling)** | Acquisizione di campioni del segnale analogico a intervalli uniformi su griglia w x h | Griglia di punti su una scena continua |
| **Quantizzazione (Quantization)** | Approssimazione di ciascun campione con uno dei valori in un range predefinito | Valori [0, 255] per 8-bit unsigned |
| **Immagine Grayscale** | Immagine in scala di grigi con singolo canale di intensita | Fotografia in bianco e nero |
| **Immagine RGB** | Immagine a colori con tre canali indipendenti (Red, Green, Blue) | Fotografia digitale standard |
| **Sintesi Additiva** | Colori creati come somma di luce rossa, verde e blu | Monitor, fotocamere digitali |
| **Sintesi Sottrattiva** | Colori creati sottraendo luce mediante filtri (CMY) | Pittura, stampa, pellicola fotografica |
| **Spazio HSV** | Spazio colore con tinta (Hue), saturazione, valore/luminosita | Piu intuitivo per selezione colori |
| **Spazio YCbCr** | Spazio colore con Luma (Y) e componenti cromatiche (Cb, Cr) | Trasmissione digitale di immagini, video compression |
| **Kernel (Maschera)** | Matrice di pesi usata nel filtraggio per combinare pixel vicini | Kernel 3x3 per blur, sharpening, edge detection |
| **Convoluzione** | Operazione matematica tra immagine e kernel per filtraggio | Filtraggio digitale, applicazione di effetti |
| **Gradiente** | Vettore che indica direzione e magnitudine di massima variazione di intensita | Usato per edge detection e estrazione feature |
| **Edge (Bordo)** | Zona dell'immagine con repentina variazione di intensita | Contorni di oggetti, transizioni brusche |
| **Trasformata di Fourier (FFT)** | Decomposizione di un'immagine in componenti frequenziali | Analisi del contenuto frequenziale |
| **Passa Basso** | Filtro che riduce alte frequenze (dettagli, rumore) | Blur, riduzione rumore |
| **Passa Alto** | Filtro che riduce basse frequenze (uniformita) | Edge detection, sharpening |
| **Filtro Gaussiano** | Kernel passa basso basato su funzione gaussiana | Blur smooth, preprocessing feature extraction |
| **Operatore Sobel** | Operatore per calcolo del gradiente usando kernel specifici | Edge detection robusto a rumore |
| **Magnitudine del Gradiente** | Ampiezza del vettore gradiente, indica intensita dell'edge | Valori alti indicano bordi forti |
| **Fase di Fourier** | Informazione di fase nella trasformata di Fourier | Determina posizione e orientazione dei dettagli |

---

## 11 Domande Tipiche d'Esame

**Q1: Spiega il processo di digitalizzazione di un'immagine continua. Quali sono i due step fondamentali?**

A: La digitalizzazione richiede due processi: (1) Campionamento: acquisizione di campioni del segnale analogico su una griglia uniforme w × h, che converte da infiniti punti a valori discreti; (2) Quantizzazione: approssimazione di ciascun campione con uno dei valori in un range predefinito (es. [0, 255] per 8-bit). Insieme, questi convertono un'immagine continua in una matrice digitale discreta.

**Q2: Qual è la differenza tra RGB e YCbCr? Perché YCbCr è preferito per la trasmissione?**

A: RGB è semplice e intuitivo (3 canali indipendenti) ma contiene ridondanza; l'occhio umano è sensibile a luminosità più che a cromaticità. YCbCr separa Luma (Y, luminosità) da componenti cromatiche (Cb, Cr). Questo permette di trasmettere Y ad alta risoluzione e Cb/Cr a risoluzione ridotta, diminuendo significativamente la banda necessaria senza perdita percettiva (sfruttando la sensibilità umana).

**Q3: Cosa è un kernel nel contesto del filtraggio? Come kernel diversi producono effetti diversi?**

A: Un kernel è una matrice di pesi usata nel filtraggio per combinare linearmente i valori dei pixel vicini. La convoluzione applica il kernel ad ogni posizione dell'immagine: g(x,y) = sum(h(u,v) * f(x-u, y-v)). Kernel diversi (con pesi diversi) producono effetti diversi: blur kernel con pesi uniformi sfoca l'immagine; sharpening kernel amplifica il centro ed è negativo ai bordi; gradient kernel calcola derivate spaziali.

**Q4: Spiega come il gradiente di un'immagine viene utilizzato per l'edge detection.**

A: Un edge è una zona di repentina variazione di intensità. Il gradiente ∇f = [∂f/∂x, ∂f/∂y] indica direzione e magnitudine di massima variazione. La magnitudine ||∇f|| = sqrt((∂f/∂x)² + (∂f/∂y)²) misura l'intensità dell'edge: valori alti indicano bordi forti, valori bassi indicano zone omogenee. Operatori come Sobel approssimano il gradiente con kernel 3×3 specifici ed sono robusti al rumore.

**Q5: Qual è il vantaggio del filtraggio nel dominio frequenziale rispetto al dominio spaziale?**

A: Nel dominio frequenziale (usando FFT), il filtraggio diventa una semplice moltiplicazione nel dominio trasformato: filtrare nel dominio spaziale (convoluzione) è equivalente a moltiplicare nel dominio frequenziale. Questo è più efficiente computazionalmente per kernel grandi. Inoltre, il dominio frequenziale fornisce intuizione: passa-basso riduce alte frequenze (blur, noise), passa-alto enfatizza dettagli (edge), passa-banda isola pattern a frequenze specifiche. Il controllo frequenziale è più naturale per alcuni problemi (noise reduction, pattern filtering).

---

## 12 Riferimenti e Risorse

### Testi Fondamentali

- Gonzalez, R. C., & Woods, R. E. (2008). *Elaborazione digitale delle immagini* (3rd ed.). Prentice Hall.
  - Capitoli 2-3: Immagini digitali, sampling, quantizzazione, spazi colore
  - Capitoli 4-5: Filtraggio, convoluzione, edge detection

- Forsyth, D. A., & Ponce, J. (2012). *Computer Vision: A Modern Approach* (2nd ed.). Pearson.
  - Capitoli 2-3: Rappresentazione digitale, spazi colore

- Kaehler, A., & Bradski, G. (2017). *Learning OpenCV 3: Computer Vision in C++ with the OpenCV Library*. O'Reilly Media.
  - Capitoli 4-7: Filtraggio, kernel, edge detection

- Elgendy, M. (2020). *Deep Learning for Vision Systems*. Manning Publications.
  - Capitolo 2: Image processing fundamentals

### Riferimenti Specializzati

- Proakis, J. G., & Manolakis, D. G. (2006). *Digital Signal Processing* (4th ed.). Pearson.
  - Trasformata di Fourier discreta, FFT, filtraggio frequenziale

- Bracewell, R. N. (2000). *The Fourier Transform and Its Applications* (3rd ed.). McGraw-Hill.
  - Teoria di Fourier, trasformate 1D e 2D

### Standard Scientifici

- IEEE Transactions on Image Processing
- IEEE Transactions on Pattern Analysis and Machine Intelligence
- Image and Vision Computing Journal

### Risorse Online

- OpenCV Documentation: https://docs.opencv.org/ (kernel, filtraggio, edge detection)
- BIOLAB UniBo: http://biolab.csr.unibo.it/ (research papers, datasets)
- Scikit-Image: https://scikit-image.org/ (Python library per image processing)

---

**Prossima lezione:** Lezione 03 - Segmentazione (Mean Shift, color analysis, morphological operations)
