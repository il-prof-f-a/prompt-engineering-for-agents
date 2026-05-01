---
name: vercel-agent-skills
description: >
  Collezione ufficiale Vercel di skill per lo sviluppo frontend.
  Copre React best practices (40+ regole), web design guidelines (100+ regole),
  React Native, View Transitions, composition patterns e deploy su Vercel.
  Attivazione automatica su task frontend rilevanti.
role: frontend
source: vercel-labs/agent-skills
version_verified: 2026-04-26
license: MIT
update_url: https://github.com/vercel-labs/agent-skills
install_cmd: "npx skills add vercel-labs/agent-skills"
---

## Scopo

Skill ufficiali Vercel che insegnano all'agente le best practice
per React, Next.js e frontend in generale. Si attivano automaticamente
quando l'agente rileva task rilevanti — non serve invocarle manualmente.

## Skill disponibili

### `react-best-practices`
40+ regole di performance React/Next.js da Vercel Engineering, divise in 8 categorie per priorità:

| Categoria | Priorità |
|-----------|---------|
| Eliminare waterfall | Critica |
| Bundle size | Critica |
| Performance server-side | Alta |
| Data fetching client-side | Media-Alta |
| Re-render optimization | Media |
| Rendering performance | Media |
| JS micro-ottimizzazioni | Bassa-Media |

**Trigger:** scrittura componenti React, data fetching, code review performance, ottimizzazione bundle.

### `web-design-guidelines`
Audit del codice UI su 100+ regole. Categorie: Accessibility, Focus States, Forms, Animation, Typography, Images, Performance, Navigation & State, Dark Mode, Touch & Interaction, Locale & i18n.

**Trigger:** "Review my UI", "Check accessibility", "Audit design", "Check my site against best practices".

### `react-native-guidelines`
16 regole per React Native / Expo su 7 sezioni: Performance (FlashList, memoization), Layout, Animation (Reanimated), Images (expo-image), State Management (Zustand), Architecture (monorepo), Platform (iOS/Android).

### `react-view-transitions`
View Transition API con React: `<ViewTransition>`, `addTransitionType`, shared element transitions, Next.js App Router `transitionTypes`, CSS pseudo-elements, animazioni JS via Web Animations API.

**Trigger:** page transitions, route animations, list-to-detail morphing, directional navigation.

### `composition-patterns`
Pattern di composizione React scalabili: compound components, state lifting, evitare boolean prop proliferation, prop drilling, API flessibili per librerie.

### `vercel-deploy-claimable`
Deploy istantaneo su Vercel con URL preview e URL di claim (trasferimento ownership). Auto-rileva 40+ framework da `package.json`.

**Trigger:** "Deploy my app", "Push this live", "Deploy and give me the link".

## Installazione

```bash
# Tutte le skill
npx skills add vercel-labs/agent-skills

# Skill specifiche
npx skills add vercel-labs/agent-skills --skill react-best-practices
npx skills add vercel-labs/agent-skills --skill web-design-guidelines

# Per agenti specifici
npx skills add vercel-labs/agent-skills -a claude-code -a cursor
```

## Struttura di ogni skill

```
<skill-name>/
├── SKILL.md        # istruzioni per l'agente
├── scripts/        # script automazione (opzionali)
└── references/     # documentazione di supporto (opzionale)
```

## Aggiornamento

```bash
npx skills update vercel-agent-skills
```

Verifica nuove skill: https://github.com/vercel-labs/agent-skills

---

*Verificato: 2026-04-26 · Fonte: [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) · Licenza: MIT*
