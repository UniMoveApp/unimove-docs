# 1. Introduzione

Lo scopo di questo documento (Object Design Document) è documentare in dettaglio le decisioni di Object Design, rendendo esplicite le interfacce tra le classi e i sottosistemi del sistema UniMove. Il documento funge da strumento di coordinamento e fornisce un riferimento tecnico condiviso per gli architetti, gli sviluppatori e i tester, garantendo la coerenza tra implementazione e progettazione architetturale.

## 1.1 Object design trade-offs
Durante la fase di Object Design per UniMove, il team ha affrontato diverse scelte progettuali, accettando i seguenti compromessi (trade-offs):
* **Performance vs. Semplicità del codice:** Per garantire tempi di risposta rapidi su rete mobile e l'aggiornamento in tempo reale delle code di prenotazione, si è talvolta preferito ottimizzare i percorsi di accesso ai dati e le interrogazioni al database, sacrificando in parte la semplicità strutturale del codice.
* **Riuso di librerie esistenti (Buy vs. Build) vs. Sviluppo custom:** Si è deciso di massimizzare il riuso di componenti "off-the-shelf" (COTS) e framework consolidati (come Spring Boot e Flutter) per ridurre i tempi di sviluppo e minimizzare i rischi, preferendoli alla scrittura di soluzioni proprietarie che avrebbero richiesto maggiore sforzo e collaudo.
* **Flessibilità vs. Semplicità architetturale:** L'applicazione di design pattern aumenta la flessibilità e l'estendibilità futura del sistema (es. anticipazione del cambiamento), ma introduce un livello di astrazione e complessità aggiuntivo rispetto a un'architettura più rigida e basilare.

## 1.2 Linee guida per la documentazione delle interfacce
Per garantire uniformità e coerenza, la documentazione delle interfacce del sistema segue le seguenti linee guida predefinite:
* **Uso di Javadoc:** Per il backend Spring Boot, la documentazione viene generata e mantenuta direttamente nel codice tramite Javadoc. Ogni commento descrive le responsabilità della classe, le signature dei metodi, i parametri (`@param`), i valori di ritorno (`@return`) e le eccezioni sollevate (`@throws`).
* **Convenzioni di naming:** Alle classi vengono assegnati nomi univoci basati su sostantivi (es. `User`), mentre i metodi sono identificati da verbi che descrivono l'azione eseguita (es. `updatePreferences()`).
* **Livelli di visibilità:** La visibilità di ogni attributo e operazione (`public`, `private`, `protected`) deve essere specificata rigorosamente in fase di design.
* **Principio Need to Know (Information Hiding):** Le interfacce pubbliche espongono solo ciò che è strettamente necessario al chiamante, proteggendo gli attributi con accessi privati e nascondendo i dettagli implementativi e le logiche interne.

## 1.3 Definizioni, acronimi e abbreviazioni
Di seguito sono definiti i termini tecnici utilizzati nel presente documento:
* **API (Application Programming Interface):** Insieme di procedure e strumenti che permettono la comunicazione tra il client mobile e il backend.
* **DTO (Data Transfer Object):** Oggetti utilizzati esclusivamente per trasportare i dati tra i sottosistemi o attraverso le chiamate API REST, nascondendo l'implementazione interna del database.
* **Repository:** Oggetto di soluzione o pattern che incapsula la logica necessaria per accedere ai dati persistenti del sistema.
* **Service:** Oggetto di soluzione che contiene e coordina la logica di business complessa dell'applicazione.
* **Controller (Control):** Oggetto di soluzione che gestisce il flusso applicativo, ricevendo gli input dall'interfaccia (Boundary) e orchestrando gli Entity.
* **Entity:** Oggetto di applicazione che rappresenta i concetti e le informazioni persistenti del dominio reale (es. Studente, Evento).
* **COTS (Commercial Off-The-Shelf):** Componenti software commerciali o open-source già esistenti e pronti per essere integrati nel sistema per favorire il riuso.
* **Design Pattern:** Schema risolutivo generale, riutilizzabile e adattabile per un problema di progettazione ricorrente.

## 1.4 Riferimenti
* **RAD:** Requirements Analysis Document del progetto UniMove.
* **SDD:** System Design Document del progetto UniMove.
* **Spring Boot (Documentazione ufficiale):** https://spring.io/projects/spring-boot [25].
* **Flutter (Documentazione ufficiale):** https://docs.flutter.dev/ [25].
