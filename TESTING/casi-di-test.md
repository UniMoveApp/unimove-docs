# Documento Casi di Test

**Progetto:** UniMove
**Autore:** Luca Lanese
**Data:** Luglio 2026
**Strumenti:** JUnit 5, Mockito, AssertJ
**Totale test:** 75 tutti passati

---

## 1. AuthService (9 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Login utente nuovo | Credenziali valide, utente non presente nel DB | Utente creato, token JWT restituito | Utente creato correttamente, token restituito | ✅ PASS |
| 2 | Login utente esistente | Credenziali valide, utente già presente nel DB | Dati utente aggiornati (non duplicati), token restituito | Dati aggiornati correttamente | ✅ PASS |
| 3 | Login credenziali errate | Username/password non validi su CINECA | Eccezione "Credenziali non valide", nessun salvataggio | Eccezione lanciata correttamente | ✅ PASS |
| 4 | Risposta CINECA malformata | CINECA risponde senza dati utente | Eccezione "Risposta CINECA non valida" | Eccezione lanciata correttamente | ✅ PASS |
| 5 | Mapping ruolo Docenti | `grpDes = "Docenti"` | Ruolo mappato a `PROFESSOR` | Mappato correttamente | ✅ PASS |
| 6 | Mapping ruolo PTA | `grpDes = "PTA"` | Ruolo mappato a `STAFF` | Mappato correttamente | ✅ PASS |
| 7 | Mapping ruolo sconosciuto | `grpDes = "RuoloInesistente"` | Ruolo di default `STUDENT` | Default applicato correttamente | ✅ PASS |
| 8 | Register username esistente | Username già presente nel DB | Eccezione "Username già esistente" | Eccezione lanciata correttamente | ✅ PASS |
| 9 | Register username nuovo | Username non presente nel DB | Utente creato con ruolo STUDENT, password BCrypt, token restituito | Utente creato correttamente | ✅ PASS |

---

## 2. JwtFilter (5 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Nessun header Authorization | Richiesta senza header `Authorization` | Nessuna autenticazione, catena di filtri prosegue | Comportamento confermato | ✅ PASS |
| 2 | Header senza prefisso Bearer | Header `Authorization: Basic ...` | Nessuna autenticazione | Comportamento confermato | ✅ PASS |
| 3 | Token non valido | Token JWT non valido/scaduto | Nessuna autenticazione | Comportamento confermato | ✅ PASS |
| 4 | Token valido | Token JWT valido | SecurityContext popolato con principal corretto e authority `ROLE_USER` | Popolamento corretto confermato | ✅ PASS |
| 5 | Continuità della catena filtri | Qualsiasi richiesta (con o senza token valido) | La catena di filtri prosegue sempre | Comportamento confermato | ✅ PASS |

---

## 3. UserService (14 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Profilo con corse imminenti | Username esistente, corse future presenti | Profilo restituito con lista corse popolata | Lista popolata correttamente | ✅ PASS |
| 2 | Profilo senza corse imminenti | Username esistente, nessuna corsa futura | Profilo restituito con lista corse vuota | Lista vuota correttamente | ✅ PASS |
| 3 | Profilo utente non trovato | Username inesistente | Eccezione "Utente non trovato" | Eccezione lanciata correttamente | ✅ PASS |
| 4 | Aggiornamento preferenze riuscito | Username esistente, nuove preferenze | Preferenze aggiornate e salvate | Aggiornamento confermato | ✅ PASS |
| 5 | Aggiornamento preferenze, utente non trovato | Username inesistente | Eccezione "Utente non trovato" | Eccezione lanciata correttamente | ✅ PASS |
| 6 | Aggiornamento IBAN riuscito | Username esistente, nuovo IBAN/intestatario | IBAN e intestatario aggiornati e salvati | Aggiornamento confermato | ✅ PASS |
| 7 | Aggiornamento IBAN, utente non trovato | Username inesistente | Eccezione "Utente non trovato" | Eccezione lanciata correttamente | ✅ PASS |
| 8 | Lista tratte popolata | Username esistente con tratte salvate | Lista tratte restituita correttamente | Lista popolata correttamente | ✅ PASS |
| 9 | Lista tratte vuota | Username esistente senza tratte | Lista vuota restituita | Lista vuota correttamente | ✅ PASS |
| 10 | Aggiunta tratta sotto il limite | Username esistente, meno di 3 tratte salvate | Nuova tratta creata e restituita | Creazione confermata | ✅ PASS |
| 11 | Aggiunta tratta, limite raggiunto | Username esistente, già 3 tratte salvate | Eccezione "Massimo 3 tratte preferite consentite" | Eccezione lanciata correttamente | ✅ PASS |
| 12 | Eliminazione tratta valida e autorizzata | Tratta esistente, appartenente all'utente | Tratta eliminata con successo | Eliminazione confermata | ✅ PASS |
| 13 | Eliminazione tratta non trovata | ID tratta inesistente | Eccezione "Tratta preferita non trovata" | Eccezione lanciata correttamente | ✅ PASS |
| 14 | Eliminazione tratta di altro utente | Tratta esistente, appartenente ad un altro utente | Eccezione "Non sei autorizzato a eliminare questa tratta preferita" | Eccezione lanciata correttamente | ✅ PASS |

