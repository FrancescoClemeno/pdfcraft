# Fork FrancescoClemeno/pdfcraft

Questo fork è PdfCraft ([storytold/pdfcraft](https://github.com/storytold/pdfcraft)) più alcune
modifiche personali. Si tiene aggiornato da solo e pubblica le proprie build, così l'app che scarichi
resta aggiornata **e** mantiene le modifiche.

## Modifiche del fork

**Copia immagine** è stata accettata in PdfCraft originale (storytold/pdfcraft#1363) e ora arriva
da lì: strumento *Seleziona*, clic su un'immagine e ⌘C / Ctrl+C la copia come file nel formato
originale (JPEG così com'è, altrimenti PNG); tasto destro ▸ *Copia immagine* copia i pixel. Il
fork non ha più modifiche al codice dell'app.

Restano solo le parti del fork:

- *Aiuto ▸ Verifica aggiornamenti* guarda le release di **questo** fork
  (`crates/ui-egui/src/updates.rs`, `apps/pdfcraft/src/updates.rs`), che vengono ricompilate qui.
- I workflow `fork-sync.yml` e `fork-release.yml`, lo script `.github/fork/rehash-catalogs.sh` e
  questo file.

Per proporre altre funzioni all'originale si apre una PR da un **branch separato** con solo
quella modifica, mai dal `main` del fork (che contiene le parti qui sopra).

## Come resta aggiornato

1. **`.github/workflows/fork-sync.yml`** (ogni giorno, o a mano da *Actions ▸ Fork sync ▸ Run workflow*)
   unisce `main` di storytold/pdfcraft nel `main` del fork con un merge commit: le modifiche del fork
   restano. Se c'è un conflitto non spinge nulla e apre una issue con etichetta `fork-sync` che dice
   quali file sistemare.
2. Se la versione in `Cargo.toml` è nuova (non esiste ancora la release `vX.Y.Z` nel fork), avvia
   **`.github/workflows/fork-release.yml`**, che compila l'installer Windows x64 (`.msi`) e la versione
   portatile, e pubblica la release `vX.Y.Z` del fork.
3. L'app, con *Aiuto ▸ Verifica aggiornamenti*, trova quella release e apre la pagina per scaricarla
   (PdfCraft non installa nulla da solo).

Per ricompilare la stessa versione dopo aver cambiato il codice del fork: *Actions ▸ Fork sync ▸ Run
workflow* con "Also build and publish a fork release now" spuntato (o *Actions ▸ Fork release ▸ Run
workflow*). La release con lo stesso numero viene sostituita; l'app non la segnala come nuova, va
scaricata a mano.

## Configurazione una tantum (su GitHub)

1. **Abilitare le Actions** nel fork: scheda *Actions* ▸ "I understand my workflows, go ahead and
   enable them". I workflow pianificati dei fork sono spenti finché non lo fai.
2. **Token per la sincronizzazione** (consigliato): il token predefinito non può spingere modifiche
   ai file in `.github/workflows`, quindi il sync si fermerebbe la prima volta che l'originale ne
   cambia uno. Crea un *fine-grained personal access token* (Settings ▸ Developer settings), solo
   per questo repository, con permessi *Contents*, *Workflows*, *Actions* e *Issues* in lettura e
   scrittura, e salvalo come secret del repository `FORK_SYNC_TOKEN` (*Settings ▸ Secrets and
   variables ▸ Actions*).
3. Prima release: *Actions ▸ Fork sync ▸ Run workflow* con la casella della release spuntata.

GitHub sospende i workflow pianificati dopo 60 giorni senza attività nel repository: se succede,
riattivali dalla scheda *Actions*.

## Primo avvio (build non firmate)

La build del fork non ha un certificato a pagamento:

- **Windows**: SmartScreen ▸ *Ulteriori informazioni* ▸ *Esegui comunque*.

Per avere anche macOS o Linux, copia i job corrispondenti da `.github/workflows/release.yml`
in `fork-release.yml` (senza `environment: release` e con un runner standard, es. `macos-15`).

## Aggiungere altre modifiche

- Lavora su un branch, poi unisci in `main`; il sync quotidiano le conserva.
- Per ridurre i conflitti con l'originale, preferisci file nuovi e modifiche piccole e localizzate
  ai file esistenti, e aggiungi le traduzioni accanto a voci esistenti (non in fondo ai file
  `crates/ui-egui/src/i18n/*.tsv`, dove anche l'originale aggiunge righe).
- Ogni testo `tl!("…")` nuovo va tradotto in tutti i cataloghi controllati dai test (vedi
  `crates/ui-egui/src/i18n/mod.rs`), altrimenti `cargo test` fallisce.
- Aggiorna la tabella qui sopra.
