# Sistema di Memoria Persistente Multi-Modello — Guida Completa

## Problema che questo sistema risolve

Ogni volta che apri una nuova conversazione con un modello AI (Claude, GPT, Gemini, modelli locali), il modello riparte da zero: non sa nulla del progetto, delle decisioni prese, dello stato del codice, di cosa è già stato fatto. Devi rispiegare tutto ogni volta.

Questo sistema risolve il problema creando una **memoria esterna persistente** nella cartella `.ai/`, scritta e letta dai modelli stessi tramite tre prompt specializzati. Il risultato: puoi cambiare modello, aprire una nuova sessione, o riprendere dopo giorni — e il modello sa esattamente dove eravate rimasti.

---

## I 3 Prompt e il loro ruolo nel ciclo di lavoro

```
[INIZIO PROGETTO o RESET]
        │
        ▼
┌─────────────────────────────────┐
│  PROMPT UNIVERSALE DI BOOTSTRAP │  ← crea / aggiorna .ai/
└─────────────────────────────────┘
        │
        ▼
[INIZIO DI OGNI SESSIONE DI LAVORO]
        │
        ▼
┌─────────────────┐
│ PROMPT DI AVVIO │  ← carica il contesto, propone il piano
└─────────────────┘
        │
        ▼
[...lavoro effettivo...]
        │
        ▼
[FINE SESSIONE / CAMBIO MODELLO]
        │
        ▼
┌──────────────────────┐
│ PROMPT DI HANDOFF    │  ← aggiorna .ai/ per il prossimo
└──────────────────────┘
        │
        └──────────────────────────────► torna a PROMPT DI AVVIO
```

I tre prompt formano un **ciclo chiuso**: Bootstrap → Avvio → Lavoro → Handoff → Avvio → ...

---

## Prompt 1: PROMPT UNIVERSALE DI BOOTSTRAP

**File:** `PROMPT UNIVERSALE DI BOOTSTRAP.md`  
**Quando usarlo:** All'inizio di un progetto nuovo, oppure quando si vuole riallineare la memoria `.ai/` con lo stato reale del codice.  
**Chi lo riceve:** Qualsiasi modello AI capace di leggere e scrivere file.

### Cosa fa

Analizza l'intera cartella del progetto e crea (o aggiorna) la cartella `.ai/` con 7 file di memoria strutturata.

Opera in due modalità:

| Modalità | Condizione | Comportamento |
|----------|------------|---------------|
| **INIT** | `.ai/` non esiste | Crea la cartella e tutti i 7 file da zero |
| **UPDATE** | `.ai/` esiste già | Legge i file esistenti, verifica la coerenza con il codice, aggiorna solo ciò che è cambiato |

### Comportamento critico

Il prompt impone un **passaggio di allineamento obbligatorio** prima di scrivere qualsiasi file:

1. Il modello riassume in max 10 righe cosa ha capito del progetto
2. Dichiara se ha compreso davvero l'architettura oppure no
3. **Attende conferma prima di procedere**

Questo impedisce che il modello inventi architetture o funzionalità non esistenti.

Il file `architecture.md` ha una regola speciale: **viene scritto solo se l'architettura è realmente comprensibile dal codice**. Se il modello non la capisce, deve dichiararlo esplicitamente e chiedere chiarimenti — non inventare.

---

## Prompt 2: PROMPT DI AVVIO

**File:** `PROMPT DI AVVIO.md`  
**Quando usarlo:** All'inizio di ogni nuova sessione di lavoro, prima di dare qualsiasi task al modello.  
**Chi lo riceve:** Il modello che deve lavorare sul progetto in quella sessione.

### Cosa fa

Obbliga il modello a leggere tutti i file `.ai/` prima di fare qualsiasi cosa, poi struttura la risposta in fasi precise:

1. **Max 3 domande di chiarimento** se necessario
2. **3 regole più importanti** dedotte dai file `.ai/`
3. **Riassunto del task attuale** (letto da `tasks_todo.md` e `session_handoff.md`)
4. **Valutazione dell'architettura**: è sufficiente per procedere o serve chiarimento?
5. **Piano in massimo 5 step** — se il task tocca l'architettura, include uno step esplicito per aggiornare `architecture.md`
6. **Attende conferma** prima di eseguire qualsiasi cosa

### Perché questo ordine è importante

Il prompt impone un rallentamento deliberato. Il modello non può iniziare a scrivere codice al primo messaggio: deve prima dimostrare di aver capito il contesto, le decisioni prese, lo stato attuale. Solo dopo la tua conferma parte.

Questo riduce drasticamente le regressions e le violazioni di decisioni già prese.

---

## Prompt 3: PROMPT DI HANDOFF

**File:** `PROMPT DI HANDOFF.md`  
**Quando usarlo:** Alla fine di ogni sessione, prima di chiudere la conversazione o passare a un altro modello.  
**Chi lo riceve:** Il modello che ha appena lavorato sul progetto.

### Cosa fa

Aggiorna i file `.ai/` per fotografare lo stato esatto del progetto alla fine della sessione, in modo che il prossimo modello (o la prossima sessione) possa riprendere senza perdita di contesto.

