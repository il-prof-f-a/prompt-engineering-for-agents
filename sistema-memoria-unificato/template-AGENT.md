# AGENT.md

> File hub. Letto per primo da ogni agente in ogni sessione.
> Contiene SOLO indice + regole di lettura prioritaria.
> Non duplicare qui contenuto degli altri file.
> Aggiornare solo quando si creano nuove pagine o cambia la struttura.

---

## Lettura prioritaria (ordine obbligato, fermarsi appena il task è chiaro)

Per OGNI nuovo task in OGNI sessione, leggere in quest'ordine:

1. [`.ai/session-handoff.md`](./.ai/session-handoff.md) — cosa è successo nell'ultima sessione, prossimi step
2. [`.ai/current-state.md`](./.ai/current-state.md) — stato operativo attuale
3. [`.ai/tasks-todo.md`](./.ai/tasks-todo.md) — task attivi (TODO/DOING/DONE)
4. [`.ai/project-stack.md`](./.ai/project-stack.md) — stack, framework, metodologia, architettura del progetto
5. [`.ai/project-spec.md`](./.ai/project-spec.md) — obiettivo immutabile del progetto
6. [`.company/company-standards.md`](./.company/company-standards.md) — modus operandi aziendale

Caricare solo se rilevante per il task:

7. [`.ai/decisions.md`](./.ai/decisions.md) — decisioni passate (consultare prima di scelte tecniche)
8. [`.ai/open-questions.md`](./.ai/open-questions.md) — dubbi aperti
9. [`llm-wiki/index.md`](./llm-wiki/index.md) + pagine specifiche — conoscenza derivata da documentazione

Mai leggere alla cieca tutta `llm-wiki/`. Aprire solo le pagine indicizzate rilevanti.

---

## Politiche di scrittura per file

| File                            | Politica             | Modificare quando                                      |
| ------------------------------- | -------------------- | ------------------------------------------------------ |
| `.ai/project-spec.md`           | **IMMUTABILE**       | Mai (solo l'utente, a mano)                            |
| `.ai/project-stack.md`          | merge-only           | Solo se cambia stack/metodologia/architettura          |
| `.company/company-standards.md` | merge-only           | Solo se cambiano standard aziendali                    |
| `.ai/current-state.md`          | overwrite-controlled | Ogni handoff                                           |
| `.ai/session-handoff.md`        | overwrite            | Ogni handoff                                           |
| `.ai/tasks-todo.md`             | overwrite-controlled | Durante e a fine sessione                              |
| `.ai/decisions.md`              | append-only          | Quando emerge una decisione verificabile               |
| `.ai/open-questions.md`         | merge-only           | Quando emergono dubbi nuovi o si chiudono dubbi vecchi |
| `.ai/operations-log.md`         | append-only          | A ogni handoff e a ogni operazione significativa       |
| `llm-wiki/raw/*`                | **IMMUTABILE**       | Mai modificare le fonti                                |
| `llm-wiki/pages/*`              | merge-only           | Solo durante un'operazione di ingest esplicita         |
| `llm-wiki/index.md`             | merge-only           | Quando si aggiungono o rimuovono pagine wiki           |
| `llm-wiki/log.md`               | append-only          | A ogni operazione di ingest                            |

---

## Comportamento di default per tipo di task

- **Sola lettura / analisi / risposta a domanda**: leggi i file 1-6, rispondi.
  Aggiungi dubbi a `open-questions.md` se ne emergono.
- **Modifica codice non strutturale**: leggi 1-6, proponi piano in massimo 5 step,
  chiedi conferma, esegui, aggiorna `current-state.md`, `tasks-todo.md`, `session-handoff.md`,
  `operations-log.md`.
- **Modifica architettura, stack, dipendenze, API, schema dati, sicurezza**: leggi 1-7,
  segnala impatto su `project-stack.md` e `decisions.md`, **chiedi conferma esplicita prima
  di procedere**.
- **Handoff fine sessione**: usa il PROMPT-HANDOFF.
- **Riavvio nuova sessione**: usa il PROMPT-RIAVVIO.
- **Ingest documento nella wiki**: solo su richiesta esplicita dell'utente, mai
  automatico.

---

## Principi non negoziabili

1. **Non inventare.** Se architettura, decisione, stato non sono chiari → dichiarare e
   aggiungere a `open-questions.md`.
2. **Aggiornare solo ciò che è cambiato.** Niente riscritture inutili.
3. **Scrivere per un altro agente, non per un umano.** Frasi operative, niente narrazione.
4. **L'utente è la fonte di verità per `project-spec.md`.** Mai riscriverlo.
5. **Chiedere conferma prima di modifiche strutturali.**
6. **Loggare le operazioni significative** in `operations-log.md`.
7. **Codice > wiki.** Se il codice contraddice un file di memoria, fidarsi del codice e
   segnalare il disallineamento in `open-questions.md`.

---

## Indice memoria operativa (`.ai/`)

- [project-spec.md](./.ai/project-spec.md) — obiettivo del progetto, IMMUTABILE
- [project-stack.md](./.ai/project-stack.md) — stack, framework, metodologia, architettura
- [current-state.md](./.ai/current-state.md) — fotografia stato attuale
- [tasks-todo.md](./.ai/tasks-todo.md) — backlog operativo
- [session-handoff.md](./.ai/session-handoff.md) — ultimo passaggio di consegne
- [decisions.md](./.ai/decisions.md) — registro decisioni
- [open-questions.md](./.ai/open-questions.md) — dubbi e ambiguità
- [operations-log.md](./.ai/operations-log.md) — log cronologico

## Indice standard aziendali (`.company/`)

- [company-standards.md](./.company/company-standards.md) — modus operandi aziendale,
  framework di riferimento, standard sviluppo, test policy, programmi I/O ricorrenti

## Indice LLM-wiki (`llm-wiki/`)

> **Generata SOLO su richiesta esplicita.** Nessun ingest automatico.

- [llm-wiki/index.md](./llm-wiki/index.md) — indice navigabile delle pagine wiki
- `llm-wiki/raw/` — fonti immutabili dei documenti ingestiti
- `llm-wiki/pages/` — pagine wiki markdown con link interni `[[nome-pagina]]`
- [llm-wiki/log.md](./llm-wiki/log.md) — registro operazioni di ingest
