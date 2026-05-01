# Guida all'invocazione delle Skill per Agenti AI

Come installare e invocare skill in Claude Code, Codex, Cursor e tutti gli altri agenti supportati.

---

## Come funzionano le skill

Una skill è una cartella contenente almeno un file `SKILL.md` con frontmatter YAML:

```markdown
---
name: nome-skill
description: Cosa fa e quando attivarla.
---

# Istruzioni per l'agente
...
```

L'agente legge questo file e sa **cosa fare** e **quando farlo** — automaticamente o su invocazione esplicita.

---

## 1. Claude Code

### Percorsi di installazione

```
# Progetto (condiviso col team, committato in git)
.claude/skills/<nome-skill>/SKILL.md

# Globale (tutti i tuoi progetti)
~/.claude/skills/<nome-skill>/SKILL.md
```

### Installazione rapida con npx

```bash
# Da un repo GitHub
npx skills add anthropics/skills
npx skills add vercel-labs/agent-skills
npx skills add markdown-viewer/skills
npx skills add obra/superpowers

# Skill specifica
npx skills add vercel-labs/agent-skills --skill react-best-practices

# Dal marketplace ufficiale Anthropic
/plugin marketplace add anthropics/skills
/plugin install example-skills@anthropic-agent-skills
```

### Installazione manuale

```bash
# Copia la skill nella cartella del progetto
cp -r ai-skills/backend/api-design .claude/skills/

# oppure globale
cp -r ai-skills/backend/api-design ~/.claude/skills/
```

### Invocazione in Claude Code

Le skill si attivano **automaticamente** quando l'agente rileva un task rilevante
(basandosi sul campo `description` del SKILL.md).

Per invocarle **esplicitamente** in chat:

```
# Nomina la skill nella richiesta
Usa la skill api-design per progettare gli endpoint del mio progetto.

# Oppure descrivi il task — l'agente sceglie da solo
Crea un'API REST per gestire gli utenti.
→ l'agente carica automaticamente la skill api-design se installata
```

### Slash commands in Claude Code

Claude Code supporta slash commands per alcune operazioni built-in:

```bash
/skill                  # lista le skill installate (se supportato dalla versione)
/plugin install ...     # installa un plugin/skill dal marketplace
/plugin list            # lista plugin installati
```

Per le skill custom, l'invocazione è **per descrizione**, non per comando `/`.

---

## 2. OpenAI Codex CLI

### Percorsi di installazione

```
# Progetto
.agents/skills/<nome-skill>/SKILL.md

# Globale
~/.codex/skills/<nome-skill>/SKILL.md
```

### Installazione

```bash
npx skills add vercel-labs/agent-skills -a codex
npx skills add obra/superpowers -a codex

# Globale
npx skills add vercel-labs/agent-skills -a codex -g
```

### Installazione manuale

```bash
mkdir -p .agents/skills/api-design
cp ai-skills/backend/api-design/SKILL.md .agents/skills/api-design/
```

### Invocazione in Codex

Automatica per descrizione del task, oppure tramite il **pannello Plugin**:

```
# In Codex App:
Sidebar → Plugins → Coding → cerca la skill → "+"

# In Codex CLI:
/plugins          # apre l'interfaccia di ricerca
→ cerca il nome della skill
→ Install Plugin

# In chat (invocazione esplicita)
Segui la skill fastapi per creare il server.
```

---

## 3. Cursor

### Percorsi di installazione

```
# Progetto
.agents/skills/<nome-skill>/SKILL.md

# Globale
~/.cursor/skills/<nome-skill>/SKILL.md
```

### Installazione

```bash
npx skills add vercel-labs/agent-skills -a cursor
npx skills add obra/superpowers -a cursor

# Oppure dal marketplace in Agent chat
/add-plugin superpowers
```

### Invocazione in Cursor

```
# In Agent chat — invocazione esplicita
@skill api-design progetta gli endpoint REST

# Automatica — descrivi il task
Crea i componenti React per la dashboard
→ carica automaticamente react-best-practices se installata
```

---

## 4. Gemini CLI

### Percorsi di installazione

```
# Progetto
.agents/skills/<nome-skill>/SKILL.md

# Globale
~/.gemini/skills/<nome-skill>/SKILL.md
```

### Installazione

```bash
npx skills add vercel-labs/agent-skills -a gemini-cli

# Per Superpowers
gemini extensions install https://github.com/obra/superpowers
gemini extensions update superpowers   # aggiornamento
```

---

## 5. GitHub Copilot (VS Code / CLI)