Aggiorna 5 file:

| File | Cosa viene scritto |
|------|--------------------|
| `current_state.md` | Stato aggiornato, ultimi file modificati, problemi aperti, impatto architetturale |
| `session_handoff.md` | Cosa è stato fatto, stato preciso, prossimi step numerati, cosa NON rifare |
| `decisions.md` | Nuove decisioni tecniche o architetturali emerse nella sessione |
| `tasks_todo.md` | Task aggiornati (TODO/DOING/DONE) |
| `architecture.md` | Solo se il lavoro ha modificato o chiarito realmente struttura, flussi o dipendenze |

### Regola fondamentale

Il testo viene scritto **per un altro modello, non per un umano**. Deve essere operativo, preciso, privo di riassunti vaghi. Il test di qualità è: *un modello senza cronologia chat può continuare solo leggendo `.ai/`?*

Il prompt include una checklist finale obbligatoria:
- Il quadro architetturale è comprensibile?
- Lo stato attuale è coerente con l'architettura?
- I prossimi step dipendono da vincoli strutturali?
- C'è qualcosa che il prossimo modello rischierebbe di rompere?

---

## I 7 File della Cartella `.ai/`

La cartella `.ai/` è il cuore del sistema. Ogni file ha uno scopo preciso e viene letto/scritto in momenti diversi del ciclo.

### `.ai/project_brief.md`

**Scopo:** Descrizione generale e stabile del progetto.  
**Letto da:** Tutti i modelli all'avvio.  
**Aggiornato:** Raramente (solo se cambia lo stack, la struttura principale, o gli obiettivi).

Contiene:
- Descrizione del progetto e obiettivo
- Stack tecnologico
- Struttura delle cartelle
- Componenti principali
- Sezione opzionale `## NOTE TECNICHE` per annotazioni manuali specifiche

Questo è il file più stabile: una volta scritto bene, cambia poco.

---

### `.ai/architecture.md`

**Scopo:** Mappa completa dell'architettura del sistema.  
**Letto da:** Obbligatoriamente prima di qualsiasi modifica strutturale.  
**Aggiornato:** Solo quando l'architettura cambia realmente.

È il file più critico del sistema. Contiene:
- Panoramica architetturale
- Sottosistemi e moduli principali con le loro responsabilità
- Flussi principali del programma
- Flussi dei dati
- Punti di ingresso
- Dipendenze interne ed esterne
- Integrazioni, API, servizi esterni
- Convenzioni architetturali rilevanti
- Colli di bottiglia o aree sensibili

**Regola d'oro:** Questo file non si scrive per completezza formale, ma solo quando il modello ha davvero capito l'architettura. Un `architecture.md` sbagliato è peggio di uno assente — guida male i modelli successivi. Se non è chiaro, si dichiara l'incertezza invece di inventare.

---

### `.ai/current_state.md`

**Scopo:** Fotografia dello stato attuale del progetto.  
**Letto da:** Ogni modello all'avvio, per capire da dove riprendere.  
**Aggiornato:** A ogni handoff.

Contiene:
- Cosa funziona
- Cosa non è completo
- Ultime aree di lavoro (dedotte o segnalate)
- Come lo stato si collega all'architettura

È il file che cambia più frequentemente. Deve rispondere alla domanda: *se entrassi in questo progetto adesso, da dove partiresti?*

---

### `.ai/decisions.md`

**Scopo:** Registro delle decisioni tecniche e architetturali prese.  
**Letto da:** Ogni modello all'avvio, per non reinventare decisioni già prese.  
**Aggiornato:** Solo quando emergono nuove decisioni verificabili.

Contiene decisioni come:
- Scelta di librerie o framework specifici (con motivazione)
- Pattern architetturali adottati
- Approcci scartati e perché
- Vincoli tecnici imposti

**Regola critica:** Non si inventano decisioni. Si aggiunge solo ciò che è realmente emerso e verificabile nella sessione. Un `decisions.md` gonfiato con decisioni inventate è dannoso quanto un `architecture.md` falso.

---

### `.ai/session_handoff.md`

**Scopo:** Passaggio di consegne tra sessioni o modelli.  
**Letto da:** Il modello all'inizio di ogni nuova sessione.  
**Aggiornato:** A ogni handoff, sostituendo la versione precedente.

Contiene:
- Cosa è stato fatto nella sessione appena conclusa
- Stato attuale preciso
- Prossimi step numerati e in ordine
- Cosa NON rifare (errori o strade già percorse)
- Stato di `architecture.md` (aggiornato / invariato / da rivedere)

Questo è il file che simula la "memoria a breve termine" del progetto. Risponde alla domanda: *cosa sta succedendo proprio adesso?*

---

### `.ai/tasks_todo.md`

**Scopo:** Backlog e stato dei task di sviluppo.  
**Letto da:** Ogni modello per capire le priorità.  
**Aggiornato:** A ogni handoff e durante le sessioni.

Formato a tre colonne:
```
## TODO
- [ ] Task da fare

## DOING
- [ ] Task in corso

## DONE
- [x] Task completato
```

