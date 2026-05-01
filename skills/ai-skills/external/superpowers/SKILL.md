---
name: superpowers
description: >
  Metodologia completa di sviluppo software per agenti AI. 14 skill composable
  che trasformano l'agente da "veloce digitatore" a partner di ingegneria
  disciplinato: TDD obbligatorio, design Socratico, debugging sistematico a
  4 fasi, subagent-driven development con review a due stadi.
  Supporta Claude Code, Codex, Cursor, OpenCode, Gemini CLI, GitHub Copilot.
role: potenziamento agenti
source: obra/superpowers
version: "v5.0.7"
version_date: "2026-03-31"
version_verified: 2026-04-26
license: MIT
update_url: https://github.com/obra/superpowers
marketplace_url: "https://claude.com/plugins/superpowers"
install_cmd: "/plugin install superpowers@claude-plugins-official"
community: "https://discord.gg/35wsABTejz"
---

## Scopo

Superpowers è una metodologia di sviluppo completa. Prima di scrivere codice,
l'agente verifica le skill applicabili. I workflow sono **obbligatori, non
suggerimenti**. L'agente non salta alla scrittura di codice — prima fa il design,
poi il piano, poi l'implementazione con TDD.

## Workflow base (7 step)

| Step | Skill | Cosa fa |
|------|-------|---------|
| 1 | `brainstorming` | Raffina l'idea con domande Socratiche, salva design doc |
| 2 | `using-git-worktrees` | Crea branch isolato, setup progetto, verifica test baseline |
| 3 | `writing-plans` | Piano con task da 2-5 min: file esatti, codice completo, passi di verifica |
| 4 | `subagent-driven-development` | Dispatcha subagenti per task, review a due stadi (spec + qualità) |
| 5 | `test-driven-development` | RED-GREEN-REFACTOR obbligatorio. Cancella codice scritto prima dei test |
| 6 | `requesting-code-review` | Review per severity tra task. I critical bloccano il progresso |
| 7 | `finishing-a-development-branch` | Verifica test, opzioni merge/PR/keep/discard, cleanup worktree |

## Skill library completa (14)

### Testing
- **`test-driven-development`** — ciclo RED-GREEN-REFACTOR, anti-patterns reference

### Debugging
- **`systematic-debugging`** — 4 fasi: root-cause-tracing, defense-in-depth, condition-based-waiting
- **`verification-before-completion`** — verifica che il problema sia davvero risolto

### Collaboration & Planning
- **`brainstorming`** — design Socratico, esplorazione alternative
- **`writing-plans`** — piani dettagliati, no placeholder, no TBD
- **`executing-plans`** — esecuzione a batch con checkpoint umani
- **`subagent-driven-development`** — iterazione veloce con review a due stadi
- **`dispatching-parallel-agents`** — workflow subagenti concorrenti
- **`requesting-code-review`** — checklist pre-review
- **`receiving-code-review`** — rispondere al feedback
- **`using-git-worktrees`** — branch di sviluppo parallelo
- **`finishing-a-development-branch`** — workflow merge/PR/discard

### Meta
- **`writing-skills`** — creare nuove skill con best practice e test
- **`using-superpowers`** — introduzione al sistema

## Filosofia

- **Test-Driven Development** — test prima, sempre
- **Systematic over ad-hoc** — processo sopra le intuizioni
- **Complexity reduction** — il codice più semplice che funziona
- **Evidence over claims** — verifica prima di dichiarare "fatto"

## Installazione

```bash
# Claude Code — marketplace ufficiale Anthropic (consigliato)
/plugin install superpowers@claude-plugins-official

# Claude Code — marketplace Superpowers
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace

# Cursor
/add-plugin superpowers

# GitHub Copilot CLI
copilot plugin marketplace add obra/superpowers-marketplace
copilot plugin install superpowers@superpowers-marketplace

# Gemini CLI
gemini extensions install https://github.com/obra/superpowers

# OpenCode (dì all'agente)
# Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md
```

## Aggiornamento

Gli aggiornamenti sono spesso automatici. Per forzare:

```bash
# Claude Code
/plugin update superpowers

# Gemini CLI
gemini extensions update superpowers
```

**Versione corrente:** v5.0.7 (2026-03-31)
**Changelog:** https://github.com/obra/superpowers/blob/main/RELEASE-NOTES.md

> **Nota:** non vengono accettate contribuzioni di nuove skill. Ogni modifica
> deve funzionare su tutti gli agenti supportati.

---

*Versione: v5.0.7 · Verificato: 2026-04-26 · Fonte: [obra/superpowers](https://github.com/obra/superpowers) · Licenza: MIT*
*Community: [Discord](https://discord.gg/35wsABTejz) · [Marketplace](https://claude.com/plugins/superpowers)*
