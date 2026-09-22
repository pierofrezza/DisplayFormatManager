# DisplayFormatManager 1.4.0

**23 settembre 2026 · build 191 · macOS 13 Ventura o successivo · Mac con Apple Silicon**

DisplayFormatManager 1.4.0 si concentra su un accesso più rapido allo stato dei display e alle azioni più immediate, introducendo un nuovo elemento opzionale nella barra menu di macOS, disponibile sia nella Base sia nella Pro.

La barra menu è volutamente pensata come **riepilogo e accesso rapido**, non come sostituto della finestra principale di DisplayFormatManager. Il controllo completo dei display resta nell'app.

## Barra menu di macOS

Entrambe le edizioni possono ora aggiungere opzionalmente DisplayFormatManager alla barra menu di macOS.

Il pannello può mostrare:

- display collegati;
- nome del display e riepilogo sintetico del segnale corrente;
- stato HiDPI quando attivo;
- tipo di connessione fisica, inclusi HDMI e DisplayPort;
- display compatibili attualmente in standby;
- stato dei profili persistenti;
- accesso rapido alla Test Card;
- pulsante per portare in primo piano la finestra principale dell'app.

Quando non è collegato alcun display esterno supportato, il pannello mostra uno stato vuoto dedicato invece di presentare controlli display inattivi.

## Display collegati e sincronizzazione live

La barra menu e la finestra principale condividono lo stesso stato dell'app, senza mantenere modelli display indipendenti.

Di conseguenza il pannello segue:

- collegamento e scollegamento dei display;
- avvio con un display già collegato;
- standby e wake HDMI-CEC;
- wake esterni;
- cambiamenti dello stato dei profili persistenti;
- stato delle Test Card.

Il riepilogo del display rimane volutamente compatto, lasciando alla finestra principale la selezione dei formati e la diagnostica tecnica dettagliata.

## Riaccensione rapida HDMI-CEC

Quando un display compatibile è in standby e DFM ha già verificato un endpoint HDMI-CEC operativo, la barra menu può rendere disponibile l'azione **Accendi**.

Non viene introdotta una seconda implementazione CEC: la barra menu utilizza lo stesso stato e lo stesso percorso di controllo già validati nella finestra principale.

Il controllo Power continua a non essere disponibile quando la connessione non fornisce evidenze sufficienti di un percorso CEC operativo.

## Accesso rapido alla Test Card

La barra menu include un unico menu **Test Card** con l'elenco dei display attualmente collegati.

Le Test Card restano indipendenti per display, quindi possono esserne aperte più di una contemporaneamente. Il menu riflette lo stato corrente e permette di aprire o chiudere ciascuna Test Card.

## Impostazioni barra menu e avvio al login

Nella toolbar della finestra principale è disponibile un nuovo controllo Impostazioni.

Contiene due opzioni:

- **Mostra nella barra menu**
- **Apri al login**

`Apri al login` è disponibile solo quando `Mostra nella barra menu` è attivo.

Quando l'avvio al login è abilitato, DisplayFormatManager parte come utility nella barra menu senza aprire automaticamente la finestra principale e senza rimanere visibile nel Dock. Selezionando **Apri l'app**, DFM torna alla normale presenza come applicazione e apre la finestra principale.

Disattivando la barra menu viene disattivato e deregistrato anche l'avvio al login, evitando configurazioni in cui l'app resterebbe attiva in background senza un accesso evidente.

## Rifiniture dell'interfaccia

Il pannello della barra menu include:

- icona dell'app e nome dell'edizione nella testata;
- riepilogo compatto dei display con indicatori HiDPI e connessione;
- stati dei profili persistenti coerenti con la finestra principale;
- Test Card sul lato sinistro del footer;
- **Apri l'app** sul lato destro;
- aggiornamento live dell'aspetto Light/Dark.

L'icona nella barra menu usa un simbolo dedicato al controllo del display, con un peso più marcato per renderla facilmente riconoscibile.

## Metadati del report tecnico

Il documento logico condiviso del report include ora esplicitamente:

- versione dell'app;
- numero di build.

Poiché la sheet interattiva e il TXT esportato utilizzano la stessa sorgente logica, l'informazione compare in modo coerente in entrambe le rappresentazioni.

## Verifica release

La 1.4.0 build 191 è stata sottoposta a QA finale su:

- edizioni Base e Pro;
- localizzazione italiana e inglese;
- attivazione/disattivazione della barra menu;
- persistenza delle impostazioni;
- avvio con e senza display esterno;
- sincronizzazione live dell'hot-plug;
- stato vuoto senza display;
- cambio aspetto Light/Dark;
- gestione Test Card su più display;
- presentazione dei profili persistenti;
- avvio al login senza apertura automatica della finestra e senza presenza nel Dock;
- passaggio stabile dalla modalità solo barra menu alla finestra principale;
- sincronizzazione standby/wake HDMI-CEC con Samsung HDMI via Belkin AVC003.

Il core di controllo display, CEC e diagnostica a basso livello già validato nella 1.3.0 è rimasto invariato salvo quanto strettamente necessario alla nuova presentazione nella barra menu o al lifecycle dell'app.

## Compatibilità

DisplayFormatManager 1.4.0 richiede:

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
bde34c2c56523510bfc29dade279541a7f2e1546118ca3dca5b41a3e90241613  DisplayFormatManager-1.4.0.dmg
70cf20bfa0ac95c9d8603364f062a6fa8e51bb478e91498e30d1f5d3dc081b3a  DisplayFormatManager-Pro-1.4.0.dmg
```

## Feedback

Display, adattatori, dock e percorsi di connessione possono produrre moltissime combinazioni differenti.

Se incontri un comportamento particolare, una configurazione insolita o qualcosa che ritieni possa essere migliorato, puoi segnalarlo tramite le **GitHub Issues** del progetto.