---

## 4. RideMapper (4 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Mapping con tutti gli hotspot | Corsa con 3 hotspot impostati | Lista hotspot con tutti e 3 gli elementi, dati guidatore e corsa mappati correttamente | Mapping corretto confermato | ✅ PASS |
| 2 | Mapping con hotspot parziali | Corsa con solo 1 hotspot impostato | Lista hotspot con un solo elemento | Mapping corretto confermato | ✅ PASS |
| 3 | Mapping senza hotspot | Corsa senza alcun hotspot impostato | Lista hotspot vuota | Lista vuota confermata | ✅ PASS |
| 4 | Mapping posti totali/disponibili | Corsa con posti totali e disponibili diversi (es. 4 totali, 2 disponibili) | Entrambi i valori mappati correttamente e distintamente | Mapping corretto confermato | ✅ PASS |

---

## 5. RideService Sprint 1 (7 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Creazione corsa con tutti i campi | Dati completi (città, orari, hotspot, veicolo, posti, preferenze) | Corsa creata e salvata correttamente, posti disponibili = posti totali | Creazione confermata | ✅ PASS |
| 2 | Creazione corsa senza campi opzionali | Solo campi obbligatori (città, orario, posti) | Corsa creata con campi opzionali a `null` | Creazione confermata | ✅ PASS |
| 3 | Creazione corsa, utente non trovato | Username inesistente | Eccezione "Utente non trovato", nessun salvataggio | Eccezione lanciata correttamente | ✅ PASS |
| 4 | Ricerca senza filtri | Tutti i parametri `null` | Restituisce tutte le corse disponibili | Risultati corretti | ✅ PASS |
| 5 | Ricerca con filtro singolo | Solo città di partenza specificata | Restituisce le corse corrispondenti al filtro | Risultati corretti | ✅ PASS |
| 6 | Ricerca con filtri combinati | Username guidatore, città partenza/arrivo, data tutti specificati | Tutti i parametri passati correttamente alla query | Parametri passati correttamente | ✅ PASS |
| 7 | Ricerca senza risultati | Filtri che non corrispondono a nessuna corsa | Restituisce lista vuota | Lista vuota confermata | ✅ PASS |

---

## 6. RideService Sprint 2 (13 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Avvio corsa in stato OPEN | Corsa con stato `OPEN`, utente è il guidatore | Stato aggiornato a `IN_PROGRESS`, corsa salvata | Stato aggiornato correttamente | ✅ PASS |
| 2 | Avvio corsa non in stato OPEN | Corsa con stato `CLOSED` | Eccezione "Corsa non ancora disponibile per partenza", nessun salvataggio | Eccezione lanciata correttamente | ✅ PASS |
| 3 | Avvio corsa da non guidatore | Utente diverso dal guidatore | Eccezione "Non sei il guidatore di questa corsa" | Eccezione lanciata correttamente | ✅ PASS |
| 4 | Completamento corsa in stato IN_PROGRESS | Corsa con stato `IN_PROGRESS`, utente è il guidatore | Stato aggiornato a `COMPLETED`, corsa salvata | Stato aggiornato correttamente | ✅ PASS |
| 5 | Completamento corsa non in stato IN_PROGRESS | Corsa con stato `OPEN` | Eccezione "Corsa non ancora in corso", nessun salvataggio | Eccezione lanciata correttamente | ✅ PASS |
| 6 | Cancellazione corsa con preavviso sufficiente | Corsa con stato `OPEN`, partenza tra 5 giorni | Corsa eliminata dal database | Eliminazione confermata | ✅ PASS |
| 7 | Cancellazione corsa senza preavviso sufficiente | Corsa con stato `OPEN`, partenza tra 10 ore | Eccezione "Non puoi cancellare una corsa con meno di 48 ore dalla partenza" | Eccezione lanciata correttamente | ✅ PASS |
| 8 | Cancellazione corsa non in stato OPEN | Corsa con stato `IN_PROGRESS` | Eccezione "Corsa non ancora disponibile per cancellazione" | Eccezione lanciata correttamente | ✅ PASS |
| 9 | Lista corse guidatore con filtro stato | Username esistente, filtro `OPEN` | Lista corse filtrate per stato restituita | Lista corretta | ✅ PASS |
| 10 | Lista corse guidatore senza filtro | Username esistente, `status` null | Tutte le corse del guidatore restituite | Lista completa | ✅ PASS |
| 11 | Archivio con corse come guidatore | Corse completate come guidatore presenti | Lista corse completate come guidatore | Lista popolata correttamente | ✅ PASS |
| 12 | Archivio con corse come passeggero | Corse completate come passeggero presenti | Lista corse completate come passeggero | Lista popolata correttamente | ✅ PASS |
| 13 | Archivio con duplicati | Stessa corsa presente come guidatore e passeggero | Lista senza duplicati (1 elemento) | Deduplicazione corretta | ✅ PASS |

---