### Percorsi di installazione

```
# Progetto (rilevamento automatico in VS Code)
.github/skills/<nome-skill>/SKILL.md

# oppure
.agents/skills/<nome-skill>/SKILL.md

# Globale
~/.copilot/skills/<nome-skill>/SKILL.md
```

### Installazione

```bash
npx skills add vercel-labs/agent-skills -a github-copilot

# GitHub Copilot CLI — Superpowers
copilot plugin marketplace add obra/superpowers-marketplace
copilot plugin install superpowers@superpowers-marketplace
```

### Invocazione

In VS Code Agent mode, le skill si attivano automaticamente.
In Copilot Chat puoi referenziarle esplicitamente:

```
# Chat
@workspace usa la skill api-design per questo progetto
```

---

## 6. OpenCode

### Percorsi di installazione

```
# Progetto
.agents/skills/<nome-skill>/SKILL.md

# Globale
~/.config/opencode/skills/<nome-skill>/SKILL.md
```

### Installazione

```bash
npx skills add vercel-labs/agent-skills -a opencode

# Per Superpowers — dì all'agente in chat
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md
```

---

## 7. Altri agenti — tabella percorsi

| Agente | Path progetto | Path globale |
|--------|--------------|-------------|
| Windsurf | `.windsurf/skills/` | `~/.codeium/windsurf/skills/` |
| Roo Code | `.roo/skills/` | `~/.roo/skills/` |
| Cline | `.agents/skills/` | `~/.agents/skills/` |
| Amp | `.agents/skills/` | `~/.config/agents/skills/` |
| Goose | `.goose/skills/` | `~/.config/goose/skills/` |
| Continue | `.continue/skills/` | `~/.continue/skills/` |
| Zencoder | `.zencoder/skills/` | `~/.zencoder/skills/` |
| Trae | `.trae/skills/` | `~/.trae/skills/` |

Per tutti: `npx skills add <repo> -a <agent-name>` installa nel percorso corretto.

---

## Installazione multi-agente in un colpo solo

```bash
# Installa per tutti gli agenti rilevati automaticamente
npx skills add vercel-labs/agent-skills --all

# Installa per agenti specifici
npx skills add vercel-labs/agent-skills -a claude-code -a cursor -a codex

# Installa le skill di questo repo per Claude Code
npx skills add ./ai-skills -a claude-code
```

---

## Cowork (Claude Desktop)

In Cowork le skill invocabili con `/` sono quelle registrate nel sistema plugin,
non i file SKILL.md standalone. Per creare una skill Cowork vera:

1. Usa la skill **`skill-creator`** già installata
2. Oppure struttura la skill come plugin Cowork con `plugin.json`

Le skill in `ai-skills/` sono pensate per **agenti di coding** (Claude Code, Cursor, ecc.)
e si invocano nei rispettivi ambienti, non in questa chat.

---

## Workflow di gestione (riepilogo comandi npx)

```bash
npx skills add <repo>           # installa
npx skills list                 # lista installate
npx skills find [query]         # cerca
npx skills update [nome]        # aggiorna
npx skills remove [nome]        # rimuovi
npx skills init [nome]          # crea nuovo SKILL.md template
```

---

## Risorse

