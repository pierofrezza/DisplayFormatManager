# DisplayFormatManager 1.0.0

![DisplayFormatManager Pro 1.0.0](../../main/assets/screenshot-it.png)

**12 agosto 2026 · macOS 13 Ventura o successivo · Mac con Apple Silicon**

## Perché nasce DisplayFormatManager

DisplayFormatManager nasce da un problema molto concreto: il comportamento non sempre prevedibile di macOS nella gestione dei formati video dei display esterni.

In determinate configurazioni, soprattutto quando entrano in gioco campionamento, HDR, scaling e frequenza di aggiornamento, macOS può scegliere un formato diverso da quello atteso, con effetti visibili come banding o cambiamenti apparentemente inspiegabili nella qualità dell’immagine.

In attesa che Apple risolva definitivamente questi comportamenti, ho pensato di **metterci una toppa**.

Da qui è nato DisplayFormatManager: inizialmente come strumento per capire cosa stesse realmente facendo macOS e scegliere esplicitamente il formato desiderato; poi, durante lo sviluppo, il progetto è cresciuto fino a diventare una vera utility per il controllo dei display esterni.

## Funzioni comuni

DisplayFormatManager e DisplayFormatManager Pro condividono un insieme di funzioni pensate per rendere più visibile e controllabile ciò che macOS sta realmente facendo con un display esterno.

Entrambe le edizioni permettono di:

- visualizzare il formato video e il campionamento realmente utilizzati dal display;
- analizzare risoluzione, scaling e frequenza di aggiornamento;
- visualizzare direttamente nella sidebar la **connessione fisica del display**, ad esempio HDMI o DisplayPort;
- selezionare tra i formati compatibili disponibili nell’ambito supportato dall’edizione utilizzata;
- applicare temporaneamente una modifica e decidere se mantenerla;
- ripristinare automaticamente la configurazione precedente se la modifica non viene confermata;
- creare e gestire **profili persistenti**;
- riconoscere il display associato a un profilo ed evitare applicazioni automatiche quando il display non è disponibile o non può essere identificato in modo sufficientemente sicuro;
- utilizzare una **Test Card integrata** per controllare direttamente la resa della configurazione applicata;
- esportare un **report** con i dati rilevati dall’app, limitatamente alle informazioni e alle funzioni disponibili nell’edizione utilizzata;
- organizzare le informazioni attraverso **sezioni collassabili**, mantenendo leggibile l’interfaccia anche quando il display espone numerose modalità.

L’obiettivo non è sostituire le impostazioni Display di macOS, ma offrire un livello di controllo in più proprio nei casi in cui la selezione automatica del sistema non produce il risultato desiderato.

## DisplayFormatManager — Base

La versione Base costituisce il nucleo di DisplayFormatManager ed è pensata soprattutto per analizzare e correggere il formato utilizzato da macOS **all’interno del timing attualmente attivo**.

Permette di:

- vedere quale formato e quale campionamento macOS sta realmente utilizzando;
- scegliere tra i formati compatibili disponibili per il timing corrente;
- intervenire sulla configurazione supportata senza cambiare la natura del timing attivo;
- creare profili persistenti locali e mantenerli nel tempo;
- sospendere e riattivare i profili senza necessariamente eliminarli;
- visualizzare Adaptive Sync / VRR quando è attivo;
- utilizzare Test Card e report per verificare e documentare la configurazione del display.

La Base **non effettua il passaggio tra SDR e HDR**.

Se il timing attivo è SDR, le modifiche restano nell’ambito SDR; se il timing attivo è HDR, restano nell’ambito HDR.

Allo stesso modo, Adaptive Sync / VRR può essere rilevato e mostrato quando è in uso, ma **non viene controllato direttamente dalla versione Base**.

I profili persistenti creati con la Base rimangono **confinati all’interno dell’app**: possono essere creati, attivati, sospesi, riattivati e gestiti localmente, ma non possono essere esportati o importati.

## DisplayFormatManager Pro

**DisplayFormatManager Pro include tutto ciò che offre la versione Base** e aggiunge un livello di controllo più ampio sull’intera configurazione del display.

