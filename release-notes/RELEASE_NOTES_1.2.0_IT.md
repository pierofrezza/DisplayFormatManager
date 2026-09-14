# DisplayFormatManager 1.2.0

**14 settembre 2026 · macOS 13 Ventura o successivo · Mac con Apple Silicon**

DisplayFormatManager 1.2.0 amplia in modo significativo la diagnostica della connessione video e migliora il riconoscimento dei formati HDR, con particolare attenzione a **Dolby Vision**, **DisplayPort**, **banda del collegamento** e **Display Stream Compression (DSC)**.

Questa versione rende inoltre più chiara la distinzione tra il formato trasportato sul collegamento DisplayPort e quello effettivamente ricevuto dal display a valle, così da descrivere meglio le catene che includono adattatori, dock e bridge DisplayPort → HDMI.

## Supporto Dolby Vision migliorato

Dolby Vision e Dolby Vision Low Latency vengono ora riconosciuti correttamente come formati HDR.

La presentazione delle modalità è stata affinata per riportare in modo più coerente:

- famiglia del segnale;
- profondità colore;
- range;
- stato HDR;
- variante Dolby Vision, quando identificabile.

La classificazione rimane basata su ciò che macOS e il display espongono effettivamente, senza trasformare una modalità in un formato diverso da quello rilevato.

## Catalogo HDR/SDR più preciso

È stata migliorata l'associazione tra modalità SDR e HDR che condividono gli stessi timing.

Il confronto tiene ora maggiormente conto di raster, frequenza e natura Fixed/Adaptive della modalità, riducendo il rischio di associare tra loro configurazioni appartenenti a categorie differenti.

Quando entrambe le famiglie sono disponibili, le schede vengono mostrate nell'ordine:

`SDR | HDR`

e la scheda attiva segue la categoria della configurazione realmente in uso.

## Diagnostica DisplayPort ampliata

Il report Pro include informazioni più dettagliate sul collegamento DisplayPort realmente negoziato.

Quando disponibili, vengono riportati:

- classe del link;
- numero di lane;
- velocità per lane;
- codifica del collegamento;
- banda lorda e banda utile;
- utilizzo stimato;
- margine residuo rispetto alla capacità disponibile.

Queste informazioni descrivono il collegamento effettivamente attivo e restano separate dalle sole capacità teoriche dichiarate dall'hardware.

## Rilevamento live del Display Stream Compression

DisplayFormatManager può ora rilevare se **Display Stream Compression (DSC)** è realmente attivo sul collegamento corrente.

Il rilevamento è basato sullo stato live della connessione e non su una semplice stima di banda.

Quando DSC è attivo e i dati risultano validi, il report Pro può mostrare:

- versione DSC;
- profondità colore in ingresso;
- formato cromatico in ingresso;
- bit per pixel sorgente e target;
- dimensioni dell'immagine;
- dimensioni delle slice;
- chunk size;
- rapporto di compressione;
- banda stimata dopo DSC;
- utilizzo e margine del collegamento.

Quando DSC non è attivo, il report lo indica esplicitamente.

L'interfaccia può inoltre mostrare l'indicazione `· DSC` quando la compressione risulta effettivamente attiva.

## Formato DisplayPort sorgente distinto dal formato finale

Nelle catene che includono un bridge o un adattatore, il formato trasportato sul lato DisplayPort può non coincidere con quello ricevuto dal display finale.

DisplayFormatManager 1.2.0 rende visibile questa distinzione nel report Pro.

Per esempio, una catena può trasportare a monte un segnale DisplayPort YCbCr 4:4:4 a 10 bit con DSC e consegnare a valle un segnale HDMI YCbCr 4:2:0 a 10 bit.

Quando disponibili, il report separa quindi:

- formato finale/downstream;
- formato del link DisplayPort;
- profondità colore;
- range;
- stato PQ/HDR;
- colorimetria del link.

Questa distinzione permette di rendere visibili eventuali conversioni effettuate da adattatori o bridge invece di attribuirle direttamente al display o al Mac.

## Diagnostica HDMI ampliata

La lettura delle capacità HDMI dichiarate tramite EDID è stata ampliata.

Il report Pro può includere, quando presenti, informazioni relative a:

- HDMI VSDB;
- HDMI Forum VSDB;
- limite TMDS;
- SCDC;
- capacità FRL.

Queste informazioni descrivono ciò che il display o il dispositivo a valle dichiara di supportare e rimangono distinte dallo stato live del collegamento.

## Miglioramenti dell'interfaccia

Sono state rifinite diverse parti dell'interfaccia per rendere più leggibili modalità e informazioni tecniche.

In particolare:

- le card delle modalità Scaling si adattano meglio allo spazio disponibile;
- i badge possono andare a capo in modo più naturale quando lo spazio è ridotto;
- alcune etichette DisplayPort sono state rese più precise;
- l'indicazione DSC usa un separatore visivo più coerente con il resto dell'interfaccia;
- l'ordine delle schede HDR/SDR è stato uniformato a `SDR | HDR`.

## Report Pro più veloci

La raccolta delle informazioni live necessarie al report Pro è stata ottimizzata.

I dati della connessione vengono riutilizzati all'interno della stessa generazione del report evitando interrogazioni duplicate non necessarie.

Il contenuto del report rimane invariato, ma il tempo necessario per generarlo e salvarlo è stato sensibilmente ridotto.

## Base e Pro

La distinzione tra le due edizioni rimane quella introdotta con le versioni precedenti.

**DisplayFormatManager Base** continua a concentrarsi sull'analisi e sul controllo del formato nell'ambito consentito dall'edizione, con profili locali, Test Card, report e rollback protetto.

**DisplayFormatManager Pro** aggiunge il controllo tra timing e configurazioni differenti, il passaggio SDR/HDR, le funzioni avanzate di Scaling, Adaptive Sync / VRR, l'importazione/esportazione dei profili `.dfmprofile` e la diagnostica tecnica più approfondita della connessione.

## Compatibilità

DisplayFormatManager 1.2.0 richiede:

- **macOS 13 Ventura o successivo**
- **Mac con Apple Silicon**

La disponibilità concreta di formati, sampling, HDR, Dolby Vision, profondità colore, frequenze elevate, Adaptive Sync / VRR e DSC dipende dal display, dalla connessione utilizzata, da eventuali adattatori o dock e da ciò che macOS rende effettivamente disponibile.

DisplayFormatManager non crea modalità non supportate dal display.

> **Nota:** DisplayFormatManager non supporta la gestione dei display Apple, né integrati né esterni. Le funzioni di analisi e controllo sono destinate ai display esterni di terze parti compatibili.

## Distribuzione

Le applicazioni Base e Pro sono:

- firmate con certificato Apple Developer ID;
- compilate con Hardened Runtime;
- notarizzate da Apple;
- distribuite tramite DMG anch'essi firmati e notarizzati.

I checksum SHA-256 ufficiali dei DMG sono inclusi nel file `SHA256SUMS.txt` allegato alla release.

## Feedback

Display, adattatori, dock e connessioni possono produrre moltissime combinazioni differenti.

Se incontri un comportamento particolare, una configurazione insolita o qualcosa che ritieni possa essere migliorato, puoi segnalarlo tramite le **GitHub Issues** del progetto.
