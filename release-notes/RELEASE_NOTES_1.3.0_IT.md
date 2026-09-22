# DisplayFormatManager 1.3.0

**22 settembre 2026 · build 183 · macOS 13 Ventura o successivo · Mac con Apple Silicon**

DisplayFormatManager 1.3.0 completa un importante ciclo di lavoro sulla diagnostica del percorso video e sul controllo dei display esterni, con un nuovo report tecnico interattivo, lettura DPCD, analisi EDID/CTA più profonda, rilevamento DSC consolidato e supporto HDMI-CEC sui percorsi compatibili.

La release mantiene un approccio conservativo: le informazioni a basso livello vengono mostrate solo quando possono essere associate in modo sufficientemente sicuro al display selezionato. Le diagnostiche restano read-only dove previsto e DFM evita di promuovere stati o rendere disponibili controlli quando le evidenze non sono sufficienti.

## Nuovo report tecnico

Il report è stato completamente riorganizzato.

Entrambe le edizioni possono ora:

- aprire il report in una **sheet espandibile/comprimibile**;
- espandere o comprimere tutte le sezioni;
- salvare il report in formato TXT;
- ottenere sheet e TXT dalla stessa sorgente dati logica, evitando divergenze tra le due rappresentazioni.

La presentazione EDID è stata inoltre riordinata in una struttura più leggibile: riepilogo, capacità dichiarate, HDMI/CTA, timing, diagnostica avanzata e raw finale.

## Diagnostica DPCD

DisplayFormatManager Pro può leggere informazioni DPCD reali sui percorsi DisplayPort compatibili.

Quando disponibili e associabili in modo sicuro al display, il report può includere:

- revisione DPCD;
- receiver capabilities;
- link rate massimo;
- numero massimo di lane;
- configurazione link corrente;
- Enhanced Framing;
- stato CR / EQ / SYMBOL_LOCK delle lane;
- interlane alignment;
- Extended Receiver Capabilities.

La lettura è isolata, read-only e fail-closed.

## Display Stream Compression e compatibilità macOS 26.7

Il rilevamento live del **Display Stream Compression (DSC)** è stato consolidato anche dopo i cambiamenti osservati nelle API IOAV di macOS 26.7.

Quando DSC è attivo e i dati risultano validi, il report Pro può mostrare:

- versione DSC;
- profondità colore e formato cromatico in ingresso;
- bit per pixel sorgente e target;
- dimensioni immagine e slice;
- chunk size;
- rapporto di compressione;
- banda stimata dopo DSC;
- utilizzo e margine del collegamento.

Il rilevamento continua a basarsi sullo stato live della connessione, non su una semplice stima di banda.

## EDID e CTA più completi

Il recupero EDID è stato ampliato sui percorsi DisplayPort e DisplayPort → HDMI, mantenendo il percorso IORegistry come prima scelta e aggiungendo un fallback read-only tramite IOAV / DCPAVServiceProxy quando necessario.

Il parser strutturato può interpretare, quando presenti:

- EDID 1.3 e 1.4;
- timing dettagliati, standard ed established;
- Display Range Limits;
- CTA Video Data Block;
- Audio Data Block;
- Speaker Allocation;
- HDR Static Metadata;
- Colorimetry;
- Video Capability;
- YCbCr 4:2:0 Capability Map;
- HDMI VSDB;
- HDMI Forum VSDB;
- TMDS, SCDC e FRL dichiarati;
- AMD FreeSync VSDB v1.

I blocchi non standardizzati o non sufficientemente documentati restano disponibili come raw senza attribuire loro significati non verificati.

## HDMI-CEC

La gestione HDMI-CEC è stata consolidata sui percorsi compatibili che espongono CEC-over-AUX.

Quando DFM riesce a verificare un endpoint operativo, può rendere disponibile il controllo **Power** per mettere il display in stand-by e riattivarlo.

La logica gestisce in modo conservativo:

- rilevamento della disponibilità CEC;
- stato Power;
- standby e wake;
- hot-plug;
- wake avviati da DFM o dall'esterno;
- recupero della sorgente e dell'indirizzo logico quando necessario.

Il controllo Power non viene mostrato quando il percorso o il sink non forniscono evidenze sufficienti di operatività CEC.

## HDR e MPDisplay

È stata rimossa la dipendenza passiva da MPDisplay usata in precedenza per il normale rilevamento HDR.

Il supporto HDR viene ora ricavato dal catalogo modalità e lo stato HDR attivo/disattivo dalla modalità corrente. Questo ha eliminato il warning `bucketizeDisplayModes` osservato con alcune configurazioni senza reintrodurre una dipendenza passiva da MPDisplay.

## Interfaccia e localizzazione

Sono stati rifiniti:

- layout e leggibilità della sheet tecnica;
- colonne proprietà/valore dinamiche per sezione;
- indicatori e terminologia DisplayPort;
- presentazione EDID/CTA/HDMI;
- stringhe italiane e inglesi del report;
- coerenza tra report visuale ed esportazione TXT.

Il logging automatico di sessione usato durante lo sviluppo è stato rimosso dal percorso release.

## Verifica release

La 1.3.0 build 183 è stata sottoposta a regression test release-equivalent su:

- HP U28 4K HDR diretto USB-C → DisplayPort;
- HP U28 4K HDR via Apple A2119;
- HP U28 4K HDR via Belkin AVC003;
- Samsung HDMI via Belkin AVC003 per HDMI-CEC e controllo della regressione MPDisplay;
- edizioni Base e Pro sui percorsi principali.

Sono stati verificati startup/refresh, Test Card, scaling, rollback, HDR/SDR, DPCD, DSC, EDID/CTA/HDMI/FreeSync, coerenza sheet ↔ TXT, salvataggio report, assenza del session log automatico e comportamento HDMI-CEC sui percorsi validati.

## Compatibilità

DisplayFormatManager 1.3.0 richiede:

- **macOS 13 Ventura o successivo**
- **Mac con Apple Silicon**
- **display esterno di terze parti compatibile**

La disponibilità concreta di formati, sampling, HDR, Dolby Vision, profondità colore, frequenze elevate, Adaptive Sync / VRR, DPCD, DSC e HDMI-CEC dipende dal display, dalla connessione utilizzata, dagli adattatori o dock presenti e da ciò che macOS espone per quella specifica configurazione.

DisplayFormatManager non crea modalità non supportate dal display.

> **Nota:** DisplayFormatManager non supporta la gestione dei display Apple, né integrati né esterni.

## Distribuzione

Le applicazioni Base e Pro sono:

- firmate con certificato Apple Developer ID;
- compilate con Hardened Runtime;
- notarizzate da Apple;
- distribuite tramite DMG anch'essi firmati, notarizzati e stapled.

Checksum SHA-256 ufficiali:

```text
57ce02ae8d2b69aa30e548479f24034795b473487821744b718ceb94d37d31c2  DisplayFormatManager-1.3.0.dmg
9da34f0a9b2d67be2b0558d4c25a81b21c3dc91da58c7a6f26d5a1c667c9fa22  DisplayFormatManager-Pro-1.3.0.dmg
```

## Feedback

Display, adattatori, dock e percorsi di connessione possono produrre moltissime combinazioni differenti.

Se incontri un comportamento particolare, una configurazione insolita o qualcosa che ritieni possa essere migliorato, puoi segnalarlo tramite le **GitHub Issues** del progetto.
