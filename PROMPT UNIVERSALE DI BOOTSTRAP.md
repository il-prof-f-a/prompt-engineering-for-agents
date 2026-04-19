## TASK

Devi inizializzare o aggiornare il sistema di memoria persistente del progetto nella cartella `.ai/`.

L'obiettivo è permettere a modelli diversi (Claude, GPT, Gemini, modelli locali) di lavorare senza perdere contesto.

---

## ANALISI INIZIALE (OBBLIGATORIA)

1. Analizza l'intera cartella progetto
2. Identifica:
   - linguaggi utilizzati
   - struttura del codice
   - file principali
   - eventuali README o documentazione
   - ultime modifiche (se deducibili)
3. Verifica se esiste già la cartella `.ai/`

---

## DECISIONE MODALITÀ

### CASO A → `.ai/` NON ESISTE

Modalità: INIT

### CASO B → `.ai/` ESISTE

Modalità: UPDATE

---

## COMPORTAMENTO

### MODALITÀ INIT

Devi:

1. creare la cartella `.ai/`
2. generare i seguenti file:
- `.ai/project_brief.md`
- `.ai/architecture.md`
- `.ai/current_state.md`
- `.ai/decisions.md`
- `.ai/session_handoff.md`
- `.ai/tasks_todo.md`
- `.ai/model_prefs.md`

---

### MODALITÀ UPDATE

Devi:

1. leggere tutti i file `.ai/`
2. verificare coerenza con il codice
3. aggiornare SOLO se necessario
4. NON sovrascrivere informazioni valide

---

## GENERAZIONE CONTENUTI

### `.ai/project_brief.md`

Deve contenere:

- descrizione progetto
- obiettivo
- stack tecnologico
- struttura cartelle
- componenti principali

Includi anche questa sezione opzionale (se non utilizzata, lasciarla vuota):

```md
## NOTE TECNICHE (OPZIONALE)
[Inserire eventuali indicazioni manuali specifiche sul progetto]
```

---

### `.ai/architecture.md`

File critico. Deve descrivere la struttura generale del sistema in modo che un altro modello possa capire il progetto senza leggere tutta la cronologia chat.

Deve contenere, se deducibili con sufficiente certezza:

- panoramica architetturale
- sottosistemi/moduli principali
- responsabilità di ciascun sottosistema
- flussi principali del programma
- flussi dei dati
- punti di ingresso
- dipendenze interne ed esterne
- integrazioni/API/servizi
- convenzioni architetturali rilevanti
- eventuali colli di bottiglia o aree sensibili

### Regola speciale per `architecture.md`

Scrivilo o aggiornalo **solo se hai davvero capito l'architettura** dal codice e dai file disponibili.

- Se l'architettura è chiara → scrivi/aggiorna il file in modo operativo
- Se l'architettura NON è chiara o è solo parzialmente deducibile → NON inventare nulla, dichiaralo esplicitamente e chiedi spiegazioni prima di consolidare il file
- Se il file esiste già e contiene informazioni plausibili → preservale e correggi solo ciò che è verificabile

---

### `.ai/current_state.md`

Deve descrivere:

- stato attuale del progetto
- cosa funziona
- cosa non è completo
- ultime aree di lavoro (dedotte dal codice)
- riferimento sintetico a come lo stato si collega all'architettura

---

### `.ai/decisions.md`

Se vuoto:

- inizializzalo

Se esiste:

- NON inventare decisioni
- aggiungi solo se deducibili chiaramente

Annota anche eventuali decisioni architetturali solo se verificabili.

---

### `.ai/session_handoff.md`

Se INIT:

- descrivi stato iniziale
- indica possibili prossimi sviluppi

Se UPDATE:

- sintetizza stato attuale
- prepara continuità per prossimo modello

Deve anche dire se `architecture.md` è stato:

- confermato
- aggiornato
- lasciato invariato perché già coerente
- NON aggiornato perché architettura non sufficientemente chiara

---

### `.ai/tasks_todo.md`

Genera:

- task attivi dedotti dal codice
- eventuali miglioramenti logici
- bug potenziali (se evidenti)

Formato:

- TODO
- DOING
- DONE

Quando utile, indica se un task impatta l'architettura o richiede prima l'aggiornamento di `architecture.md`.

---

### `.ai/model_prefs.md`

Definisci uso modelli:

- Claude → architettura, reasoning
- GPT → codice, refactoring
- locale → debug, test veloci

Aggiungi anche che tutti i modelli devono leggere `architecture.md` prima di fare modifiche strutturali.

---

## REGOLE CRITICHE

- NON inventare funzionalità non presenti
- NON fare supposizioni arbitrarie
- se qualcosa non è chiaro, dichiaralo
- privilegia informazioni verificabili dal codice
- `architecture.md` è obbligatorio, ma va scritto solo quando davvero compreso
- se manca comprensione sufficiente dell'architettura, fermati e chiedi chiarimenti prima di consolidarlo

---

## OUTPUT

1. Elenca le informazioni chiave che hai dedotto dal progetto
2. Mostra i file `.ai/` generati o aggiornati
3. Spiega eventuali incertezze
4. Indica esplicitamente lo stato di `architecture.md`:
   - creato
   - aggiornato
   - invariato
   - non scritto in attesa di chiarimenti

---

## ALLINEAMENTO

Prima di scrivere file:

- riassumi cosa hai capito del progetto (max 10 righe)
- indica se ritieni di aver compreso davvero l'architettura oppure no
- attendi conferma

DOPO conferma:

- genera o aggiorna i file