Quando utile, indica se un task impatta l'architettura o richiede prima l'aggiornamento di `architecture.md`.

---

### `.ai/model_prefs.md`

**Scopo:** Definisce quale modello usare per quale tipo di lavoro.  
**Letto da:** Chi coordina il lavoro tra più modelli.  
**Aggiornato:** Raramente, solo se cambiano le preferenze.

Schema tipico (personalizzabile):
- **Claude** → architettura, reasoning, decisioni strutturali
- **GPT** → codice, refactoring, debugging
- **Modello locale** → debug rapido, test, operazioni ripetitive

Contiene anche la regola che tutti i modelli devono leggere `architecture.md` prima di fare modifiche strutturali.

---

## Flusso di utilizzo pratico

### Primo utilizzo su un progetto esistente

```
1. Apri una nuova chat con il modello preferito
2. Incolla il contenuto di "PROMPT UNIVERSALE DI BOOTSTRAP.md"
3. Aggiungi: "Il progetto è in [path/cartella]"
4. Leggi il riassunto che propone il modello
5. Conferma (o correggi) → il modello crea la cartella .ai/
6. Verifica i file generati, correggili manualmente se necessario
```

### Ogni sessione di lavoro

```
1. Apri una nuova chat
2. Incolla il contenuto di "PROMPT DI AVVIO.md"
3. Aggiungi: "Il task di oggi è: [descrizione]"
4. Leggi il piano proposto dal modello
5. Conferma → il modello inizia a lavorare
6. A fine sessione, incolla "PROMPT DI HANDOFF.md"
7. Il modello aggiorna i file .ai/
8. Chiudi la chat
```

### Cambio di modello a metà progetto

```
1. Nella sessione corrente, incolla "PROMPT DI HANDOFF.md"
2. Apri nuova chat con il nuovo modello
3. Incolla "PROMPT DI AVVIO.md"
4. Il nuovo modello legge .ai/ e riprende da dove si era fermato
```

---

## Perché funziona: i principi dietro il sistema

**1. La memoria è esterna al modello, non interna alla chat.**
I file `.ai/` esistono sul filesystem. Non dipendono dalla cronologia della conversazione. Qualsiasi modello su qualsiasi piattaforma può leggerli.

**2. La scrittura è controllata, non automatica.**
I prompt non danno carta bianca: ogni scrittura passa da un momento di allineamento in cui il modello dichiara cosa ha capito e attende conferma. Questo previene la "allucinazione di architetture".

**3. `architecture.md` è trattato come documento critico.**
Non si scrive per completare un template. Si scrive solo quando davvero compreso. L'incertezza dichiarata è preferibile alla certezza falsa.

**4. Il testo è scritto per macchine, non per umani.**
I file `.ai/` sono strumenti operativi per i modelli, non documentazione per sviluppatori. Il linguaggio deve essere preciso e privo di ambiguità narrative.

**5. Il ciclo è chiuso.**
Ogni sessione produce un handoff che alimenta la sessione successiva. Il sistema migliora progressivamente: più sessioni passano, più i file `.ai/` diventano accurati e utili.

---

## Errori comuni da evitare

| Errore | Conseguenza | Soluzione |
|--------|-------------|-----------|
| Usare AVVIO senza aver fatto prima il BOOTSTRAP | Il modello non trova `.ai/` e riparte da zero | Fare sempre BOOTSTRAP prima sulla cartella |
| Non fare HANDOFF a fine sessione | La sessione successiva non sa cosa è stato fatto | Fare sempre HANDOFF prima di chiudere |
| Lasciare `architecture.md` vuoto per paura di sbagliare | I modelli non sanno come è strutturato il sistema | Meglio un `architecture.md` parziale e dichiaratamente incerto che assente |
| Correggere i file `.ai/` a mano senza logica | I file diventano incoerenti tra loro | Se si modifica manualmente, aggiornare anche i file correlati |
| Usare il sistema solo con un modello | Si perde il vantaggio principale | Testarlo attivamente con Claude, GPT e modelli locali in sequenza |

---

## Struttura finale attesa su un progetto attivo

```
progetto/
├── .ai/
│   ├── project_brief.md      ← descrizione stabile
│   ├── architecture.md       ← mappa critica del sistema
│   ├── current_state.md      ← fotografia aggiornata
│   ├── decisions.md          ← decisioni prese
│   ├── session_handoff.md    ← ultimo passaggio di consegne
│   ├── tasks_todo.md         ← backlog attivo
│   └── model_prefs.md        ← preferenze modelli
├── [codice sorgente...]
├── PROMPT UNIVERSALE DI BOOTSTRAP.md
├── PROMPT DI AVVIO.md
└── PROMPT DI HANDOFF.md
```

I tre prompt `.md` rimangono nella root del progetto. Vengono copiati-incollati nelle chat quando necessario. I file `.ai/` sono scritti e aggiornati dai modelli. La cartella `.ai/` andrebbe aggiunta al repository (non al `.gitignore`) perché è parte integrante della documentazione operativa del progetto.
