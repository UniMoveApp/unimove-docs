# Documento Casi di Test 

**Progetto:** UniMove  
**Sprint:** 1  
**Autore:** Luca Lanese  
**Data:** Giugno 2026  
**Strumenti:** JUnit 5, Mockito, AssertJ  
**Totale test:** 39  tutti passati 

---

## 1. AuthService (9 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Login utente nuovo | Credenziali valide, utente non presente nel DB | Utente creato, token JWT restituito | Utente creato correttamente, token restituito |  PASS |
| 2 | Login utente esistente | Credenziali valide, utente già presente nel DB | Dati utente aggiornati (non duplicati), token restituito | Dati aggiornati correttamente |  PASS |
| 3 | Login credenziali errate | Username/password non validi su CINECA | Eccezione "Credenziali non valide", nessun salvataggio | Eccezione lanciata correttamente |  PASS |
| 4 | Risposta CINECA malformata | CINECA risponde senza dati utente | Eccezione "Risposta CINECA non valida" | Eccezione lanciata correttamente |  PASS |
| 5 | Mapping ruolo Docenti | `grpDes = "Docenti"` | Ruolo mappato a `PROFESSOR` | Mappato correttamente |  PASS |
| 6 | Mapping ruolo PTA | `grpDes = "PTA"` | Ruolo mappato a `STAFF` | Mappato correttamente |  PASS |
| 7 | Mapping ruolo sconosciuto | `grpDes = "RuoloInesistente"` | Ruolo di default `STUDENT` | Default applicato correttamente |  PASS |
| 8 | Register username esistente | Username già presente nel DB | Eccezione "Username già esistente" | Eccezione lanciata correttamente |  PASS |
| 9 | Register username nuovo | Username non presente nel DB | Utente creato con ruolo STUDENT, password BCrypt, token restituito | Utente creato correttamente |  PASS |

---

## 2. JwtFilter (5 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Nessun header Authorization | Richiesta senza header `Authorization` | Nessuna autenticazione, catena di filtri prosegue | Comportamento confermato |  PASS |
| 2 | Header senza prefisso Bearer | Header `Authorization: Basic ...` | Nessuna autenticazione | Comportamento confermato |  PASS |
| 3 | Token non valido | Token JWT non valido/scaduto | Nessuna autenticazione | Comportamento confermato |  PASS |
| 4 | Token valido | Token JWT valido | SecurityContext popolato con principal corretto e authority `ROLE_USER` | Popolamento corretto confermato |  PASS |
| 5 | Continuità della catena filtri | Qualsiasi richiesta (con o senza token valido) | La catena di filtri prosegue sempre | Comportamento confermato |  PASS |

---

## 3. UserService (14 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Profilo con corse imminenti | Username esistente, corse future presenti | Profilo restituito con lista corse popolata | Lista popolata correttamente |  PASS |
| 2 | Profilo senza corse imminenti | Username esistente, nessuna corsa futura | Profilo restituito con lista corse vuota | Lista vuota correttamente |  PASS |
| 3 | Profilo utente non trovato | Username inesistente | Eccezione "Utente non trovato" | Eccezione lanciata correttamente |  PASS |
| 4 | Aggiornamento preferenze riuscito | Username esistente, nuove preferenze | Preferenze aggiornate e salvate | Aggiornamento confermato |  PASS |
| 5 | Aggiornamento preferenze, utente non trovato | Username inesistente | Eccezione "Utente non trovato" | Eccezione lanciata correttamente |  PASS |
| 6 | Aggiornamento IBAN riuscito | Username esistente, nuovo IBAN/intestatario | IBAN e intestatario aggiornati e salvati | Aggiornamento confermato |  PASS |
| 7 | Aggiornamento IBAN, utente non trovato | Username inesistente | Eccezione "Utente non trovato" | Eccezione lanciata correttamente |  PASS |
| 8 | Lista tratte popolata | Username esistente con tratte salvate | Lista tratte restituita correttamente | Lista popolata correttamente |  PASS |
| 9 | Lista tratte vuota | Username esistente senza tratte | Lista vuota restituita | Lista vuota correttamente |  PASS |
| 10 | Aggiunta tratta sotto il limite | Username esistente, meno di 3 tratte salvate | Nuova tratta creata e restituita | Creazione confermata |  PASS |
| 11 | Aggiunta tratta, limite raggiunto | Username esistente, già 3 tratte salvate | Eccezione "Massimo 3 tratte preferite consentite" | Eccezione lanciata correttamente |  PASS |
| 12 | Eliminazione tratta valida e autorizzata | Tratta esistente, appartenente all'utente | Tratta eliminata con successo | Eliminazione confermata |  PASS |
| 13 | Eliminazione tratta non trovata | ID tratta inesistente | Eccezione "Tratta preferita non trovata" | Eccezione lanciata correttamente |  PASS |
| 14 | Eliminazione tratta di altro utente | Tratta esistente, appartenente ad un altro utente | Eccezione "Non sei autorizzato a eliminare questa tratta preferita" | Eccezione lanciata correttamente |  PASS |

---

## 4. RideMapper (4 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Mapping con tutti gli hotspot | Corsa con 3 hotspot impostati | Lista hotspot con tutti e 3 gli elementi, dati guidatore e corsa mappati correttamente | Mapping corretto confermato |  PASS |
| 2 | Mapping con hotspot parziali | Corsa con solo 1 hotspot impostato | Lista hotspot con un solo elemento | Mapping corretto confermato |  PASS |
| 3 | Mapping senza hotspot | Corsa senza alcun hotspot impostato | Lista hotspot vuota | Lista vuota confermata |  PASS |
| 4 | Mapping posti totali/disponibili | Corsa con posti totali e disponibili diversi (es. 4 totali, 2 disponibili) | Entrambi i valori mappati correttamente e distintamente | Mapping corretto confermato |  PASS |

---

## 5. RideService (7 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Creazione corsa con tutti i campi | Dati completi (città, orari, hotspot, veicolo, posti, preferenze) | Corsa creata e salvata correttamente, posti disponibili = posti totali | Creazione confermata |  PASS |
| 2 | Creazione corsa senza campi opzionali | Solo campi obbligatori (città, orario, posti) | Corsa creata con campi opzionali a `null` | Creazione confermata |  PASS |
| 3 | Creazione corsa, utente non trovato | Username inesistente | Eccezione "Utente non trovato", nessun salvataggio | Eccezione lanciata correttamente |  PASS |
| 4 | Ricerca senza filtri | Tutti i parametri `null` | Restituisce tutte le corse disponibili | Risultati corretti |  PASS |
| 5 | Ricerca con filtro singolo | Solo città di partenza specificata | Restituisce le corse corrispondenti al filtro | Risultati corretti |  PASS |
| 6 | Ricerca con filtri combinati | Username guidatore, città partenza/arrivo, data tutti specificati | Tutti i parametri passati correttamente alla query | Parametri passati correttamente |  PASS |
| 7 | Ricerca senza risultati | Filtri che non corrispondono a nessuna corsa | Restituisce lista vuota | Lista vuota confermata |  PASS |

---

## Riepilogo

| Componente | Test | Esito |
|---|---|---|
| AuthService | 9 |  Tutti passati |
| JwtFilter | 5 |  Tutti passati |
| UserService | 14 |  Tutti passati |
| RideMapper | 4 |  Tutti passati |
| RideService | 7 |  Tutti passati |
| **Totale** | **39** | ** 39/39 passati** |