# Quality Plan UniMove

**Progetto:** UniMove  
**Versione:** 1.0  
**Data:** Luglio 2026  
**Team:** Luca Lanese (Backend), Francesco Ferretti (Frontend)  
**Università:** Università degli Studi del Molise  
**Metodologia:** Agile/Scrum 2 Sprint  

---

## 1. Introduzione al Prodotto

**UniMove** è un'applicazione mobile di car sharing universitario destinata esclusivamente alla comunità accademica dell'Università degli Studi del Molise. Il sistema consente a studenti, docenti e dipendenti di organizzare e condividere spostamenti verso le sedi universitarie (es. Campobasso – Pesche), superando le rigidità e le inefficienze del trasporto pubblico locale.

### Contesto

Attualmente la mobilità verso le sedi UNIMOL dipende da trasporto pubblico rigido, navette universitarie con orari fissi e uso individuale di veicoli privati. Queste modalità non coprono le necessità di flessibilità della comunità accademica, generando costi elevati e impatto ambientale. UniMove risponde a questa esigenza reale con una piattaforma strutturata di mobilità condivisa.

### Architettura del Sistema

Il sistema è composto da due componenti principali:

| Componente | Tecnologia | Responsabilità |
|---|---|---|
| **Backend REST API** | Java 21, Spring Boot 4.0.6, PostgreSQL 15, Flyway | Logica di business, autenticazione, persistenza dati |
| **Applicazione Mobile** | Flutter (Dart SDK >=3.0.0), Riverpod, Dio, Go Router | Interfaccia utente, navigazione, gestione stato |

L'autenticazione è completamente delegata al sistema istituzionale **CINECA ESSE3**, che funge da Identity Provider e garantisce che solo utenti UNIMOL possano accedere al sistema. I token JWT (HMAC-SHA384, scadenza 24h) vengono conservati in modo sicuro tramite `flutter_secure_storage`.

---

## 2. Piani di Prodotto

### 2.1 Funzionalità per Sprint

**Sprint 1 Funzionalità Core (Giugno 2026):**
- Autenticazione istituzionale via CINECA ESSE3 con generazione token JWT stateless
- Gestione profilo utente: preferenze di viaggio, IBAN, tratte preferite (max 3), foto profilo da CINECA
- Creazione corsa con hotspot, dati veicolo e preferenze di viaggio
- Ricerca corse con filtri multipli (città, data, guidatore)

**Sprint 2 Ciclo di Vita Completo (Luglio 2026):**
- Prenotazione corsa con selezione hotspot e aggiornamento atomico dei posti disponibili
- Ciclo di vita corsa: OPEN → IN_PROGRESS → COMPLETED
- Eliminazione corsa con preavviso minimo 48h
- Cancellazione prenotazione con preavviso minimo 24h e abbandono corsa IN_PROGRESS
- Home e archivio: lista corse guidatore, prenotazioni passeggero, archivio corse terminate
- Recensioni post-corsa con valutazione 1-5 stelle e commento opzionale
- Refactoring eccezioni custom tipizzate (ResourceNotFoundException, UnauthorizedException, InvalidRequestException, AuthFailedException)
- Integrazione SonarCloud su entrambi i repository

**Funzionalità fuori scope (sprint successivi):**
- Chat privata guidatore-passeggero
- Tracciamento GPS in tempo reale
- Notifiche push
- Lista d'attesa automatizzata
- Donazioni volontarie
- Segnalazione utenti

### 2.2 Release

| Release | Sprint | Data | Contenuto principale |
|---|---|---|---|
| v0.1.0 | Sprint 1 | Giugno 2026 | Autenticazione, profilo, creazione e ricerca corse |
| v1.0.0 | Sprint 2 | Luglio 2026 | Prenotazioni, ciclo di vita, recensioni, qualità codice |

### 2.3 Controlli Periodici di Qualità

