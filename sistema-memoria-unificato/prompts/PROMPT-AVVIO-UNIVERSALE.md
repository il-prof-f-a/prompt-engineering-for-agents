# PROMPT-AVVIO-UNIVERSALE

## TASK

Devi inizializzare o aggiornare il sistema di memoria persistente del progetto in cui ti
trovi. Il sistema permette a qualsiasi agente LLM (Claude Code, Codex, Gemini CLI,
OpenCode, modelli locali) di lavorare in continuità, senza perdita di contesto e con
massima efficienza in termini di token.

Struttura finale attesa:

```
PROGETTO/
├── AGENT.md
├── .ai/
│   ├── project-spec.md          (IMMUTABILE)
│   ├── project-stack.md
│   ├── current-state.md
│   ├── tasks-todo.md
│   ├── session-handoff.md
│   ├── decisions.md             (append-only)
│   ├── open-questions.md
│   └── operations-log.md        (append-only)
├── .company/
│   └── company-standards.md
└── llm-wiki/
    ├── index.md
    ├── log.md
    ├── raw/                     (vuota all'init)
    └── pages/                   (vuota all'init)
```

---

## FASE 0 — RILEVAMENTO

1. Analizza la cartella corrente del progetto.
2. Identifica:
   - linguaggi e file principali
   - presenza di README, documentazione, file di specifica
   - stack visibile (package.json, pyproject.toml, Cargo.toml, go.mod, ecc.)
   - eventuali fonti di task (issues, kanban, file `TODO.md`, ecc.)
3. Verifica esistenza di:
   - `AGENT.md` a root
   - cartella `.ai/`
   - cartella `.company/`
   - cartella `llm-wiki/`

---

## FASE 1 — DECISIONE MODALITÀ

| Condizione                                           | Modalità                                                       |
| ---------------------------------------------------- | -------------------------------------------------------------- |
| `AGENT.md` e `.ai/` non esistono                     | **INIT**                                                       |
| `AGENT.md` esiste e `.ai/` esiste                    | **UPDATE**                                                     |
| Esiste solo una parte (es. `.ai/` ma non `AGENT.md`) | **REPAIR** (allinea il mancante senza distruggere il presente) |

Dichiara la modalità rilevata prima di procedere.

---

## FASE 2 — DOMANDE INIZIALI MINIME (SOLO MODALITÀ INIT)

In modalità INIT, prima di iniziare a costruire qualsiasi file, fai SOLO questa domanda
ad alto livello:

> **D1.** È un progetto **nuovo** (da costruire) o **esistente** (da assorbire)?
> **D2.** Esiste già un file `company-standards.md` da un altro progetto che vuoi
> importare? Se sì, dammi il path; se no, lo costruiamo insieme da zero.
> **D3.** Esiste un documento di specifica/brief del progetto già scritto (PDF, .docx,
> .md, link a Notion/Confluence)? Se sì, dammi il riferimento; se no, lo costruiamo
> insieme.

Attendi risposta. Le risposte determinano il flusso successivo.

In modalità UPDATE o REPAIR, salta la Fase 2 e vai direttamente alla Fase 7 (allineamento).

---

## FASE 3 — COSTRUZIONE DI `company-standards.md`

Obiettivo: definire il **modus operandi aziendale** trasversale a più progetti. Vive in
`.company/company-standards.md`.

### Caso A — l'utente ha indicato un file da importare

1. Leggi il file indicato.
2. Verifica che copra le sezioni obbligatorie (vedi sotto). Se ne mancano, fai domande
   solo per quelle.
3. Mostra un sunto di 8-12 righe di cosa userai come `company-standards.md`.
4. Chiedi conferma esplicita.
5. Solo dopo conferma, scrivi `.company/company-standards.md`.

### Caso B — costruzione da zero

Fai questo blocco di domande in un'unica tornata. Massimo 8 domande:

1. Quali **framework e librerie** sono standard nei progetti aziendali (es. Next.js,
   FastAPI, Spring Boot, Django, ecc.)?
2. Quali **standard di codice** (linter, formatter, naming convention, max complexity,
   coverage minima)?
3. Quali **tipi di test obbligatori** (smoke, unit, integration, e2e, contract, fuzz)?
4. Quali **programmi esterni di test/QA** o tool ricorrenti (Postman, k6, Playwright,
   SonarQube, ecc.)?
