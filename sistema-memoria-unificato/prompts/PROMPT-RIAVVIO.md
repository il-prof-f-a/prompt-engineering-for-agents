# PROMPT-RIAVVIO

## TASK

Stai entrando in una sessione di lavoro su un progetto che usa il sistema di memoria
unificato (`.ai/`, `.company/`, `llm-wiki/`, `AGENT.md`).

Devi **contestualizzarti leggendo SOLO ciò che serve**, proporre un piano e attendere
conferma. Non iniziare a modificare nulla finché l'utente non conferma.

Criteri di successo:
- nessuna regressione
- coerenza con decisioni esistenti
- minimo consumo di token (lettura on-demand, non a tappeto)
- nessun lavoro non confermato

---

## FASE 1 — LETTURA OBBLIGATORIA (in ordine, fermarsi appena il task è chiaro)

Apri e leggi i file in quest'ordine. Fermati appena hai contesto sufficiente per
rispondere o pianificare.

1. `AGENT.md` (a root) — hub di interlink, regole di lettura
2. `.ai/session-handoff.md` — cosa è successo l'ultima volta, prossimi step
3. `.ai/current-state.md` — stato operativo attuale
4. `.ai/tasks-todo.md` — task attivi
5. `.ai/project-stack.md` — stack, framework, metodologia, architettura
6. `.ai/project-spec.md` — obiettivo immutabile del progetto
7. `.company/company-standards.md` — modus operandi aziendale

Carica solo se rilevante per il task corrente:
8. `.ai/decisions.md` — decisioni passate
9. `.ai/open-questions.md` — dubbi aperti
10. `llm-wiki/index.md` + pagine specifiche — solo se il task riguarda contenuto
    documentale già ingestito

**Mai leggere alla cieca tutta `llm-wiki/`.** Mai aprire i file `raw/`. Aprire solo le
pagine indicizzate effettivamente rilevanti.

---

## FASE 2 — CONTROLLO DI COERENZA

Prima di proporre qualsiasi azione, verifica internamente:

1. lo stato operativo (`current-state.md`) è coerente con l'architettura
   (`project-stack.md`)?
2. i task in `tasks-todo.md` non sono in conflitto con `decisions.md`?
3. ci sono `open-questions.md` aperte che bloccano il task corrente?
4. il task richiesto dall'utente impatta architettura, stack, API, schema dati,
   sicurezza o deployment?
5. il task è coperto dagli standard in `company-standards.md`, oppure va in deroga (e in
   tal caso `project-stack.md` lo dichiara)?

Se trovi divergenze:
- dichiarale subito in chat;
- non correggere a caso;
- indica quale file di memoria andrebbe aggiornato e quando (durante l'handoff o
  prima);
- se il codice contraddice la memoria, considera il codice più affidabile e segnala il
  disallineamento.

---

## FASE 3 — RISPOSTA STRUTTURATA OBBLIGATORIA

Non iniziare a modificare nulla. Rispondi in chat con queste 5 sezioni, esattamente in
quest'ordine:

### 1. Domande di chiarimento (massimo 3, solo se necessarie)

Solo le domande che non puoi risolvere leggendo memoria + codice. Se non ce ne sono,
scrivi "Nessuna".

### 2. Le 3 regole più importanti dedotte dalla memoria

Tre bullet, frasi operative, non descrittive. Esempio:
- "Tutti i test devono passare prima del merge — politica aziendale (`company-standards.md`)."
- "L'API esposta è OpenAPI 3.1 — ogni modifica richiede update di `pages/api.md` (`project-stack.md`)."
- "Il client X richiede output in PDF/A-3 — vincolo non negoziabile (`project-spec.md`)."

### 3. Riassunto del task attuale

In 2-4 righe: cosa l'utente sta chiedendo, cosa significa concretamente nel contesto del
progetto, quali file/moduli probabilmente coinvolti.

### 4. Valutazione architettura

Una riga:
- "Architettura sufficiente per procedere" — oppure
- "Architettura parzialmente chiara, serve chiarimento su: [punto specifico]" — oppure
- "Architettura non chiara, fermo qui finché non si aggiorna `project-stack.md`."

### 5. Piano in massimo 5 step

Step numerati, operativi, verificabili. Per ogni step indica:
- cosa fa
- file/moduli toccati
- impatto: nessuno / codice non strutturale / strutturale / architetturale

Se almeno uno step è "strutturale" o "architetturale", aggiungi uno step finale esplicito
per aggiornare `project-stack.md` e `decisions.md`.

---

## FASE 4 — ATTESA DELLA CONFERMA

Termina la risposta con:

> **In attesa di conferma per procedere.** Modifico `project-stack.md` solo se conferma
> esplicita. Procedo solo dopo "ok" / "conferma" / "vai".

Eccezione: se il task è di **sola lettura, analisi, documentazione non distruttiva,
risposta a domanda**, puoi procedere subito senza conferma. Dichiaralo:
"Task non distruttivo: procedo direttamente."

---

## FASE 5 — DURANTE IL LAVORO (dopo conferma)

Mentre lavori:

- non reinventare parti già definite (consulta `decisions.md` se in dubbio);
- non modificare architettura/API/schema dati senza aggiornare la pagina corrispondente
  in `project-stack.md`;
- preserva lo stile del progetto (`company-standards.md` + `project-stack.md`);
- evita refactoring non richiesti;
- segnala codice fragile o duplicato senza modificarlo se non è il task;
- se prendi una decisione tecnica nuova, registrala in `decisions.md` (append-only);
- se emerge un dubbio bloccante, registralo in `open-questions.md`;
- se completi un task in `tasks-todo.md`, spostalo in DONE.

---

## FASE 6 — FINE SESSIONE

Quando il lavoro è concluso, **non aggiornare la memoria nel mezzo**: termina il task,
poi usa il **PROMPT-HANDOFF** per fare il refresh ordinato.

Se la sessione si chiude per esaurimento di tempo o di contesto, segnalalo all'utente:
"Sessione in pausa. Lancia `PROMPT-HANDOFF` per consolidare lo stato prima di chiudere."

---

## REGOLE CRITICHE

- **Mai iniziare senza aver letto AGENT.md e i file 2-7.**
- **Mai modificare `project-spec.md`.**
- **Mai aprire alla cieca tutta la `llm-wiki/`.**
- **Mai procedere su task strutturali senza conferma esplicita.**
- **Sempre dichiarare le incertezze** invece di colmarle con ipotesi.
- **Sempre rispettare** `company-standards.md` salvo deroghe esplicite in
  `project-stack.md`.
