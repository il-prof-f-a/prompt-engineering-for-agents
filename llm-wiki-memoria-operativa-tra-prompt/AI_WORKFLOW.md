# AI_WORKFLOW.md

## Scopo

Guidare una sessione di lavoro su un progetto che usa la struttura `.llm/`.

Questo prompt serve quando l’agente deve continuare sviluppo, analisi, refactoring, debug, documentazione o progettazione senza perdere continuità tra modelli.

---

## Regola principale

Non iniziare a modificare il progetto senza avere prima letto la memoria persistente.

La memoria non è separata dalla wiki: è parte dello stesso grafo.

Le pagine knowledge spiegano.
Le pagine operative guidano.
Il log registra.
Le fonti provano.

---

## Lettura obbligatoria iniziale

Leggi in questo ordine:

1. `.llm/index.md`
2. `.llm/pages/project-brief.md`
3. `.llm/pages/overview.md`
4. `.llm/pages/architecture.md`
5. `.llm/pages/current-state.md`
6. `.llm/pages/decisions.md`
7. `.llm/pages/tasks-todo.md`
8. `.llm/pages/session-handoff.md`
9. `.llm/pages/open-questions.md`

Poi leggi le altre pagine rilevanti in base al task:

- `data-model.md`
- `api.md`
- `frontend.md`
- `backend.md`
- `deployment.md`
- `security.md`

---

## Controllo di coerenza

Prima di proporre modifiche verifica:

1. lo stato operativo è coerente con l’architettura?
2. i task attivi sono coerenti con le decisioni?
3. ci sono domande aperte che bloccano il lavoro?
4. il codice conferma ciò che dice la wiki?
5. il task richiesto impatta architettura, API, DB, sicurezza o deployment?

Se trovi divergenze:

- dichiarale subito;
- non correggere a caso;
- indica quale pagina va aggiornata;
- se il codice contraddice la wiki, considera il codice più affidabile ma segnala il disallineamento.

---

## Comportamento all’avvio della sessione

Non iniziare subito.

Rispondi prima con:

1. massimo 3 domande di chiarimento, solo se necessarie;
2. le 3 regole più importanti dedotte dalla wiki;
3. riassunto del task attuale;
4. valutazione: l’architettura è sufficiente per procedere?
5. piano massimo in 5 step.

Attendi conferma se il lavoro modifica codice, architettura, database, API o comportamento applicativo.

Puoi procedere senza conferma solo per attività di sola lettura, analisi o documentazione non distruttiva.

---

## Regole durante il lavoro

- Non reinventare parti già definite.
- Non ignorare decisioni precedenti.
- Non modificare architettura senza aggiornare `architecture.md`.
- Non modificare API senza aggiornare `api.md`.
- Non modificare DB o strutture dati senza aggiornare `data-model.md`.
- Non modificare autenticazione, validazione, permessi o gestione segreti senza aggiornare `security.md`.
- Non lasciare task completati in `TODO`.
- Non dichiarare completato ciò che non è stato verificato.
- Se fai una scelta tecnica nuova, registrala in `decisions.md`.

---

## Aggiornamenti obbligatori dopo modifiche

Se modifichi codice, configurazioni o documentazione, aggiorna almeno:

- `current-state.md`
- `tasks-todo.md`
- `session-handoff.md`
- `operations-log.md`

Inoltre aggiorna, se coinvolte:

- `architecture.md`
- `data-model.md`
- `api.md`
- `frontend.md`
- `backend.md`
- `deployment.md`
- `security.md`
- `decisions.md`
- `open-questions.md`

---

## Politica sulle pagine

### Knowledge pages

Esempi:

- `overview.md`
- `architecture.md`
- `data-model.md`
- `api.md`
- `frontend.md`
- `backend.md`
- `deployment.md`
- `security.md`

Politica:

- aggiornamento `merge-only`;
- non rimuovere informazioni ancora valide;
- separare fatti, deduzioni e ipotesi;
- aggiungere collegamenti interni.

---

### Operational pages

Esempi:

- `current-state.md`
- `tasks-todo.md`
- `session-handoff.md`

Politica:

- aggiornamento `overwrite-controlled`;
- devono restare brevi, operative e leggibili;
- servono al prossimo agente per agire subito.

---

### Append-only pages

Esempi:

- `decisions.md`
- `operations-log.md`

Politica:

- non cancellare voci precedenti;
- aggiungere nuove voci in ordine cronologico;
- se una decisione cambia, aggiungere una nuova decisione che spiega il superamento della precedente.

---

## Standard per modifiche al codice

Prima:

1. capisci il punto di ingresso;
2. controlla dipendenze;
3. verifica eventuali test;
4. identifica rischio di regressione;
5. proponi piano.

Durante:

1. modifica il minimo necessario;
2. evita refactoring non richiesti;
3. preserva stile del progetto;
4. segnala codice fragile o duplicato.

Dopo:

1. esegui test o controlli disponibili;
2. aggiorna `.llm/`;
3. prepara handoff.

---

## Output finale richiesto

Alla fine della sessione rispondi con:

1. cosa è stato fatto;
2. file modificati;
3. test o verifiche eseguite;
4. pagine `.llm/` aggiornate;
5. decisioni aggiunte;
6. problemi aperti;
7. prossimi step.