5. Quali **convenzioni Git** (branching model, message format, PR template, review
   policy)?
6. Quali **standard di documentazione** (ADR, README obbligatorio, OpenAPI, ecc.)?
7. Quali **programmi I/O ricorrenti** con cui i progetti aziendali si interfacciano
   spesso (es. Excel, AutoCAD, ERP, sistemi di telemetria, database aziendali, ecc.)?
8. **Vincoli aziendali** non negoziabili (sicurezza, compliance, GDPR, normative di
   settore, accessibilità)?

Attendi le risposte. Poi:

1. Mostra all'utente un sunto strutturato di 10-15 righe con le sezioni che andrai a
   scrivere e cosa conterrà ognuna.
2. Chiedi conferma esplicita ("Confermi la scrittura di `.company/company-standards.md`
   con questo contenuto?").
3. Solo dopo conferma, scrivi il file.

### Sezioni obbligatorie di `company-standards.md`

```md
# Company Standards — Modus Operandi Aziendale

> File trasversale, copiabile tra progetti. Modificabile solo se cambiano davvero gli
> standard aziendali (politica: merge-only).

## 1. Stack di riferimento
## 2. Standard di codice e qualità
## 3. Test policy
## 4. Tool di test e QA esterni
## 5. Convenzioni Git e PR
## 6. Standard di documentazione
## 7. Programmi I/O ricorrenti
## 8. Vincoli e compliance
## 9. Note e deroghe note
```

---

## FASE 4 — COSTRUZIONE DI `project-spec.md` (IMMUTABILE)

Obiettivo: definire l'**obiettivo immutabile del progetto**. Vive in `.ai/project-spec.md`.
Una volta scritto, l'agente non lo modifica mai più. Solo l'utente può modificarlo a mano.

### Caso A — l'utente ha indicato un documento di specifica esistente

1. Leggi il documento (se PDF/docx, estrai il testo).
2. Sintetizza in `project-spec.md` le sole informazioni stabili e immutabili (vedi
   sezioni sotto).
3. Mostra un sunto di 10-15 righe.
4. Chiedi conferma esplicita.
5. Solo dopo conferma, scrivi il file.

### Caso B — costruzione da zero

Blocco di domande, una sola tornata, massimo 6 domande:

1. **Obiettivo principale** del progetto in una frase.
2. **Cliente / destinatario** (interno, esterno, pubblico, mercato).
3. **3-5 risultati attesi** misurabili (deliverable, KPI, milestone macro).
4. **Vincoli inderogabili** (deadline immutabili, budget, normative, integrazioni
   obbligatorie).
5. **Deliverable finale** (cosa esce dalla porta a fine progetto: software, report,
   sistema, prodotto).
6. **Successo** (cosa significa "il progetto è riuscito").

Attendi le risposte. Poi:

1. Mostra un sunto di 10-15 righe.
2. Chiedi conferma esplicita.
3. Solo dopo conferma, scrivi il file.

### Sezioni di `project-spec.md`

```md
---
status: immutabile
update_policy: solo-utente-a-mano
---

# Project Spec — Obiettivo del Progetto

> Documento immutabile. Definisce l'obiettivo stabile del progetto. L'agente NON lo
> modifica mai. Solo l'utente, a mano.

## 1. Obiettivo principale
## 2. Cliente / destinatario
## 3. Risultati attesi
## 4. Vincoli inderogabili
## 5. Deliverable finale
## 6. Definizione di successo
```

---

## FASE 5 — COSTRUZIONE DI `project-stack.md`

Obiettivo: definire **stack, framework, metodologia e architettura del progetto
specifico**. Sostituisce il classico `architecture.md`. Vive in `.ai/project-stack.md`.

### Strategia ibrida

1. Se c'è codice esistente, **deduci** stack e architettura osservabili (linguaggi,
   framework, struttura cartelle, entry point, dipendenze).

