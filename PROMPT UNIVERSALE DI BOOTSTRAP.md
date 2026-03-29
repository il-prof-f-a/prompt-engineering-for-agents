## TASK

Devi inizializzare o aggiornare il sistema di memoria persistente del progetto nella cartella `.ai/`.

L'obiettivo è permettere a modelli diversi (Claude, GPT, Gemini, modelli locali) di lavorare senza perdere contesto.

---

## ANALISI INIZIALE (OBBLIGATORIA)

1. Analizza l'intera cartella progetto

2. Identifica:
   
   - linguaggi utilizzati
   
   - struttura del codice
   
   - file principali
   
   - eventuali README o documentazione
   
   - ultime modifiche (se deducibili)

3. Verifica se esiste già la cartella `.ai/`

---

## DECISIONE MODALITÀ

### CASO A → `.ai/` NON ESISTE

Modalità: INIT

### CASO B → `.ai/` ESISTE

Modalità: UPDATE

---

## COMPORTAMENTO

### 🔹 MODALITÀ INIT

Devi:

1. creare la cartella `.ai/`

2. generare i seguenti file:
- `.ai/project_brief.md`

- `.ai/current_state.md`

- `.ai/decisions.md`

- `.ai/session_handoff.md`

- `.ai/tasks_todo.md`

- `.ai/model_prefs.md`

---

### 🔹 MODALITÀ UPDATE

Devi:

1. leggere tutti i file `.ai/`

2. verificare coerenza con il codice

3. aggiornare SOLO se necessario

4. NON sovrascrivere informazioni valide

---

## GENERAZIONE CONTENUTI

### 📄 `.ai/project_brief.md`

Deve contenere:

- descrizione progetto

- obiettivo

- stack tecnologico

- struttura cartelle

- componenti principali

Includi anche questa sezione opzionale (se non utilizzata, lasciarla vuota):

```
## NOTE TECNICHE (OPZIONALE)
[Inserire eventuali indicazioni manuali specifiche sul progetto]
```

---

### 📄 `.ai/current_state.md`

Deve descrivere:

- stato attuale del progetto

- cosa funziona

- cosa non è completo

- ultime aree di lavoro (dedotte dal codice)

---

### 📄 `.ai/decisions.md`

Se vuoto:

- inizializzalo

Se esiste:

- NON inventare decisioni

- aggiungi solo se deducibili chiaramente

---

### 📄 `.ai/session_handoff.md`

Se INIT:

- descrivi stato iniziale

- indica possibili prossimi sviluppi

Se UPDATE:

- sintetizza stato attuale

- prepara continuità per prossimo modello

---

### 📄 `.ai/tasks_todo.md`

Genera:

- task attivi dedotti dal codice

- eventuali miglioramenti logici

- bug potenziali (se evidenti)

Formato:

- TODO

- DOING

- DONE

---

### 📄 `.ai/model_prefs.md`

Definisci uso modelli:

- Claude → architettura, reasoning

- GPT → codice, refactoring

- locale → debug, test veloci

---

## REGOLE CRITICHE

- NON inventare funzionalità non presenti

- NON fare supposizioni arbitrarie

- se qualcosa non è chiaro, dichiaralo

- privilegia informazioni verificabili dal codice

---

## OUTPUT

1. Elenca le informazioni chiave che hai dedotto dal progetto

2. Mostra i file `.ai/` generati o aggiornati

3. Spiega eventuali incertezze

---

## ALLINEAMENTO

Prima di scrivere file:

- riassumi cosa hai capito del progetto (max 10 righe)

- attendi conferma

DOPO conferma:

- genera i file
