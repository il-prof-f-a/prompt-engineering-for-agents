# AI_HANDOFF.md

## Scopo

Preparare il passaggio di consegne tra modelli o tra sessioni senza perdita di contesto.

Questo prompt va usato alla fine di una sessione, soprattutto dopo modifiche a codice, architettura, database, API, configurazioni, documentazione o task.

---

## Obiettivo

Un modello che non conosce la chat deve poter continuare il lavoro leggendo solo `.llm/`.

Il passaggio di consegne deve aggiornare sia:

- memoria operativa;
- wiki di comprensione.

---

## Lettura iniziale obbligatoria

Prima di aggiornare, leggi:

1. `.llm/index.md`
2. `.llm/pages/current-state.md`
3. `.llm/pages/tasks-todo.md`
4. `.llm/pages/session-handoff.md`
5. `.llm/pages/decisions.md`
6. `.llm/pages/architecture.md`
7. `.llm/pages/open-questions.md`
8. `.llm/logs/operations-log.md`

Poi leggi le pagine specifiche impattate dal lavoro:

- `data-model.md`
- `api.md`
- `frontend.md`
- `backend.md`
- `deployment.md`
- `security.md`
- altre pagine create nel grafo.

---

## File da aggiornare sempre

### `.llm/pages/current-state.md`

Aggiorna:

- stato attuale;
- cosa funziona;
- cosa resta incompleto;
- ultimi file modificati;
- problemi aperti;
- impatto sull’architettura;
- eventuali verifiche eseguite.

---

### `.llm/pages/session-handoff.md`

Aggiorna in modo operativo:

- cosa è stato fatto;
- stato preciso;
- prossimi step numerati;
- cosa non rifare;
- cosa controllare prima di proseguire;
- rischi di regressione;
- stato di `architecture.md`.

---

### `.llm/pages/tasks-todo.md`

Aggiorna:

- task completati in `DONE`;
- task avviati in `DOING`;
- task futuri in `TODO`;
- eventuale impatto architetturale;
- eventuali blocchi o dipendenze.

---

### `.llm/logs/operations-log.md`

Aggiungi una voce append-only:

```md
## YYYY-MM-DD HH:mm - Handoff

**Tipo:** handoff
**File modificati:** ...
**Pagine aggiornate:** ...
**Decisioni aggiunte:** ...
**Verifiche eseguite:** ...
**Problemi aperti:** ...
**Prossimi step:** ...
```

---

## File da aggiornare se necessario

### `.llm/pages/architecture.md`

Aggiornalo solo se il lavoro ha modificato o chiarito realmente:

- struttura del sistema;
- responsabilità dei moduli;
- flussi applicativi;
- flussi dati;
- interazioni tra sottosistemi;
- dipendenze;
- integrazioni;
- punti di ingresso.

Se non ci sono cambiamenti architetturali reali, non riscriverlo inutilmente.

Se l’architettura resta poco chiara, segnalarlo in `session-handoff.md` e `open-questions.md`.

---

### `.llm/pages/decisions.md`

Aggiorna solo se sono emerse vere decisioni tecniche o architetturali.

Non registrare come decisione:

- semplici tentativi;
- ipotesi;
- modifiche banali;
- preferenze non confermate.

---

### `.llm/pages/open-questions.md`

Aggiorna se emergono:

- contraddizioni;
- dubbi;
- parti non comprese;
- decisioni rimandate;
- rischi non risolti.

---

### Pagine specifiche

Aggiorna quando coinvolte:

- `data-model.md` per DB, schema, entità, file dati;
- `api.md` per endpoint, payload, protocolli, interfacce;
- `frontend.md` per UI, viste, flussi utente;
- `backend.md` per servizi, logica, job, script;
- `deployment.md` per installazione, avvio, ambiente;
- `security.md` per autenticazione, permessi, segreti, validazione, rischi.

---

## Controllo finale di coerenza

Prima di chiudere, verifica:

1. `current-state.md` è coerente con `architecture.md`?
2. `tasks-todo.md` non contiene task già completati?
3. `session-handoff.md` permette a un nuovo modello di ripartire?
4. `decisions.md` contiene solo decisioni verificabili?
5. `open-questions.md` contiene i dubbi ancora reali?
6. `operations-log.md` ha una voce per questa sessione?
7. `index.md` punta alle nuove pagine eventualmente create?

Se hai creato nuove pagine, aggiorna `index.md`.

---

## Stile di scrittura

Scrivi per un altro modello, non per un umano generico.

Usa frasi operative, precise, verificabili.

Evita:

- riassunti vaghi;
- entusiasmo;
- spiegazioni narrative;
- “probabilmente” non motivati;
- affermazioni non verificate.

Quando qualcosa è dedotto, scrivi:

```md
[Deducibile dal codice, non confermato da documentazione]
```

Quando qualcosa non è certo, scrivi:

```md
[Da verificare]
```

---

## Output finale richiesto in chat

Rispondi con:

1. pagine aggiornate;
2. stato di `architecture.md`;
3. task rimasti;
4. rischi o dubbi;
5. prossimo step consigliato.