2. Per ciò che NON è deducibile, fai domande mirate. Massimo 7 domande:
   
   1. **Approccio di sviluppo**: top-down, bottom-up, iterativo, TDD, BDD, altro?
   2. **Sorgente dei task**: backlog interno (`tasks-todo.md`), kanban (Trello/Jira/Linear), Notion, file di specifica, issue tracker, altro?
   3. **Architettura target**: monolite, microservizi, serverless, event-driven, plugin,
      altro? (se non già desumibile)
   4. **Test attivi su questo progetto** (sottoinsieme dei test aziendali standard, o
      diversi)?
   5. **Tool di build / CI / CD** specifici di questo progetto?
   6. **Programmi esterni I/O specifici** del progetto (oltre a quelli aziendali
      ricorrenti)?
   7. **Deroghe** rispetto a `company-standards.md`: ci sono punti su cui questo
      progetto si discosta dagli standard aziendali, e perché?

3. Mostra un sunto strutturato di 15-25 righe.

4. Chiedi conferma esplicita.

5. Solo dopo conferma, scrivi il file.

### Sezioni di `project-stack.md`

```md
---
update_policy: merge-only
last_verified: YYYY-MM-DD
---

# Project Stack — Stack, Metodologia, Architettura del Progetto

## 1. Stack tecnico
   - linguaggi
   - framework
   - librerie chiave
   - versione runtime / SDK

## 2. Approccio di sviluppo e metodologia
## 3. Sorgente dei task e workflow
## 4. Architettura
   - panoramica
   - moduli e responsabilità
   - flussi principali
   - flussi dati
   - punti di ingresso
   - dipendenze interne ed esterne
   - integrazioni / API / servizi esterni

## 5. Test policy del progetto
## 6. Build, CI, CD specifici
## 7. Programmi I/O specifici
## 8. Deroghe rispetto a company-standards.md
## 9. Aree sensibili / colli di bottiglia noti
## 10. Note tecniche e diagrammi (opzionale, Mermaid)
```

### Regola d'oro

Se l'architettura **non è chiara** dal codice e l'utente non l'ha ancora definita, NON
inventarla. Scrivi nella sezione 4: "Da definire — domande aperte rinviate a
`open-questions.md`" e popola `open-questions.md` di conseguenza.

---

## FASE 6 — GENERAZIONE FILE OPERATIVI

Crea i file della memoria operativa. Per quelli con contenuto deducibile, popola; per gli
altri, crea con scaffolding minimo.

### `.ai/current-state.md`

```md
---
update_policy: overwrite-controlled
last_updated: YYYY-MM-DD
---

# Current State

## Stato attuale
[Inizializzazione del sistema di memoria. Il progetto è in fase di setup.]
[Oppure, se il progetto esiste: dedurre dallo stato del codice.]

## Cosa funziona
## Cosa è incompleto
## Ultimi file / aree modificati
## Problemi aperti
## Impatto sull'architettura
```

### `.ai/tasks-todo.md`

```md
---
update_policy: overwrite-controlled
---

# Tasks

## TODO
- [ ] Verificare che AGENT.md e i file `.ai/` siano coerenti con il codice (impatto: basso)

## DOING

## DONE
- [x] Inizializzazione sistema di memoria (data odierna)
```

### `.ai/session-handoff.md`

```md
---
update_policy: overwrite
last_session: YYYY-MM-DD
---

# Session Handoff

## Cosa è stato fatto
- Inizializzazione del sistema di memoria (`.ai/`, `.company/`, `llm-wiki/`, `AGENT.md`)
- Creazione di `project-spec.md`, `project-stack.md`, `company-standards.md`

## Stato attuale preciso
[Sintesi dello stato dopo init]

## Prossimi step numerati
1. [Primo task suggerito]

## Cosa NON rifare
- Non ripetere la fase di init: il sistema è già configurato.

## Stato di project-stack.md
[confermato | parziale | da rivedere]
```

### `.ai/decisions.md`

```md
---
update_policy: append-only
---

# Decisions

## YYYY-MM-DD — Adozione del sistema di memoria unificato

**Contesto:** init del progetto.
**Decisione:** adozione di `.ai/`, `.company/`, `llm-wiki/`, `AGENT.md` come sistema di
memoria persistente per agenti LLM.
**Motivazione:** continuità tra agenti diversi, efficienza token, comprensione contesto.
**Conseguenze:** ogni nuova sessione parte da `AGENT.md` e segue la lettura prioritaria.
```

### `.ai/open-questions.md`

