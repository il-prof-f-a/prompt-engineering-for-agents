---
name: anthropic-skills
description: >
  Repository ufficiale Anthropic per le skill di Claude. Copre UX/UI design,
  document creation (docx, pdf, pptx, xlsx), MCP builder, web testing,
  algorithmic art, brand guidelines, enterprise communications.
  Le skill document sono usate in produzione su Claude.ai.
role: UX
source: anthropics/skills
version_verified: 2026-04-26
license: "Apache 2.0 (skills/); source-available (document skills)"
update_url: https://github.com/anthropics/skills
install_cmd: "/plugin marketplace add anthropics/skills"
docs_url: "https://support.claude.com/en/articles/12512176-what-are-skills"
---

## Scopo

Repository ufficiale Anthropic con skill dimostrative e di produzione per Claude.
Copre creative design, sviluppo tecnico, enterprise workflow e document creation.
Le skill `docx`, `pdf`, `pptx`, `xlsx` sono quelle **effettivamente usate in
produzione su Claude.ai** per le capacità documentali.

## Skill disponibili (17)

### Creative & Design
| Skill | Descrizione |
|-------|-------------|
| `algorithmic-art` | Generazione di arte algoritmica |
| `canvas-design` | Design su canvas |
| `frontend-design` | UX/UI development tools e best practice |
| `theme-factory` | Generazione temi CSS e design system |

### Document Creation (produzione su Claude.ai)
| Skill | Descrizione |
|-------|-------------|
| `docx` | Creazione e modifica documenti Word |
| `pdf` | Creazione e manipolazione PDF |
| `pptx` | Creazione presentazioni PowerPoint |
| `xlsx` | Creazione e modifica spreadsheet Excel |

### Technical
| Skill | Descrizione |
|-------|-------------|
| `claude-api` | Uso dell'API Claude con best practice (Python, JS, Go, Java, Ruby, C#, PHP) |
| `mcp-builder` | Creazione server MCP |
| `skill-creator` | Creazione e ottimizzazione di nuove skill |
| `web-artifacts-builder` | Costruzione artefatti HTML complessi |
| `webapp-testing` | Test di web application |
| `slack-gif-creator` | Creazione GIF per Slack |
| `doc-coauthoring` | Co-authoring documenti |

### Enterprise & Communication
| Skill | Descrizione |
|-------|-------------|
| `brand-guidelines` | Applicazione brand guidelines aziendali |
| `internal-comms` | Comunicazioni interne aziendali |

## Installazione

### Tramite Plugin Marketplace (Claude Code)
```bash
# Registra il marketplace
/plugin marketplace add anthropics/skills

# Installa i pacchetti
/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
```

### Tramite npx skills
```bash
npx skills add anthropics/skills

# Skill specifica
npx skills add anthropics/skills --skill frontend-design
npx skills add anthropics/skills --skill mcp-builder
```

### Claude.ai
Le skill example sono già disponibili per i piani paid. Per skill custom,
seguire: https://support.claude.com/en/articles/12512180-using-skills-in-claude

### Claude API
Vedi: https://docs.claude.com/en/api/skills-guide

## Struttura di una skill (formato minimo)

```yaml
---
name: nome-skill
description: >
  Descrizione completa di cosa fa e quando usarla.
---

# Istruzioni per Claude

## Esempi
## Linee guida
```

## Note su licenze

- `skills/` → **Apache 2.0** (open source)
- `skills/docx`, `skills/pdf`, `skills/pptx`, `skills/xlsx` → **source-available** (non open source)

## Aggiornamento

```bash
npx skills update anthropic-skills
# oppure
/plugin update anthropic-agent-skills
```

Changelog e nuove skill: https://github.com/anthropics/skills

---

*Verificato: 2026-04-26 · Fonte: [anthropics/skills](https://github.com/anthropics/skills)*
*Docs: [What are skills?](https://support.claude.com/en/articles/12512176-what-are-skills)*