In aggiunta alle funzioni della Base, la Pro permette di:

- lavorare anche tra timing e configurazioni differenti;
- gestire il passaggio tra **SDR e HDR**;
- controllare in modo coordinato scaling, formato, campionamento e frequenza di aggiornamento;
- filtrare le modalità di scaling tra **1× e HiDPI**;
- filtrare separatamente le modalità tra **Standard e Avanzate**;
- gestire **Adaptive Sync / VRR**, passando tra refresh fisso e variabile quando display e connessione lo consentono;
- visualizzare, tramite l’apposito controllo con l’**occhio**, informazioni più tecniche e dettagliate sul display e sulle modalità disponibili;
- memorizzare con maggiore completezza lo stato originale del display;
- ripristinare successivamente quella configurazione;
- esportare e importare i profili tramite file `.dfmprofile`;
- esportare più profili all’interno dello stesso preset;
- importare un preset multiprofilo scegliendo selettivamente quali profili acquisire.

La Pro è quindi pensata non soltanto per correggere il formato attualmente utilizzato da macOS, ma per **definire, conservare, trasferire e ripristinare una configurazione completa del display**.

### Scaling: 1×, HiDPI, Standard e Avanzate

I filtri di DisplayFormatManager Pro permettono di separare due caratteristiche differenti delle modalità disponibili.

**1× e HiDPI** distinguono il tipo di scaling utilizzato:

- **1×** identifica le modalità senza scaling HiDPI;
- **HiDPI** identifica le modalità in cui macOS utilizza il rendering ad alta densità e lo scala verso il raster del display.

Il secondo filtro distingue invece tra:

- **Standard** — modalità normalmente esposte e utilizzate da macOS;
- **Avanzate** — modalità reali e utilizzabili che DisplayFormatManager Pro riesce a rendere disponibili, ma che normalmente non vengono mostrate nelle Impostazioni Schermi di macOS.

“Avanzata” non significa quindi sperimentale o intrinsecamente rischiosa: indica semplicemente una modalità valida che si trova al di fuori della selezione normalmente presentata dall’interfaccia di macOS.

I due filtri sono indipendenti: una modalità può quindi essere, ad esempio, `HiDPI Standard`, `HiDPI Avanzata`, `1× Standard` oppure `1× Avanzata`, a seconda di ciò che il display e macOS rendono effettivamente disponibile.

## Profili persistenti e preset

Entrambe le edizioni permettono di creare profili persistenti, in modo da mantenere nel tempo una determinata configurazione del display.

Nella versione Base i profili rimangono locali all’app.

DisplayFormatManager Pro aggiunge invece la possibilità di renderli **portabili** utilizzando il formato:

`*.dfmprofile`

È possibile:

- esportare un singolo profilo;
- esportare più profili nello stesso file;
- importare preset provenienti da un’altra installazione;
- scegliere, durante l’importazione di un preset multiprofilo, quali profili importare e quali ignorare.

I profili importati vengono inizialmente mantenuti sospesi, in modo che sia l’utente a decidere quando attivarli.

Quando viene attivata la persistenza, DisplayFormatManager cerca di mantenere la configurazione scelta anche in seguito a cambiamenti operati dal sistema o alla riconnessione del display.

Nella versione Pro il ripristino dello stato originale può tenere conto della configurazione realmente presente sul display prima dell’attivazione del profilo, comprese risoluzione, scaling, stato SDR/HDR, frequenza e, quando previsto, il passaggio tra refresh fisso e Adaptive Sync.

## Modifiche sicure

Cambiare un formato video può significare chiedere al display di passare a una configurazione molto diversa da quella corrente.

Per questo DisplayFormatManager utilizza, dove previsto, un meccanismo di conferma temporizzata: la nuova configurazione viene applicata e può essere mantenuta esplicitamente; in caso contrario, l’app tenta di riportare il display allo stato precedente.

L’app evita inoltre di applicare automaticamente un profilo quando il display associato non è collegato o quando non è possibile identificarlo in modo sufficientemente sicuro.

## Test Card

