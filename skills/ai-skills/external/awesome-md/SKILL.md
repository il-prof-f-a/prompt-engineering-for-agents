---
name: awesome-md
description: >
  14 skill opinionated per generare diagrammi e visualizzazioni direttamente
  in Markdown, senza strumenti esterni. Copre 5 motori di rendering:
  PlantUML, Vega-Lite, JSON Canvas, HTML/CSS embedded, Infographic YAML.
  Compatibile con Claude Code, Codex, Cursor e altri agenti.
role: documentazione
source: markdown-viewer/skills
version_verified: 2026-04-26
license: GPL-3.0
update_url: https://github.com/markdown-viewer/skills
install_cmd: "npx skills add markdown-viewer/skills"
---

## Scopo

Skill per agenti AI che generano diagrammi e visualizzazioni professionali
direttamente in file Markdown. L'agente sceglie la skill giusta in base al
tipo di diagramma richiesto e segue le istruzioni del SKILL.md specifico.

## Skill disponibili (14 totali)

### Standalone (motori indipendenti)

| Skill | Code fence | Ideale per |
|-------|-----------|------------|
| `vega` | ` ```vega-lite ` / ` ```vega ` | Bar, line, scatter, heatmap, radar, word cloud |
| `infographic` | ` ```infographic ` | 70+ template YAML: KPI, timeline, roadmap, SWOT, funnel |
| `canvas` | ` ```canvas ` | Mind map, knowledge graph, concept map (JSON Canvas) |

### HTML/CSS embedded (no code fence)

| Skill | Template | Ideale per |
|-------|----------|------------|
| `architecture` | 13 layout × 12 stili | Layer diagram: User→App→Data→Infra, microservizi |
| `infocard` | 13 layout × 14 stili | Card editoriali, data highlight, annunci |

### PlantUML (` ```plantuml ` o ` ```puml `)

| Skill | Ideale per |
|-------|------------|
| `uml` | 14 tipi UML + 9500 icone mxgraph, software modeling |
| `cloud` | AWS, Azure, GCP, Alibaba, IBM, OpenStack, Kubernetes |
| `network` | LAN/WAN, data center, Cisco/Citrix device icons |
| `security` | IAM, firewall, threat model, zero-trust, compliance |
| `archimate` | Enterprise architecture, ArchiMate layered modeling |
| `bpmn` | BPMN, EIP, Lean Mapping, workflow automation |
| `data-analytics` | ETL/ELT pipeline, data warehouse, ML workflow |
| `iot` | IoT, edge computing, smart home/factory, digital twin |
| `mindmap` | Mind map gerarchico con rami direzionali |

## Guida alla selezione rapida

| Necessità | Skill |
|-----------|-------|
| Flowchart / process flow | `uml` (activity diagram) |
| Sequence / state / class | `uml` |
| Bar / line / scatter | `vega` |
| KPI dashboard / roadmap | `infographic` |
| Mind map libero | `canvas` |
| Mind map gerarchico | `mindmap` |
| Layer system diagram | `architecture` |
| Cloud infrastructure | `cloud` |
| Network topology | `network` |
| Threat model | `security` |
| Enterprise (ArchiMate) | `archimate` |
| Data pipeline / ETL | `data-analytics` |
| IoT / Edge | `iot` |
| Card editoriale | `infocard` |

## Installazione

```bash
# Tutti gli agenti supportati
npx skills add markdown-viewer/skills

# Skill singola
npx skills add markdown-viewer/skills --skill vega

# Manuale per Claude Code
cp -r skills/<skill-name> ~/.claude/skills/
```

## Struttura di ogni skill

```
<skill-name>/
├── SKILL.md          # istruzioni + frontmatter per l'agente
├── examples/         # esempi (skill PlantUML)
├── references/       # spec sintassi (canvas, vega, infographic)
├── layouts/          # template layout (architecture, infocard)
└── styles/           # template colori (architecture, infocard)
```

## Aggiornamento

```bash
npx skills update markdown-viewer/skills
```

Verifica versione e changelog: https://github.com/markdown-viewer/skills

---

*Verificato: 2026-04-26 · Fonte: [markdown-viewer/skills](https://github.com/markdown-viewer/skills) · Licenza: GPL-3.0*