Ad ogni fine sprint viene effettuata una verifica completa della qualità:
- Esecuzione completa della suite di test unitari
- Analisi SonarCloud con Quality Gate su backend e frontend
- Code review tramite Pull Request su GitHub
- Aggiornamento della documentazione (RAD, SDD, ODD, Test Cases, Quality Plan)
- Chiusura dei task Jira come "Done" con verifica Definition of Done

---

## 3. Descrizione dei Processi

### 3.1 Processo di Sviluppo

Il team adotta la metodologia **Agile/Scrum** con sprint di 2-3 settimane. Le attività sono tracciate su **Jira** con gerarchia Epic → Task → Subtask. Ogni task viene sviluppato su un branch dedicato e integrato tramite Pull Request.

**Strategia di branching (Git Flow semplificato):**

```
main        ← codice stabile, aggiornato a fine sprint
  └── dev   ← integrazione continua del team
        └── feature/<nome>   ← sviluppo singola funzionalità
        └── fix/<nome>       ← correzione bug o issue
        └── docs/<nome>      ← aggiornamento documentazione
```

**Conventional Commits** tutti i commit seguono la convenzione:
```
feat(scope): aggiunta nuova funzionalità
fix(scope): correzione bug
docs(scope): aggiornamento documentazione
refactor(scope): refactoring senza cambiamento comportamento
test(scope): aggiunta o modifica test
ci(scope): modifica pipeline CI/CD
```

### 3.2 Processo di Testing

**Backend (JUnit 5 + Mockito + AssertJ):**

I test seguono il pattern AAA (Arrange, Act, Assert) e coprono tre livelli:
- **Caso normale** comportamento atteso con input validi
- **Caso limite** valori al confine (es. 24h prima della partenza, posti esauriti)
- **Caso eccezionale** input non validi, risorse non trovate, accessi non autorizzati

**Frontend (Flutter Test):**
- Test manuali end-to-end sull'applicazione tramite dispositivo Android
- Verifica dei flussi principali: login, creazione corsa, prenotazione, recensione

**Definition of Done** per ogni feature:

1. ✅ Implementazione completa: service, controller, DTO, mapper
2. ✅ Test unitari scritti e passati per tutti i casi d'uso
3. ✅ Test manuali Postman completati (normale, limite, eccezionale)
4. ✅ Pull Request aperta con descrizione delle modifiche
5. ✅ CI/CD verde (build, CodeQL, SonarCloud Quality Gate)
6. ✅ Documentazione aggiornata (SDD, ODD, Test Cases)
7. ✅ PR mergiata su `dev`

### 3.3 Processo di Code Review

Ogni Pull Request viene revisionata prima del merge. La review verifica:
- Correttezza funzionale e copertura dei casi limite
- Rispetto dei pattern architetturali (constructor injection, separazione layer, mapper separati)
- Assenza di duplicazione e code smell rilevati da SonarCloud
- Coerenza con le eccezioni custom tipizzate
- Superamento del Quality Gate SonarCloud (coverage ≥ 80%, duplication ≤ 3%)

### 3.4 Processo di Rilascio

Al termine di ogni sprint:
1. Merge di `dev` su `main` tramite PR
2. Verifica CI/CD automatica su `main`
3. Verifica Quality Gate SonarCloud sull'intera codebase
4. Aggiornamento documentazione e casi di test
5. Chiusura degli Epic e Task Jira come "Done"
6. Sincronizzazione dei branch locali con il remoto

### 3.5 Pipeline CI/CD

| Workflow | Trigger | Strumento | Verifica |
|---|---|---|---|
| Spring Boot CI | Push e PR su backend | GitHub Actions + Maven | Build e test unitari |
| CodeQL | Push e PR su backend | GitHub Actions | Analisi sicurezza statica |
| Dependency Review | PR su backend | GitHub Actions | Vulnerabilità nelle dipendenze |
| SonarCloud Backend | Push e PR su backend | SonarCloud | Qualità, coverage, duplicazione |
| SonarCloud Frontend | Push e PR su frontend | SonarCloud + Flutter | Qualità, duplicazione |

