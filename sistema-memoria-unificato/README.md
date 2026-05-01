# Sistema di Memoria Unificato per Agenti LLM

Sistema a 3 prompt + struttura di file per far lavorare in continuità qualsiasi agente
(Claude Code, Codex, Gemini CLI, OpenCode, modelli locali) sullo stesso progetto, con
massima efficienza in termini di token, comprensione del contesto e svolgimento di task.

Unisce tre idee:

- la **memoria operativa** in `.ai/` (stato, task, decisioni, handoff)
- gli **standard aziendali** in `.company/` (modus operandi che si copia tra progetti)
- la **LLM-wiki stile Karpathy** in `llm-wiki/` (solo su file documentali: specifiche,
  manuali, guide, articoli tecnici, paper scientifici, wiki aziendale)

Il punto di ingresso è un singolo file hub a root, `AGENT.md`, letto per primo da ogni
agente, contenente solo link e regole di lettura (no contenuto inline → token-efficient).

---

## Struttura finale di un progetto

```
PROGETTO/
├── AGENT.md                          ← HUB (~100 righe, link + regole prioritarie)
├── .ai/                              ← memoria operativa
│   ├── project-spec.md              ← IMMUTABILE: obiettivo del progetto
│   ├── project-stack.md             ← stack + metodologia + architettura specifica
│   ├── current-state.md             ← snapshot operativo (overwrite-controlled)
│   ├── tasks-todo.md                ← TODO/DOING/DONE
│   ├── session-handoff.md           ← ultimo passaggio (overwrite)
│   ├── decisions.md                 ← registro decisioni (append-only)
│   ├── open-questions.md            ← dubbi/ambiguità (merge-only)
│   └── operations-log.md            ← log cronologico (append-only)
├── .company/                         ← standard aziendali (copiabili tra progetti)
│   └── company-standards.md
└── llm-wiki/                         ← wiki Karpathy-style, SOLO su documentazione
    ├── index.md                     ← indice navigabile
    ├── log.md                       ← log ingest
    ├── raw/                         ← fonti immutabili
    └── pages/                       ← pagine markdown interconnesse
```

---

## I 3 prompt e il ciclo

```
[NUOVO PROGETTO o RIALLINEAMENTO]
        │
        ▼
┌──────────────────────────────────┐
│  PROMPT-AVVIO-UNIVERSALE         │  ← INIT o UPDATE: costruisce/aggiorna
│  (build dei 3 file, AGENT.md)    │    .ai/, .company/, llm-wiki/, AGENT.md
└──────────────────────────────────┘
        │
        ▼
[OGNI NUOVA SESSIONE]
        │
        ▼
┌──────────────────────────────────┐
│  PROMPT-RIAVVIO                  │  ← legge AGENT.md → carica memoria
│  (contestualizza, propone piano) │    nell'ordine prioritario, propone piano
└──────────────────────────────────┘
        │
        ▼
[...lavoro effettivo...]
        │
        ▼
[FINE SESSIONE / CAMBIO AGENTE]
        │
        ▼
┌──────────────────────────────────┐
│  PROMPT-HANDOFF                  │  ← refresha SOLO i file mutabili
│  (mai tocca file immutabili)     │    aggiorna log, prepara riavvio
└──────────────────────────────────┘
        │
        └────────────────────────────► torna a PROMPT-RIAVVIO
```

---

## I tre file "built with user" (con conferma obbligatoria)

| File                            | Politica       | Quando si crea                                                        |
| ------------------------------- | -------------- | --------------------------------------------------------------------- |
| `.ai/project-spec.md`           | **IMMUTABILE** | Una volta sola, all'avvio. Mai più modificato dall'agente.            |
| `.ai/project-stack.md`          | merge-only     | All'avvio. Aggiornato solo se cambia stack/architettura.              |
| `.company/company-standards.md` | merge-only     | All'avvio: importato da progetto precedente, oppure costruito a zero. |

Per ognuno il PROMPT-AVVIO segue lo stesso ciclo:

1. blocco di domande mirate (4-8)
2. mostra un sunto strutturato di ciò che ha capito
3. chiede conferma esplicita
4. solo dopo conferma scrive il file

---

## Politiche di aggiornamento file

| Politica                 | Significato                                                              |
| ------------------------ | ------------------------------------------------------------------------ |
| **IMMUTABILE**           | L'agente NON modifica mai. Solo l'utente, a mano.                        |
| **append-only**          | Si aggiungono solo voci nuove, mai cancellare le precedenti.             |
| **merge-only**           | Si integra senza distruggere informazioni ancora valide.                 |
| **overwrite-controlled** | Si riscrive lo stato corrente preservando ciò che serve.                 |
| **overwrite**            | Si sostituisce il contenuto precedente (es. handoff = ultimo passaggio). |

---

## LLM-wiki: regola d'oro

La `llm-wiki/` viene **creata vuota** durante il PROMPT-AVVIO.

L'ingest avviene **solo su richiesta esplicita** dell'utente, con un messaggio del tipo:

> "Fai ingest del documento `specifiche-cliente.pdf` nella llm-wiki"

Nessun ingest automatico. Nessuna lettura non richiesta. Questo evita consumo inutile di token su file documentali grandi che spesso non servono per il task corrente.

Le fonti vanno in `llm-wiki/raw/` come copie immutabili. Le pagine sintetiche con link
incrociati stile Obsidian (`[[nome-pagina]]`) vanno in `llm-wiki/pages/`.

---

## Come usare

1. Apri il progetto con il tuo agente preferito.
2. **Primo accesso**: incolla `prompts/PROMPT-AVVIO-UNIVERSALE.md`. L'agente analizza, fa domande, costruisce con te i 3 file built-with-user, genera `AGENT.md`.
3. **Ogni nuova sessione**: incolla `prompts/PROMPT-RIAVVIO.md`. L'agente legge
   `AGENT.md`, carica solo ciò che serve, propone piano, chiede conferma.
4. **Fine sessione**: incolla `prompts/PROMPT-HANDOFF.md`. L'agente aggiorna i file
   mutabili e lascia tutto pronto per la sessione successiva.
5. **LLM-wiki on demand**: chiedi "fai ingest di X nella llm-wiki" quando ti serve.

---

## File in questa cartella

- `README.md` — questo file
- `template-AGENT.md` — il template di `AGENT.md` che il PROMPT-AVVIO genera nella root del progetto
- `prompts/PROMPT-AVVIO-UNIVERSALE.md` — prompt 1 (init / update)
- `prompts/PROMPT-RIAVVIO.md` — prompt 2 (avvio sessione)
- `prompts/PROMPT-HANDOFF.md` — prompt 3 (fine sessione)
