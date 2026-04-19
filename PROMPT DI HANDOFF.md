## TASK

Preparare passaggio di consegne tra modelli senza perdita di contesto.

---

## FILE DA AGGIORNARE

### .ai/current_state.md

- stato aggiornato
- ultimi file modificati
- problemi aperti
- eventuale impatto sul quadro architetturale

---

### .ai/session_handoff.md

- cosa è stato fatto
- stato attuale preciso
- prossimi step numerati
- cosa NON rifare
- eventuale stato di `architecture.md` (aggiornato / invariato / da rivedere)

---

### .ai/decisions.md

- aggiungi nuove decisioni tecniche
- aggiungi decisioni architetturali solo se realmente emerse e verificabili

---

### .ai/tasks_todo.md

- aggiorna task

---

### .ai/architecture.md

Aggiornalo solo se il lavoro svolto ha modificato o chiarito realmente:

- struttura del sistema
- responsabilità dei moduli
- flussi applicativi
- flussi dati
- interazioni tra sottosistemi
- dipendenze o integrazioni

Se non ci sono cambiamenti architetturali reali, NON riscriverlo inutilmente.

Se l'architettura è ancora poco chiara, segnalarlo nel handoff invece di inventare.

---

## REGOLE

- scrivi in modo operativo
- evita riassunti vaghi
- scrivi per un altro modello (NON per un umano)
- mantieni sincronizzati stato, task, decisioni e architettura
- non dichiarare come certa una struttura che non è stata verificata

---

## CONTROLLO

Verifica che un modello senza chat possa continuare solo leggendo `.ai/`.

Checklist minima:

- il quadro architetturale è comprensibile?
- lo stato attuale è coerente con l'architettura?
- i prossimi step dipendono da qualche vincolo strutturale?
- c'è qualcosa che il prossimo modello rischierebbe di rompere?