---

## 4. Obiettivi di Qualità

Gli attributi di qualità prioritari per UniMove sono definiti in coerenza con i requisiti non funzionali del RAD (sezione 3.3) e misurati tramite metriche concrete.

### 4.1 Affidabilità

**Obiettivo:** Il sistema deve garantire comportamento corretto e stabile durante l'utilizzo quotidiano, anche in condizioni di errore.

**Misure e soglie:**
- Disponibilità minima del 95% su base mensile
- Le operazioni critiche (prenotazione, cancellazione) sono atomiche tramite `@Transactional` rollback automatico in caso di errore
- Aggiornamento dei posti disponibili tramite query atomica `@Modifying` con lock a livello di riga in PostgreSQL prevenzione race condition
- Gestione centralizzata delle eccezioni tramite `GlobalExceptionHandler` con codici HTTP semanticamente corretti

**Stato attuale:**
- 75 test unitari sul backend, tutti passati
- Coverage backend: 89.1%
- 0 bug rilevati da SonarCloud (Reliability Rating: A)

### 4.2 Sicurezza

**Obiettivo:** Solo utenti UNIMOL autenticati possono accedere al sistema. I dati sensibili sono protetti in ogni strato.

**Misure e soglie:**
- Autenticazione delegata a CINECA ESSE3 nessuna password memorizzata nel sistema (tranne flusso register con BCrypt)
- Token JWT stateless (HMAC-SHA384, scadenza 24h) nessuna sessione lato server
- Token conservato in `flutter_secure_storage` sul dispositivo mobile
- UUID come chiavi primarie nessun identificatore sequenziale esposto nelle API
- CSRF disabilitato con sessioni stateless (`SecurityConfig`)
- Nessuna entità del database esposta direttamente tutti i dati transitano tramite DTO
- Security Rating: A su SonarCloud backend
- Conformità GDPR per dati personali, geolocalizzazione e IBAN

**Stato attuale:**
- 0 security hotspot sul backend
- 1 issue di sicurezza aperta sul frontend (catch vuoto in ApiClient) in risoluzione

### 4.3 Manutenibilità

**Obiettivo:** Il codice deve essere comprensibile, modificabile ed estendibile con sforzo minimo.

**Misure e soglie:**
- Architettura a livelli (Controller → Service → Repository) con separazione netta delle responsabilità
- Constructor injection ovunque nessun `@Autowired`
- Mapper come componenti separati (`RideMapper`, `BookingMapper`, `ReviewMapper`)
- Eccezioni custom tipizzate al posto di `RuntimeException` generiche
- Metodi privati di supporto per eliminare duplicazione (`getUser`, `getVerifiedRide` in `RideService`)
- Duplication ≤ 3% (Quality Gate) attuale: 0.0% backend
- Maintainability Rating ≥ A (Quality Gate) attuale: A backend e frontend
- Conventional Commits per tracciabilità completa delle modifiche

**Stato attuale:**
- 0 duplicazioni sul backend
- 30 issue di maintainability sul frontend (minor)

### 4.4 Usabilità

**Obiettivo:** Il sistema deve essere accessibile e intuitivo per tutta la comunità accademica UNIMOL, indipendentemente dal livello di familiarità tecnologica.

**Misure e soglie:**
- Un nuovo utente completa il primo accesso e configura le preferenze entro 3 minuti
- Navigazione raggiungibile con massimo 3 tap dalla home
- Messaggi di errore in lingua italiana, chiari e privi di dettagli tecnici
- Interfaccia responsive adattata alle dimensioni del dispositivo mobile (Material Design)
- Compatibilità cross-platform Android e iOS tramite Flutter