## 7. BookingService (14 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Prenotazione valida | Passeggero esistente, corsa `OPEN`, nessuna prenotazione precedente, posti disponibili | Prenotazione creata, `available_seats` decrementato | Creazione confermata | ✅ PASS |
| 2 | Prenotazione su corsa non OPEN | Corsa con stato `IN_PROGRESS` | Eccezione "Corsa non ancora disponibile per prenotazioni" | Eccezione lanciata correttamente | ✅ PASS |
| 3 | Guidatore prenota la propria corsa | Utente coincide con il guidatore della corsa | Eccezione "Non puoi prenotare la tua stessa corsa" | Eccezione lanciata correttamente | ✅ PASS |
| 4 | Doppia prenotazione | Prenotazione già esistente per la coppia corsa-passeggero | Eccezione "Hai già prenotato questa corsa" | Eccezione lanciata correttamente | ✅ PASS |
| 5 | Nessun posto disponibile | `decrementAvailableSeats` restituisce 0 | Eccezione "Nessun posto disponibile" | Eccezione lanciata correttamente | ✅ PASS |
| 6 | Utente non trovato | Username inesistente | Eccezione "Utente non trovato" | Eccezione lanciata correttamente | ✅ PASS |
| 7 | Corsa non trovata | ID corsa inesistente | Eccezione "Corsa non trovata" | Eccezione lanciata correttamente | ✅ PASS |
| 8 | Cancellazione prenotazione valida | Prenotazione esistente, corsa `OPEN`, partenza tra 5 giorni | Prenotazione eliminata, `available_seats` incrementato | Eliminazione confermata | ✅ PASS |
| 9 | Cancellazione con meno di 24 ore | Corsa `OPEN`, partenza tra 10 ore | Eccezione "Non puoi cancellare una prenotazione con meno di 24 ore dalla partenza" | Eccezione lanciata correttamente | ✅ PASS |
| 10 | Abbandono corsa IN_PROGRESS | Corsa con stato `IN_PROGRESS` | Prenotazione eliminata, `available_seats` incrementato | Abbandono confermato | ✅ PASS |
| 11 | Abbandono corsa COMPLETED | Corsa con stato `COMPLETED` | Eccezione "Non puoi abbandonare una corsa già terminata" | Eccezione lanciata correttamente | ✅ PASS |
| 12 | Prenotazione non trovata | ID prenotazione inesistente per il passeggero | Eccezione "Prenotazione non trovata" | Eccezione lanciata correttamente | ✅ PASS |
| 13 | Lista prenotazioni con prenotazioni presenti | Passeggero con prenotazioni esistenti | Lista prenotazioni popolata | Lista corretta | ✅ PASS |
| 14 | Lista prenotazioni vuota | Passeggero senza prenotazioni | Lista vuota | Lista vuota confermata | ✅ PASS |

---

## 8. ReviewService (9 test)

| # | Caso di Test | Input | Risultato Atteso | Risultato Ottenuto | Esito |
|---|---|---|---|---|---|
| 1 | Recensione valida | Corsa `COMPLETED`, recensore è passeggero, nessuna recensione precedente | Recensione creata con recensore, recensito, voto e commento corretti | Creazione confermata | ✅ PASS |
| 2 | Recensione su corsa non COMPLETED | Corsa con stato `OPEN` | Eccezione "Puoi recensire solo corse completate" | Eccezione lanciata correttamente | ✅ PASS |
| 3 | Recensione da non passeggero | Utente non era passeggero della corsa | Eccezione "Non sei stato passeggero di questa corsa" | Eccezione lanciata correttamente | ✅ PASS |
| 4 | Doppia recensione | Recensione già esistente per la coppia corsa-recensore | Eccezione "Hai già recensito questa corsa" | Eccezione lanciata correttamente | ✅ PASS |
| 5 | Utente non trovato | Username inesistente | Eccezione "Utente non trovato" | Eccezione lanciata correttamente | ✅ PASS |
| 6 | Corsa non trovata | ID corsa inesistente | Eccezione "Corsa non trovata" | Eccezione lanciata correttamente | ✅ PASS |
| 7 | Lista recensioni con recensioni presenti | Utente con recensioni ricevute | Lista recensioni popolata con voto corretto | Lista corretta | ✅ PASS |
| 8 | Lista recensioni vuota | Utente senza recensioni ricevute | Lista vuota | Lista vuota confermata | ✅ PASS |
| 9 | Lista recensioni, utente non trovato | Username inesistente | Eccezione "Utente non trovato" | Eccezione lanciata correttamente | ✅ PASS |

---

## Riepilogo

| Componente | Sprint | Test | Esito |
|---|---|---|---|
| AuthService | 1 | 9 | ✅ Tutti passati |
| JwtFilter | 1 | 5 | ✅ Tutti passati |
| UserService | 1 | 14 | ✅ Tutti passati |
| RideMapper | 1 | 4 | ✅ Tutti passati |
| RideService | 1 | 7 | ✅ Tutti passati |
| RideService | 2 | 13 | ✅ Tutti passati |
| BookingService | 2 | 14 | ✅ Tutti passati |
| ReviewService | 2 | 9 | ✅ Tutti passati |
| **Totale** | | **75** | **✅ 75/75 passati** |