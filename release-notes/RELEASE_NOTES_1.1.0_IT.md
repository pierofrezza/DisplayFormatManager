# DisplayFormatManager 1.1.0

**21 agosto 2026 · macOS 13 Ventura o successivo · Mac con Apple Silicon**

DisplayFormatManager 1.1.0 è il primo aggiornamento importante dopo la 1.0.0 pubblica e raccoglie anche il lavoro svolto nelle build intermedie della 1.0.1.

Questa versione interviene soprattutto su quattro aree: **sincronizzazione HDR/SDR con macOS**, **affidabilità dei cambi di formato e Scaling**, **Test Card HDR/EDR** e **nuova gestione dei profili persistenti**.

## HDR/SDR realmente sincronizzato con macOS

Nella versione Pro, i passaggi tra SDR e HDR non si limitano più a selezionare un formato video fisicamente compatibile con HDR.

DisplayFormatManager sincronizza ora anche il **vero stato HDR/SDR per-display di macOS**, così che il cambio effettuato dall'app venga riflesso anche dal selettore HDR nelle Impostazioni di Sistema e dalla pipeline HDR/EDR di WindowServer.

In pratica:

- SDR → HDR attiva anche lo stato HDR di macOS;
- HDR → SDR lo disattiva correttamente;
- i passaggi HDR → HDR e SDR → SDR non alterano inutilmente uno stato già corretto;
- timeout e **Ripristina** riportano il display anche allo stato HDR/SDR macOS originale;
- lo stato semantico di macOS e il formato video fisico rimangono distinti e verificabili.

Nessun valore di luminanza, headroom EDR o modello di display viene codificato in modo fisso: DisplayFormatManager utilizza ciò che macOS e il display rendono effettivamente disponibile.

## Motore formato, frequenza e Scaling più robusto

La 1.1.0 consolida il motore di applicazione introdotto e collaudato durante le build intermedie successive alla 1.0.0.

Sono stati corretti in particolare alcuni casi nei quali macOS poteva mantenere la nuova frequenza di aggiornamento ma ripristinare sampling o bit depth precedenti dopo un cambio di formato.

Quando vengono modificati contemporaneamente **formato video e Scaling sullo stesso raster**, DisplayFormatManager utilizza ora una sequenza deterministica:

1. applicazione dello Scaling tramite Core Graphics;
2. applicazione finale del formato fisico esatto;
3. verifica della configurazione realmente attiva.

Il rollback usa lo stesso principio per ripristinare in modo coordinato stato HDR/SDR, formato, frequenza e Scaling originali.

Core Graphics viene coinvolto nei cambi di formato soltanto quando è realmente necessario modificare anche la modalità Desktop/Scaling, evitando recommit che potrebbero interferire con il formato fisico appena applicato.

## Verifica tra modalità richiesta e modalità realmente attiva

DisplayFormatManager verifica ora in modo esplicito che la configurazione applicata corrisponda a quella richiesta.

Se, dopo l'assestamento del sistema, macOS ha applicato una configurazione differente:

- l'interfaccia segnala il mismatch;
- viene mostrata la configurazione effettivamente rilevata;
- **Mantieni** resta disabilitato, evitando di confermare come riuscita una modalità diversa da quella richiesta.

I report possono inoltre riportare **Target**, **Observed** e lo stato della verifica durante una transazione attiva.

## Timer di sicurezza a 30 secondi

La finestra di conferma delle modifiche protette passa da 20 a **30 secondi**.

Anche l'offerta di creazione di un nuovo profilo persistente dopo **Mantieni** rimane disponibile per 30 secondi.

Il timeout della modalità continua a eseguire il rollback automatico della configurazione originale.

## Nuova Test Card “Luminosità HDR”

La Test Card integra una nuova scheda **Luminosità HDR**, separata dai pattern SDR esistenti.

La nuova superficie utilizza rendering EDR floating-point dedicato e può visualizzare livelli superiori al bianco SDR di riferimento `1×` quando macOS rende disponibile headroom aggiuntivo.

La Test Card:

- rileva dinamicamente l'headroom EDR corrente;
- distingue il bianco SDR `1×` dai livelli EDR superiori;
- non supera il limite effettivamente disponibile;
- adatta il percorso di rendering alla famiglia del segnale corrente;
- mostra uno stato informativo quando l'headroom disponibile è `1×`, senza considerare questo valore un errore.

Le schede SDR preesistenti rimangono separate dal percorso HDR.

Sono stati inoltre corretti il layout della fascia inferiore, i margini dei controlli e alcuni warning runtime osservati durante lo sviluppo.

## Guida al campionamento migliorata

La scheda **Campionamento** della Test Card fornisce ora indicazioni contestuali per:

- RGB / YCbCr 4:4:4;
- YCbCr 4:2:2;
- YCbCr 4:2:0.

