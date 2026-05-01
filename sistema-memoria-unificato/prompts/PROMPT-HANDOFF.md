# PROMPT-HANDOFF

## TASK

Stai chiudendo una sessione di lavoro. Devi **aggiornare la memoria persistente** in modo
che il prossimo agente (o la prossima sessione) possa riprendere senza perdita di
contesto leggendo solo `AGENT.md` + `.ai/`.

Criteri di successo:
- nessun file immutabile modificato
- nessuna informazione valida cancellata
- aggiornamento minimo, mirato, operativo
- log delle operazioni aggiornato

---

## FASE 0 — REGOLE NON NEGOZIABILI

- **NON modificare** `.ai/project-spec.md` (immutabile, solo l'utente).
- **NON sovrascrivere** `decisions.md` né `operations-log.md`: sono append-only.
- **NON riscrivere** `project-stack.md` o `company-standards.md` se non sono cambiati
  realmente: politica merge-only.
- **NON fare ingest in `llm-wiki/`** durante l'handoff. La wiki si aggiorna solo su
  richiesta esplicita dell'utente, mai in handoff implicito.
- **NON scrivere per un umano**, scrivi per un altro agente: frasi operative,
  verificabili, niente narrazione.

---

## FASE 1 — LETTURA INIZIALE OBBLIGATORIA

Prima di aggiornare, leggi:

1. `AGENT.md`
2. `.ai/current-state.md`
3. `.ai/tasks-todo.md`
4. `.ai/session-handoff.md`
5. `.ai/decisions.md`
6. `.ai/project-stack.md`
7. `.ai/open-questions.md`
8. `.ai/operations-log.md`

Poi, solo se il lavoro le ha toccate, leggi le pagine specifiche di `llm-wiki/pages/`
coinvolte.

---

## FASE 2 — INVENTARIO DELLA SESSIONE

In risposta interna (non scrivere ancora niente sui file), prepara mentalmente:

- file di codice modificati (lista esatta)
- moduli/aree toccate
- decisioni tecniche o architetturali nuove (verificabili)
- task spostati di stato (TODO → DOING → DONE)
- problemi aperti emersi
- impatto su architettura / API / schema dati / sicurezza / deployment
- verifiche eseguite (test passati, lint, build, manual check)

---

## FASE 3 — AGGIORNAMENTI OBBLIGATORI (ogni handoff)

### 3.1 `.ai/current-state.md` (overwrite-controlled)

Aggiorna preservando ciò che è ancora valido:
- stato attuale (post-sessione)
- cosa funziona / cosa è incompleto
- ultimi file modificati (path)
- problemi aperti
- impatto sull'architettura
- verifiche eseguite in sessione

### 3.2 `.ai/session-handoff.md` (overwrite)

Sostituisci interamente il contenuto con la nuova versione. Deve permettere a un agente
senza chat di ripartire. Sezioni:

```md
---
update_policy: overwrite
last_session: YYYY-MM-DD
agent: [nome agente / modello]
---

# Session Handoff

## Cosa è stato fatto
- [bullet operativi, max 7-10]

## Stato preciso post-sessione
- [3-5 righe]

## Prossimi step numerati
1. ...
2. ...

## Cosa NON rifare
- [errori già fatti, strade già percorse senza successo]

## Cosa controllare prima di proseguire
- [verifiche necessarie, file da rileggere]

## Rischi di regressione noti
- [aree fragili toccate, dipendenze sensibili]

## Stato di project-stack.md
[confermato | aggiornato | invariato | da rivedere | non aggiornabile per dubbi]
```

### 3.3 `.ai/tasks-todo.md` (overwrite-controlled)

- task completati → DONE
- task avviati ma non finiti → DOING (con nota su dove ci si è fermati)
- task nuovi emersi → TODO
- task non più rilevanti → rimuovere con annotazione in `decisions.md` se la rimozione è
  una decisione

### 3.4 `.ai/operations-log.md` (append-only)

Aggiungi UNA voce nuova in coda:

```md
## YYYY-MM-DD HH:mm — Handoff

**Tipo:** handoff
**Agente:** [nome]
**File codice modificati:** [lista]
**Pagine memoria aggiornate:** [lista]
**Decisioni aggiunte:** [N o "nessuna"]
**Verifiche eseguite:** [test, lint, build, manual]
**Problemi aperti:** [N riferiti a open-questions.md]
**Prossimi step:** [bullet sintetici]
```

---

## FASE 4 — AGGIORNAMENTI CONDIZIONATI (solo se serve)

### 4.1 `.ai/decisions.md` (append-only)

Aggiungi UNA voce SOLO se è emersa una decisione tecnica/architetturale **verificabile**.

Non sono decisioni:
- semplici tentativi
- ipotesi
- modifiche banali
- preferenze stilistiche temporanee

Formato:

```md
## YYYY-MM-DD — [Titolo decisione]

**Contesto:** ...
**Decisione:** ...
**Motivazione:** ...
**Conseguenze:** ...
**Fonti:** [link a codice, file, issue]
```

### 4.2 `.ai/project-stack.md` (merge-only)

Aggiorna SOLO se il lavoro ha modificato o chiarito realmente:
- struttura del sistema
- responsabilità dei moduli
- flussi applicativi o flussi dati
- punti di ingresso
- dipendenze interne / esterne
- integrazioni / API / servizi
- aree sensibili

Se l'architettura è ancora poco chiara, **non inventare**: aggiorna `open-questions.md`
e segnalalo nel handoff.

### 4.3 `.company/company-standards.md` (merge-only)

Aggiorna SOLO se durante la sessione è emerso che uno standard aziendale è cambiato. Caso
raro. Se è il caso, aggiungi anche una decisione in `decisions.md`.

### 4.4 `.ai/open-questions.md` (merge-only)

- aggiungi nuovi dubbi emersi
- chiudi (o sposta a "risolti") i dubbi precedenti se la sessione li ha sciolti
- per ogni voce mantieni: domanda, contesto, file coinvolti, impatto, priorità

### 4.5 `AGENT.md` (a root)

Aggiorna SOLO se durante la sessione:
- sono stati creati nuovi file di memoria che vanno indicizzati
- è cambiata la struttura della cartella di memoria
- è cambiata una politica di scrittura

Non riscriverlo per un handoff normale.

### 4.6 `llm-wiki/`

Toccare la wiki **solo se la sessione conteneva un'operazione di ingest esplicita**. In
quel caso:
- aggiorna `llm-wiki/index.md`
- aggiungi una voce in `llm-wiki/log.md`

Altrimenti, lasciare `llm-wiki/` invariata.

---

## FASE 5 — CHECKLIST DI COERENZA FINALE

Prima di chiudere, verifica:

1. `current-state.md` è coerente con `project-stack.md`?
2. `tasks-todo.md` non contiene task già completati nella sessione?
3. `session-handoff.md` permette a un agente senza chat di riprendere autonomamente?
4. `decisions.md` contiene solo decisioni verificabili?
5. `open-questions.md` riflette lo stato dei dubbi (aggiunti i nuovi, chiusi i risolti)?
6. `operations-log.md` ha una nuova voce per questa sessione?
7. `AGENT.md` è ancora un hub minimale (no contenuto inline duplicato)?
8. `project-spec.md` è invariato?
9. La `llm-wiki/` è invariata (salvo ingest esplicito in sessione)?

Se anche un solo punto non è verificato, sistema prima di chiudere.

---

## FASE 6 — STILE DI SCRITTURA

Per ogni file aggiornato:

- frasi operative, non descrittive
- niente entusiasmo
- niente "probabilmente" non motivati
- niente affermazioni non verificate
- segna esplicitamente:
  - `[Deducibile dal codice, non confermato da documentazione]`
  - `[Da verificare]`
  - `[Confermato dall'utente in sessione YYYY-MM-DD]`

---

## FASE 7 — OUTPUT FINALE IN CHAT

Termina la chat con un messaggio compatto, max 12 righe:

1. **File memoria aggiornati:** [lista con path]
2. **Decisioni nuove:** [N o "nessuna"]
3. **Stato di `project-stack.md`:** [aggiornato / invariato / da rivedere]
4. **Task rimasti in TODO:** [N]
5. **Open questions aperte:** [N]
6. **Rischi di regressione segnalati:** [N]
7. **Prossimo step suggerito** per la prossima sessione: [una riga]

NON ripetere il contenuto dei file: l'utente lo trova nei file stessi. Il messaggio in
chat è solo l'indice del lavoro fatto.
