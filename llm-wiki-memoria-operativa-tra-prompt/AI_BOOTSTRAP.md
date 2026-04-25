# AI_BOOTSTRAP.md

## Scopo

Inizializzare o aggiornare una LLM Wiki locale per il progetto corrente.

La LLM Wiki è una memoria persistente in Markdown ispirata al pattern “LLM Wiki”: fonti originali immutabili, pagine wiki mantenute dall’LLM, collegamenti interni tra concetti e aggiornamento incrementale della conoscenza.

Questa versione unisce due funzioni:

1. **comprensione** del progetto;
2. **azione** operativa da parte di agenti LLM diversi.

L’obiettivo è permettere a Claude Code, Codex, Gemini CLI, modelli locali o altri agenti di lavorare sullo stesso progetto senza perdere contesto.

---

## Struttura da creare

Crea, se non esiste, questa struttura:

```text
llm/
├── index.md
├── raw/
├── pages/
│   ├── project-brief.md
│   ├── overview.md
│   ├── architecture.md
│   ├── data-model.md
│   ├── api.md
│   ├── frontend.md
│   ├── backend.md
│   ├── deployment.md
│   ├── security.md
│   ├── current-state.md
│   ├── decisions.md
│   ├── tasks-todo.md
│   ├── session-handoff.md
│   └── open-questions.md
├── logs/
│   └── operations-log.md
└── prompts/
    ├── AI_BOOTSTRAP.md
    ├── AI_WORKFLOW.md
    └── AI_HANDOFF.md
```

Se il progetto non richiede alcune pagine, creale comunque con stato `non-applicabile` oppure `da-verificare`.

---

## Funzione dei file statici

### `llm/index.md`

È il punto di ingresso obbligatorio.

Deve contenere:

- descrizione sintetica del progetto;
- mappa delle pagine principali;
- collegamenti interni stile Obsidian, ad esempio `[[architecture]]`;
- sezione “per iniziare una nuova sessione”;
- sezione “stato operativo” con link a:
  - `[[current-state]]`
  - `[[tasks-todo]]`
  - `[[session-handoff]]`
  - `[[open-questions]]`

---

### `.llm/raw/`

Contiene fonti originali, copie o riferimenti immutabili.

Regole:

- non modificare mai i file sorgenti originali;
- se copi una fonte in `raw/`, non riscriverla;
- se non copi la fonte, registra il riferimento nella pagina wiki pertinente;
- considera fonti: README, documentazione, codice, configurazioni, schemi DB, API, PDF, note, script, file di progetto.

---

### `.llm/pages/project-brief.md`

Contiene la descrizione stabile del progetto.

Deve includere:

- obiettivo;
- contesto;
- stack tecnologico;
- componenti principali;
- struttura cartelle;
- vincoli noti;
- note tecniche opzionali.

È una pagina di comprensione, non di stato giornaliero.

---

### `.llm/pages/overview.md`

Contiene una sintesi navigabile del sistema.

Deve spiegare:

- che cosa fa il progetto;
- quali sono i sottosistemi;
- come si leggono le altre pagine;
- quali parti sono certe e quali dedotte.

---

### `.llm/pages/architecture.md`

È una pagina critica.

Descrive la struttura del sistema solo se verificata o ragionevolmente deducibile.

Deve contenere:

- panoramica architetturale;
- moduli;
- responsabilità;
- flussi principali;
- flussi dati;
- punti di ingresso;
- dipendenze interne ed esterne;
- integrazioni;
- aree sensibili;
- eventuali diagrammi Mermaid.

Regola speciale:

- se l’architettura non è chiara, non inventare;
- scrivi esplicitamente cosa manca;
- aggiungi domande in `open-questions.md`;
- non consolidare ipotesi come fatti.

---

### `.llm/pages/data-model.md`

Contiene database, strutture dati, file persistenti, entità e relazioni.

Usa Mermaid ER diagram quando utile.

---

### `.llm/pages/api.md`

Contiene endpoint, contratti, messaggi, payload, protocolli, interfacce tra moduli o servizi.

Se non esistono API, indicare `non-applicabile`.

---

### `.llm/pages/frontend.md`

Contiene struttura UI, viste, componenti, flussi utente, asset e logica client.

Se non esiste frontend, indicare `non-applicabile`.

---

### `.llm/pages/backend.md`

Contiene servizi, controller, logica applicativa, job, script server-side, accesso ai dati.

Se non esiste backend, indicare `non-applicabile`.

---

### `.llm/pages/deployment.md`

Contiene avvio, installazione, ambiente, variabili, build, deploy, servizi esterni.

---

### `.llm/pages/security.md`

Contiene:

- credenziali;
- segreti;
- autenticazione;
- autorizzazione;
- validazione input;
- rischi;
- superfici d’attacco;
- note privacy/GDPR se pertinenti.

Non inventare misure di sicurezza non presenti.

---

### `.llm/pages/current-state.md`

