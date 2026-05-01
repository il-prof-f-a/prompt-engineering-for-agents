---
name: vercel-skills-cli
description: >
  CLI open-source per l'ecosistema agent skills. Installa, aggiorna,
  rimuove e scopre skill da qualsiasi repository GitHub/GitLab per
  tutti gli agenti AI (44+ agenti supportati). Il registry manager
  dell'ecosistema skills.
role: discovery
source: vercel-labs/skills
version_verified: 2026-04-26
license: MIT
update_url: https://github.com/vercel-labs/skills
install_cmd: "npx skills --help"
skills_directory: "https://skills.sh"
---

## Scopo

`npx skills` è il tool CLI dell'ecosistema open agent skills.
Non è una skill da attivare direttamente, ma lo strumento per
**trovare, installare e gestire** tutte le altre skill.

Supporta 44+ agenti: Claude Code, Codex, Cursor, OpenCode, Gemini CLI,
GitHub Copilot, Windsurf, Roo Code, Cline, e molti altri.

## Comandi principali

```bash
# Installare skill da un repo
npx skills add vercel-labs/agent-skills

# Listare le skill disponibili in un repo (senza installarle)
npx skills add vercel-labs/agent-skills --list

# Installare skill specifiche
npx skills add vercel-labs/agent-skills --skill react-best-practices

# Installare per agenti specifici
npx skills add vercel-labs/agent-skills -a claude-code -a cursor

# Installare globalmente (tutti i progetti)
npx skills add vercel-labs/agent-skills -g

# Installazione CI/CD non-interattiva
npx skills add vercel-labs/agent-skills --skill react-best-practices -g -a claude-code -y

# Cercare skill
npx skills find [query]

# Listare skill installate
npx skills list

# Aggiornare skill
npx skills update [nome-skill]

# Rimuovere skill
npx skills remove [nome-skill]

# Creare un nuovo template SKILL.md
npx skills init [nome]
```

## Sorgenti supportate

```bash
npx skills add owner/repo                              # GitHub shorthand
npx skills add https://github.com/owner/repo           # URL GitHub completo
npx skills add https://github.com/owner/repo/tree/main/skills/my-skill  # path diretto
npx skills add https://gitlab.com/org/repo             # GitLab
npx skills add git@github.com:owner/repo.git           # qualsiasi URL git
npx skills add ./my-local-skills                       # path locale
```

## Opzioni installazione

| Flag | Descrizione |
|------|-------------|
| `-g, --global` | Installa globalmente in `~/<agent>/skills/` |
| `-a, --agent` | Target agenti specifici |
| `-s, --skill` | Installa skill specifiche (usa `'*'` per tutte) |
| `-l, --list` | Lista disponibili senza installare |
| `--copy` | Copia file invece di symlink |
| `-y, --yes` | Skip prompts (CI/CD) |
| `--all` | Installa tutto su tutti gli agenti |

## Scope e path per agente

| Agente | Path project | Path global |
|--------|-------------|-------------|
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| Codex | `.agents/skills/` | `~/.codex/skills/` |
| Cursor | `.agents/skills/` | `~/.cursor/skills/` |
| Gemini CLI | `.agents/skills/` | `~/.gemini/skills/` |
| Windsurf | `.windsurf/skills/` | `~/.codeium/windsurf/skills/` |

## Discover: Skills Directory

Scopri skill disponibili su: **https://skills.sh**

## Aggiornamento CLI

```bash
npx skills update           # aggiorna tutte le skill installate
npx skills update my-skill  # aggiorna skill specifica
```

Changelog: https://github.com/vercel-labs/skills

---

*Verificato: 2026-04-26 · Fonte: [vercel-labs/skills](https://github.com/vercel-labs/skills) · Licenza: MIT*