**Verifica:** Test manuali end-to-end sull'applicazione Flutter con dispositivo Android.

### 4.5 Performance

**Obiettivo:** Il sistema deve rispondere in tempi accettabili anche nelle fasce orarie di punta (mattina e pomeriggio).

**Misure e soglie:**
- API backend: risposta entro 2 secondi per il 95% delle richieste
- Supporto minimo di 100 utenti concorrenti senza degradazione
- Caricamento home dopo login: massimo 3 secondi in condizioni di buona rete
- Ricerca corse: risultati entro 2 secondi dalla sottomissione della query

**Verifica:** Test manuali Postman con misurazione dei tempi di risposta.

### 4.6 Copertura del Codice

**Obiettivo:** Garantire che la logica di business sia adeguatamente coperta da test automatici.

**Misure e soglie:**

| Componente | Coverage Attuale | Soglia Quality Gate |
|---|---|---|
| Backend (service layer) | 89.1% | ≥ 80% |
| Frontend (Flutter) | 0.0% | Non configurata (pianificata) |

**Classi escluse dalla copertura backend** (per scelta architetturale motivata):
- Controller orchestrazione pura, zero logica di business
- Configurazioni (`SecurityConfig`, `RestClientConfig`) setup Spring
- Entità JPA e DTO classi dati generate da Lombok
- Eccezioni custom solo costruttore

**Compromesso accettato:** L'esclusione è documentata e motivata. La logica di business risiede interamente nei service, che sono coperti al 100%.

---

## 5. Rischi e Gestione

| # | Rischio | Probabilità | Impatto | Strategia di Mitigazione |
|---|---|---|---|---|
| R1 | Irraggiungibilità del servizio CINECA ESSE3 | Media | Alto | Il sistema restituisce un messaggio di errore chiaro in italiano senza esporre dettagli tecnici. Gli utenti con token JWT ancora valido non sono impattati. |
| R2 | Race condition sulle prenotazioni | Bassa | Alto | Decremento atomico dei posti tramite query `@Modifying` con `AND available_seats > 0` direttamente in PostgreSQL. Lock a livello di riga gestito dal DB. |
| R3 | Regressioni durante il refactoring | Media | Medio | Suite di 75 test unitari eseguita automaticamente ad ogni PR tramite CI/CD. Nessun merge senza CI verde e Quality Gate passato. |
| R4 | Deriva della documentazione rispetto al codice | Media | Medio | Aggiornamento documentazione incluso nella Definition of Done di ogni feature. Documentazione versionata nello stesso repository GitHub. |
| R5 | Vulnerabilità nelle dipendenze | Bassa | Alto | Dependency Review automatica su ogni PR tramite GitHub Actions. Aggiornamento regolare delle dipendenze tramite Maven e pubspec. |
| R6 | Perdita di dati durante operazioni critiche | Bassa | Alto | `@Transactional` su tutti i metodi write nel backend rollback automatico in caso di eccezione non gestita. |
| R7 | Assenza di test automatici sul frontend | Alta | Medio | Limitazione nota dovuta ai tempi di sviluppo. Mitigata da test manuali end-to-end sistematici su dispositivo Android. Pianificata per sprint successivi. |
| R8 | Scadenza del token JWT durante una sessione | Bassa | Basso | Token con scadenza 24h sufficiente per una sessione di utilizzo quotidiana. Il client Flutter gestisce il caso 401 con logout automatico e redirect al login. |
| R9 | Security issue nel client Flutter (catch vuoto) | Alta | Medio | Issue identificata da SonarCloud. La gestione silenziosa delle eccezioni può nascondere errori di rete. In risoluzione con aggiunta di logging tramite `debugPrint`. |
| R10 | Duplicazione codice nel frontend (10.7%) | Alta | Basso | Rilevata da SonarCloud. Non blocca il Quality Gate attuale. Pianificata refactoring dei widget comuni per sprint successivi. |