È una pagina operativa.

Deve descrivere:

- stato attuale;
- cosa funziona;
- cosa è incompleto;
- ultimi file o aree modificate;
- problemi aperti;
- impatto sull’architettura, se presente.

---

### `.llm/pages/decisions.md`

È il registro delle decisioni.

Regole:

- aggiungi solo decisioni verificabili;
- non trasformare ipotesi in decisioni;
- ogni decisione deve indicare:
  - data;
  - contesto;
  - decisione;
  - motivazione;
  - conseguenze.

Formato consigliato:

```md
## YYYY-MM-DD - Titolo decisione

**Contesto:** ...
**Decisione:** ...
**Motivazione:** ...
**Conseguenze:** ...
**Fonti:** ...
```

---

### `.llm/pages/tasks-todo.md`

Contiene task operativi.

Usa sezioni:

```md
## TODO
## DOING
## DONE
```

Per ogni task indicare, quando possibile:

- descrizione;
- file coinvolti;
- impatto architetturale: sì/no/da-verificare;
- rischi di regressione.

---

### `.llm/pages/session-handoff.md`

È il passaggio di consegne per il prossimo modello.

Deve contenere:

- cosa è stato fatto;
- stato attuale preciso;
- prossimi step numerati;
- cosa non rifare;
- cosa verificare prima di modificare codice;
- stato di `architecture.md`:
  - confermata;
  - aggiornata;
  - invariata;
  - da rivedere;
  - non scritta perché architettura non chiara.

---

### `.llm/pages/open-questions.md`

Contiene dubbi, ambiguità, contraddizioni, informazioni mancanti.

Ogni punto deve indicare:

- domanda;
- contesto;
- file coinvolti;
- impatto;
- priorità.

---

### `.llm/logs/operations-log.md`

Registro cronologico append-only.

Non riscrivere la storia.

Ogni voce deve contenere:

```md
## YYYY-MM-DD HH:mm - Operazione

**Tipo:** init/update/ingest/refactor/handoff/check
**File analizzati:** ...
**Pagine aggiornate:** ...
**Decisioni aggiunte:** ...
**Dubbi emersi:** ...
```

---

## Tipi di pagina

Ogni pagina in `.llm/pages/` deve iniziare con front matter YAML:

```md
---
type: knowledge-page | operational-state | decision-log | task-list | handoff | question-log
role: nome-ruolo
update_policy: merge-only | overwrite-controlled | append-only
last_verified: YYYY-MM-DD
---
```

Regole:

- `knowledge-page`: spiega il progetto;
- `operational-state`: guida l’azione;
- `decision-log`: registra decisioni;
- `task-list`: organizza lavoro;
- `handoff`: consente cambio modello;
- `question-log`: mantiene dubbi;
- `append-only`: non cancellare voci precedenti;
- `merge-only`: integra senza distruggere contenuto valido;
- `overwrite-controlled`: aggiorna lo stato corrente, preservando ciò che serve.

---

## Procedura INIT

Se `.llm/` non esiste:

1. analizza la cartella progetto;
2. identifica linguaggi, struttura, file principali, README, documentazione, configurazioni, schema DB, API e codice principale;
3. crea la struttura `.llm/`;
4. crea le pagine statiche;
5. popola `index.md`;
6. scrivi una prima versione delle pagine deducibili;
7. se l’architettura non è chiara, non inventare;
8. registra i dubbi in `open-questions.md`;
9. aggiorna `operations-log.md`;
10. produci un breve report in chat.

Prima di scrivere i file, mostra massimo 10 righe su cosa hai capito e chiedi conferma.

---

## Procedura UPDATE

Se `.llm/` esiste:

1. leggi `index.md`;
2. leggi le pagine operative:
   - `current-state.md`;
   - `tasks-todo.md`;
   - `session-handoff.md`;
   - `open-questions.md`;
3. leggi le pagine knowledge rilevanti;
4. confronta la wiki con il codice attuale;
5. aggiorna solo ciò che è necessario;
6. non sovrascrivere informazioni valide;
7. segnala divergenze tra codice e wiki;
8. aggiorna `operations-log.md`;
9. produci un breve report.

---

## Regole critiche

- Non modificare codice sorgente durante il bootstrap.
- Non cancellare nulla.
- Non inventare funzionalità.
- Se una cosa è dedotta, scrivilo.
- Se una fonte è troppo grande, analizzala per sezioni.
- Mantieni tutto utile per sessioni future.
- La wiki deve essere un grafo, non un riassunto file-per-file.
- Crea pagine per componenti, concetti, flussi, decisioni, problemi aperti.
- Usa link interni `[[nome-pagina]]`.
- Usa Mermaid quando chiarisce architettura, flussi o modello dati.

---

## Output finale richiesto

Alla fine rispondi con:

1. informazioni chiave dedotte;
2. file creati o aggiornati;
3. incertezze;
4. stato di `architecture.md`;
5. prossimi ingest consigliati.