```md
---
update_policy: merge-only
---

# Open Questions

[Vuoto se l'init non ha sollevato dubbi. Altrimenti, una voce per ogni dubbio:]

## [Titolo dubbio]
- **Domanda:** ...
- **Contesto:** ...
- **File coinvolti:** ...
- **Impatto:** basso/medio/alto
- **Priorità:** bassa/media/alta
```

### `.ai/operations-log.md`

```md
---
update_policy: append-only
---

# Operations Log

## YYYY-MM-DD HH:mm — INIT

**Tipo:** init
**Agente:** [nome agente / modello]
**File creati:** AGENT.md, .ai/* (8 file), .company/company-standards.md, llm-wiki/index.md, llm-wiki/log.md
**Fonti utilizzate:** [doc importati o "costruito da zero con utente"]
**Decisioni aggiunte:** 1 (adozione sistema di memoria)
**Dubbi emersi:** [N]
**Note:** ...
```

### `llm-wiki/index.md`

```md
# LLM Wiki — Index

> La wiki è inizialmente VUOTA. L'ingest avviene SOLO su richiesta esplicita
> dell'utente, mai automaticamente.

## Pagine
[Vuoto. Si popolerà man mano che l'utente chiede ingest di documenti.]

## Sorgenti immutabili in `raw/`
[Vuoto.]
```

### `llm-wiki/log.md`

```md
# LLM Wiki — Log

> Registro append-only delle operazioni di ingest.

## YYYY-MM-DD HH:mm — Init wiki vuota
- Creata struttura `llm-wiki/` (raw/, pages/, index.md, log.md).
- Nessun ingest automatico effettuato.
```

---

## FASE 7 — GENERAZIONE / AGGIORNAMENTO DI `AGENT.md`

Crea (o aggiorna senza distruggere) il file `AGENT.md` a root del progetto. Deve restare
un **hub minimale** (~100-150 righe), contenere SOLO:

- ordine di lettura prioritaria dei file di memoria
- politica di scrittura per ogni file (tabella)
- comportamento di default per tipo di task
- principi non negoziabili
- indice dei file di memoria

NON duplicare in `AGENT.md` il contenuto degli altri file. È un indice, non una sintesi.

Modello da usare: vedi `template-AGENT.md` nel sistema.

---

## FASE 8 — ALLINEAMENTO MODALITÀ UPDATE / REPAIR

Solo se modalità UPDATE o REPAIR:

1. Leggi tutti i file `.ai/*` e `AGENT.md` esistenti.
2. Confronta con il codice attuale.
3. Identifica:
   - file mancanti (da creare con scaffolding)
   - file disallineati con il codice (da segnalare)
   - file ancora validi (lasciare invariati)
4. NON sovrascrivere informazioni valide.
5. Per `project-spec.md`: NON modificare mai (è immutabile). Se serve, segnala
   all'utente.
6. Per `project-stack.md` e `company-standards.md`: politica merge-only, integra senza distruggere.
7. Aggiorna `current-state.md`, `session-handoff.md`, `tasks-todo.md` per riflettere lo stato reale.
8. Aggiungi una voce in `operations-log.md` di tipo `update` o `repair`.

---

## FASE 9 — OUTPUT FINALE IN CHAT

Dopo aver scritto tutti i file, rispondi in chat con:

1. Modalità eseguita (INIT / UPDATE / REPAIR).
2. File creati o aggiornati (lista).
3. Stato di `project-stack.md`: completo / parziale / da rivedere.
4. Numero di domande aperte aggiunte a `open-questions.md`.
5. Prossimo step suggerito.

---

## REGOLE CRITICHE

- **Non scrivere alcun file senza conferma** per i tre file built-with-user
  (`company-standards.md`, `project-spec.md`, `project-stack.md`). Mostrare sempre un
  sunto e attendere "ok" / "conferma" / "procedi".
- **Mai modificare `project-spec.md` dopo la prima scrittura.**
- **Mai inventare** funzionalità, decisioni, architettura non verificabili dal codice o
  da risposta utente.
- **Dichiarare le incertezze** invece di colmarle con ipotesi.
- **NON fare ingest automatico** in `llm-wiki/`. Solo su richiesta esplicita.
- **Aggiornare AGENT.md SOLO se cambia la struttura** (nuovi file, nuove regole). Non riscriverlo a ogni operazione.