Sia DisplayFormatManager sia DisplayFormatManager Pro includono una **Test Card integrata**.

Può essere utilizzata per controllare rapidamente e direttamente sul display il risultato di una configurazione, senza dover ricorrere a immagini o strumenti esterni.

È particolarmente utile durante le prove dei diversi formati per osservare la resa del display e individuare più facilmente differenze, banding o altre anomalie visive.

## Report

Entrambe le edizioni permettono di esportare un **report della configurazione rilevata**.

Il contenuto del report rispecchia naturalmente le capacità dell’edizione utilizzata: la versione Pro può includere informazioni e dettagli aggiuntivi legati alle funzioni avanzate che è in grado di analizzare e controllare.

Il report può essere utile sia per conservare una fotografia dello stato del display sia per documentare configurazioni particolari o comportamenti anomali.

## Compatibilità

DisplayFormatManager 1.0.0 richiede:

- **macOS 13 Ventura o successivo**
- **Mac con Apple Silicon**

> **Nota:** DisplayFormatManager non supporta la gestione dei display Apple, né integrati né esterni. Le funzioni di analisi e controllo sono destinate ai display esterni di terze parti compatibili.

La disponibilità concreta di formati, campionamenti, HDR, frequenze elevate e Adaptive Sync / VRR dipende dalle capacità del display, dalla connessione utilizzata e dalle modalità effettivamente esposte da macOS.

DisplayFormatManager non crea modalità non supportate dal display: permette di analizzare e controllare ciò che è realmente disponibile nella configurazione in uso.

## Ispirazione

DisplayFormatManager non è nato nel vuoto.

Durante lo sviluppo mi sono ispirato in parte a **BetterDisplay**, per il livello di controllo che può essere utile avere sui display in macOS, e in parte a **WhatCable**, per l’attenzione alle informazioni sulla connessione e su ciò che sta realmente accadendo tra Mac e display.

DisplayFormatManager è diventato, in un certo senso, un piccolo compendio di queste due idee: riunire informazioni sul collegamento e controllo del formato in un unico strumento, aggiungendo profili, persistenza, Test Card, report e le altre funzioni sviluppate durante il progetto.

Il tutto con l’obiettivo di mantenere un’interfaccia il più possibile **chiara e amichevole**, anche quando dietro le quinte le informazioni da gestire sono decisamente meno amichevoli. 😁

## Distribuzione

Le applicazioni Base e Pro sono:

- firmate con certificato Apple Developer ID;
- compilate con Hardened Runtime;
- notarizzate da Apple;
- distribuite attraverso DMG anch’essi firmati e notarizzati.

Questo permette a Gatekeeper di verificarne autenticità e integrità prima dell’esecuzione.

## Più di due mesi per arrivare alla 1.0

Questa prima versione stabile arriva dopo **oltre due mesi di sviluppo, analisi, esperimenti e collaudi**.

Una parte consistente del lavoro non è stata semplicemente aggiungere funzioni, ma capire cosa macOS stesse realmente facendo dietro le quinte: confrontare formati, osservare il comportamento di display e connessioni differenti, verificare cambi di sampling, scaling, HDR, refresh e Adaptive Sync e, soprattutto, assicurarsi che ogni modifica potesse essere applicata e annullata senza lasciare il display in uno stato incoerente.

Molte delle situazioni affrontate durante lo sviluppo normalmente rimangono completamente nascoste dietro le impostazioni standard del sistema.

DisplayFormatManager nasce da un’esigenza personale, ma l’idea è che possa essere utile anche a chi si è trovato almeno una volta davanti a un monitor perfettamente funzionante chiedendosi:

**«Perché oggi macOS ha deciso di farlo funzionare diversamente da ieri?»**

Se Apple nel frattempo sistemerà definitivamente il problema, tanto meglio.

Nel frattempo, c’è DisplayFormatManager. 😁

## Feedback

Display, adattatori, dock e connessioni possono produrre combinazioni molto diverse tra loro.

Se incontri un comportamento particolare, una configurazione insolita o qualcosa che ritieni possa essere migliorato, puoi segnalarlo tramite le **GitHub Issues** del progetto.
