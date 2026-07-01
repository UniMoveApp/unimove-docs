---

**Progetto:** UniMove  
**Sprint:** 2  
**Autore:** Luca Lanese  
**Data:** Luglio 2026  
**Strumenti:** JUnit 5, Mockito, AssertJ  
**Totale test Sprint 2:** 35 — tutti passati

---

## 6. RideService — Nuovi metodi Sprint 2 (14 test)

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

## 7. BookingService (13 test)

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
| 12 | Lista prenotazioni con prenotazioni presenti | Passeggero con prenotazioni esistenti | Lista prenotazioni popolata | Lista corretta | ✅ PASS |
| 13 | Lista prenotazioni vuota | Passeggero senza prenotazioni | Lista vuota | Lista vuota confermata | ✅ PASS |

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

## Riepilogo Aggiornato

| Componente | Sprint | Test | Esito |
|---|---|---|---|
| AuthService | 1 | 9 | ✅ Tutti passati |
| JwtFilter | 1 | 5 | ✅ Tutti passati |
| UserService | 1 | 14 | ✅ Tutti passati |
| RideMapper | 1 | 4 | ✅ Tutti passati |
| RideService | 1 | 7 | ✅ Tutti passati |
| RideService | 2 | 13 | ✅ Tutti passati |
| BookingService | 2 | 13 | ✅ Tutti passati |
| ReviewService | 2 | 9 | ✅ Tutti passati |
| **Totale** | | **74** | **✅ 74/74 passati** |