- [skills.sh](https://skills.sh) — directory di skill pubbliche
- [agentskills.io](https://agentskills.io) — specifica standard
- [vercel-labs/skills](https://github.com/vercel-labs/skills) — CLI
- [anthropics/skills](https://github.com/anthropics/skills) — skill ufficiali Anthropic
- [obra/superpowers](https://github.com/obra/superpowers) — framework metodologico

---

*Aggiornato: 2026-04-26 · Basato su vercel-labs/skills CLI v1.x*

---

## Come usare `skill-creator` in Cowork

`skill-creator` è la skill installata in Cowork per **creare, migliorare e ottimizzare skill**
invocabili con `/` in questa chat. Funziona con un ciclo iterativo: scrivi → testa → valuta → migliora.

### Come invocarla

Non serve un comando speciale. Basta descrivere cosa vuoi fare:

```
Voglio creare una skill per X
Aiutami a migliorare questa skill esistente
Ottimizza la descrizione di questa skill
```

`skill-creator` si attiva automaticamente e guida il processo.

---

### Il ciclo di lavoro (loop)

```
1. DEFINISCI    → cosa fa la skill, quando si attiva, che output produce
2. SCRIVI       → bozza del SKILL.md con name, description, istruzioni
3. TESTA        → 2-3 prompt realistici eseguiti con e senza la skill
4. VALUTA       → revisione output + metriche quantitative
5. MIGLIORA     → riscrittura basata sul feedback
6. RIPETI       → finché sei soddisfatto
7. OTTIMIZZA    → ottimizzazione automatica della description per il triggering
8. PACCHETTIZZA → genera il file .skill installabile
```

---

### Step 1 — Definisci l'intento

`skill-creator` ti farà queste domande:

- Cosa deve fare la skill?
- Quando deve attivarsi? (frasi tipiche dell'utente)
- Che formato di output produce?
- Vuoi test case per verificarla?

Se hai già una conversazione con un workflow che vuoi catturare, dì:
```
Trasforma questo workflow in una skill
```
Estrae autonomamente i passaggi dalla chat.

---

### Step 2 — Il SKILL.md generato

La skill creator produce un file con questa struttura:

```
nome-skill/
├── SKILL.md              ← istruzioni + frontmatter (obbligatorio)
├── evals/
│   └── evals.json        ← test case con prompt e assertions
└── scripts/              ← script riutilizzabili (opzionale)
    └── helper.py
```

Il frontmatter minimo:

```yaml
---
name: nome-skill
description: >
  Cosa fa la skill E quando usarla. La description è il meccanismo
  principale di attivazione — deve essere esplicita e un po' "pushy":
  es. "Usare sempre quando l'utente menziona X, Y o Z, anche se non
  lo chiede esplicitamente."
---
```

> **Regola chiave sulla description**: Claude tende a non attivare le skill
> abbastanza. La description deve includere sia cosa fa la skill, sia i
> contesti specifici in cui usarla — con esempi concreti di frasi trigger.

---

### Step 3 — Test case

`skill-creator` propone 2-3 prompt realistici e li esegue **con la skill** e
**senza la skill** (baseline) per confrontare i risultati.

I risultati vengono organizzati in:

```
nome-skill-workspace/
└── iteration-1/
    ├── eval-0/
    │   ├── with_skill/outputs/
    │   └── without_skill/outputs/
    └── benchmark.json
```

In Cowork (senza browser) il viewer viene generato come **file HTML statico**
che puoi aprire direttamente. Fornisce due tab:
- **Outputs** — vedi i risultati, lascia feedback caso per caso
- **Benchmark** — confronto quantitativo con/senza skill (pass rate, token, tempo)

---

### Step 4 — Valuta e itera

Dopo aver guardato i risultati nel viewer, torni in chat e dici cosa non va.
`skill-creator` generalizza il feedback (non fa patch narrow) e riscrive il SKILL.md.
Il ciclo riparte con `iteration-2/`, `iteration-3/`, ecc.

**Quando smettere**: quando il feedback è tutto vuoto, o quando sei soddisfatto.

---

### Step 5 — Ottimizzazione del triggering (opzionale)

Dopo che la skill funziona bene, puoi ottimizzare automaticamente la `description`
per massimizzare la precisione di attivazione:

```
Ottimizza la description di questa skill
```

`skill-creator` genera 20 query di test (metà should-trigger, metà should-not-trigger),
le presenta per revisione, poi esegue un loop automatico fino a 5 iterazioni che
trova la description con il miglior score su un test set held-out.

---

### Step 6 — Pacchettizzazione

Al termine, `skill-creator` genera un file `.skill` installabile:

```bash
# comando interno eseguito da skill-creator
python -m scripts.package_skill path/to/nome-skill/
```

Il file `.skill` risultante può essere installato direttamente in Cowork o
distribuito ad altri. Ti viene presentato con un link per scaricarlo.

---

### Aggiornare una skill esistente

Se vuoi migliorare una skill già installata invece di crearne una nuova:

```
Aggiorna / migliora questa skill: [nome o path]
```

`skill-creator` rispetta il nome originale (non crea versioni `-v2`),
copia la skill in una posizione scrivibile prima di modificarla,
e produce un nuovo `.skill` con lo stesso identificatore.

---

### Riepilogo comandi in chat

| Cosa vuoi fare | Cosa dire |
|----------------|-----------|
| Creare una skill nuova | "Crea una skill per X" |
| Catturare un workflow esistente | "Trasforma questo workflow in una skill" |
| Migliorare una skill | "Migliora / aggiorna la skill Y" |
| Ottimizzare il triggering | "Ottimizza la description della skill Y" |
| Solo scrivere la bozza, no test | "Scrivi solo la bozza della skill, niente eval" |
| Valutare senza eval quantitative | "Vediamo i risultati qualitativamente" |