La guida descrive il tipo di perdita di dettaglio cromatico che ci si può aspettare senza trattare una singola tinta risultante come verdetto universale.

## Informazioni su connessione e frequenza più precise

Sidebar e Test Card utilizzano una presentazione più leggibile delle frequenze di aggiornamento.

Quando il valore nominale differisce da quello tecnico, DisplayFormatManager mostra entrambi, ad esempio:

`24 Hz (23.976 Hz)`

I valori che non richiedono arrotondamento restano invece compatti.

La rilevazione della connessione è stata inoltre ampliata per utilizzare, quando macOS lo espone, il **connettore finale dichiarato** — ad esempio HDMI, DisplayPort, DVI o VGA — mantenendo separata l'identità interna utilizzata per la persistenza.

Il report Pro può includere anche informazioni sul percorso della connessione, come porta sorgente, trasporto, uscita a valle e dispositivi intermedi disponibili.

## Report HDR/EDR

I report rimangono fotografie dello **stato corrente** del display e non cronologie delle modifiche effettuate.

La versione Base aggiunge lo stato HDR macOS corrente.

La versione Pro può inoltre riportare:

- supporto HDR rilevato da macOS;
- stato HDR macOS attivo/disattivo;
- headroom EDR disponibile quando applicabile;
- formato video fisico separato dallo stato HDR semantico.

Questa distinzione permette di rendere visibili anche eventuali stati misti invece di nasconderli.

## Profili persistenti ripensati

La gestione dei profili persistenti è stata profondamente rivista.

Un profilo non viene più applicato continuamente per contrastare ogni modifica manuale. Viene invece **riarmato dagli eventi principali del ciclo di vita del display e del sistema**:

- riconnessione fisica del display;
- riattivazione dopo lo stop;
- nuovo login grafico;
- riavvio di macOS;
- riattivazione esplicita del profilo.

A ogni evento il profilo applica una sola volta lo stato completo salvato. Dopo un'applicazione riuscita, eventuali modifiche manuali effettuate dall'utente restano libere per il resto della sessione.

Se il display stack non è ancora pronto, il profilo non viene considerato applicato e il LaunchAgent può ritentare successivamente.

L'associazione tra profilo e display continua a utilizzare un comportamento prudente: in caso di identità ambigua DisplayFormatManager evita di applicare deliberatamente il profilo al display sbagliato.

## Nuovi stati dei profili

I profili possono ora mostrare cinque stati distinti:

- **Attuale** — il target del profilo corrisponde alla configurazione corrente;
- **In pausa** — il profilo è abilitato, ma nella sessione corrente è presente un override manuale;
- **Sospeso** — il profilo è stato disabilitato volontariamente e non interviene agli eventi lifecycle;
- **Display non collegato** — il display associato non è attualmente disponibile;
- **Incongruente** — il profilo non può essere associato in modo sicuro e univoco a un display.

**Mantieni** non sospende più automaticamente un profilo persistente già attivo: la configurazione mantenuta diventa un override temporaneo e il profilo passa a **In pausa**.

Sono disponibili azioni esplicite per sospendere e riattivare i profili. Un profilo in pausa può essere riattivato immediatamente, terminando volontariamente l'override della sessione.

Lo stato **Incongruente** utilizza un aspetto monocromatico adattivo: nero in Light Mode e bianco in Dark Mode, con badge sempre leggibile.

I profili importati continuano a non essere applicati automaticamente senza un intervento esplicito dell'utente.

## Catalogo delle modalità dopo un cambio

Dopo che il target richiesto è stato riconosciuto, DisplayFormatManager continua per un breve periodo a leggere nuovi snapshot dello stato del display.

Questo permette a WindowServer di ripubblicare le modalità e i timing correlati dopo l'assestamento senza eseguire un secondo modeset, riavviare il timer o alterare l'ordine della transazione.

## Base e Pro

La distinzione tra le due edizioni rimane quella introdotta con la 1.0.0.

**DisplayFormatManager Base** continua a concentrarsi sull'analisi e sul controllo del formato nell'ambito consentito dall'edizione, con profili locali, Test Card, report e rollback protetto.

**DisplayFormatManager Pro** aggiunge il controllo tra timing e configurazioni differenti, il passaggio SDR/HDR, le funzioni avanzate di Scaling, Adaptive Sync / VRR e l'importazione/esportazione dei profili `.dfmprofile`.

Le nuove informazioni sullo stato HDR macOS sono disponibili anche nella Base dove applicabile; il controllo SDR/HDR rimane una funzione Pro.

## Compatibilità

DisplayFormatManager 1.1.0 richiede:

- **macOS 13 Ventura o successivo**
- **Mac con Apple Silicon**

La disponibilità concreta di formati, sampling, HDR, headroom EDR, frequenze elevate e Adaptive Sync / VRR dipende dal display, dalla connessione utilizzata e da ciò che macOS rende effettivamente disponibile.

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
