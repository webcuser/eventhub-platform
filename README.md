# eventhub-platform

Piattaforma web per gestione, creazione e prenotazione eventi.

## Overview

# Product Requirements Document (PRD) — eventhub-platform

---

## 1. Project Overview

**eventhub-platform** è una piattaforma web per la creazione, gestione e prenotazione di eventi, progettata per semplificare l’intero ciclo di vita di un evento. Consente agli organizzatori di pubblicare eventi (fisici e online), gestire prenotazioni e vendite di biglietti, comunicare con i partecipanti e monitorare le performance degli eventi. Gli utenti possono esplorare eventi, prenotare o acquistare biglietti e ricevere notifiche in tempo reale. L’obiettivo è centralizzare la gestione degli eventi, eliminando la frammentazione tra strumenti diversi e migliorando l’esperienza di organizzatori e partecipanti.

---

## 2. Goals & Success Metrics

### Obiettivi

- Centralizzare la gestione di eventi, prenotazioni e comunicazioni.
- Offrire un’esperienza utente fluida e sicura sia per organizzatori che per partecipanti.
- Supportare eventi sia gratuiti che a pagamento, con integrazione di provider di pagamento esterni.
- Fornire dashboard e reportistica dettagliata per organizzatori e amministratori.

### Success Metrics

- **Tasso di successo nella creazione eventi:** ≥ 95% degli organizzatori riesce a pubblicare un evento senza errori.
- **Prenotazioni completate:** ≥ 90% delle prenotazioni avviate vengono completate (pagamento o conferma).
- **Tempo medio di risposta API:** < 2 secondi per il 99% delle richieste.
- **Notifiche recapitate:** ≥ 99% delle notifiche email e in-app vengono consegnate con successo.
- **Uptime piattaforma:** ≥ 99.5% mensile.
- **Zero incidenti di sicurezza noti** (data breach, accessi non autorizzati).

---

## 3. Target Users

### 3.1 Partecipante

- Ricerca eventi per data, luogo, tipologia.
- Prenota o acquista biglietti.
- Gestisce le proprie prenotazioni e biglietti.
- Riceve notifiche e promemoria.

### 3.2 Organizzatore

- Crea, modifica, pubblica e archivia eventi.
- Gestisce prenotazioni e vendite.
- Comunica con i partecipanti.
- Visualizza statistiche e report.
- Collabora con altri organizzatori tramite ruoli granulari.

### 3.3 Amministratore

- Supervisiona la piattaforma.
- Modera eventi, utenti e contenuti.
- Gestisce segnalazioni e reclami.
- Consulta audit log e report globali.

---

## 4. Core Features

### 4.1 Gestione Account

- **Registrazione e autenticazione**  
  - Supporto per email/password.
  - Recupero password.
  - Autenticazione tramite Laravel Sanctum (token-based).
- **Gestione profilo utente**  
  - Modifica dati personali, immagine profilo, telefono.
- **Gestione ruoli e permessi**  
  - Ruoli: Partecipante, Organizzatore principale, Collaboratore, Amministratore.
  - Permessi granulari per team di organizzatori.

### 4.2 Gestione Eventi

- **Creazione/modifica/eliminazione eventi**  
  - Inserimento dati obbligatori: titolo, descrizione, data/ora, tipologia (fisico/online), capienza, stato.
  - Dati aggiuntivi per eventi fisici: indirizzo, città, luogo, coordinate.
  - Dati aggiuntivi per eventi online: piattaforma, link, password, istruzioni.
  - Upload immagini evento.
  - Definizione prezzo biglietto (gratuito/a pagamento).
- **Pubblicazione e archiviazione eventi**  
  - Stato evento: bozza, pubblicato, archiviato, sospeso.
- **Gestione collaboratori**  
  - Invito e gestione di altri organizzatori con ruoli specifici.

### 4.3 Prenotazioni e Biglietti

- **Prenotazione e acquisto biglietti**  
  - Prenotazione gratuita o pagamento online (Stripe).
  - Gestione quantità biglietti per prenotazione.
  - Verifica disponibilità in tempo reale.
- **Gestione stato prenotazione**  
  - Stati: In attesa, Confermata, Pagamento non riuscito, Annullata.
- **Generazione biglietto elettronico**  
  - QR Code univoco per ogni biglietto.
  - Download biglietto in PDF.
- **Storico prenotazioni**  
  - Visualizzazione e gestione prenotazioni passate e future.

### 4.4 Pagamenti

- **Integrazione provider esterno (Stripe)**  
  - Creazione sessione pagamento.
  - Gestione webhook per conferma/rifiuto pagamento.
  - Idempotenza delle transazioni.
  - Gestione rimborsi tramite provider.
- **Notifiche stato pagamento**  
  - Email e in-app per ogni aggiornamento.

### 4.5 Comunicazione & Notifiche

- **Notifiche in-app real-time**  
  - Aggiornamenti su prenotazioni, eventi, messaggi, pagamenti (WebSocket).
  - Fallback polling periodico.
- **Notifiche email**  
  - Conferme, promemoria, aggiornamenti eventi, stato pagamenti.
- **Messaggi organizzatore-partecipante**  
  - Invio comunicazioni di massa o individuali.
- **Promemoria automatici**  
  - Reminder automatici prima dell’evento.

### 4.6 Dashboard & Report

- **Elenco eventi creati/prenotati**  
  - Filtri per stato, data, tipologia.
- **Statistiche e report**  
  - Iscrizioni, partecipazione, vendite biglietti.
  - Esportazione dati (CSV, PDF).
- **Report amministrativi**  
  - Log attività, segnalazioni, performance piattaforma.

### 4.7 Moderazione & Amministrazione

- **Gestione utenti e organizzatori**  
  - Sospensione, riattivazione, eliminazione account.
- **Moderazione eventi e contenuti**  
  - Approvazione, modifica, sospensione, eliminazione eventi.
  - Moderazione descrizioni, immagini, segnalazioni.
- **Audit log**  
  - Storico dettagliato di tutte le azioni amministrative.
- **Ripristino contenuti**  
  - Possibilità di annullare azioni amministrative errate.

---

## 5. Technical Architecture

### 5.1 Tech Stack

- **Backend:** Laravel (PHP)
- **Frontend:** Vue.js (SPA)
- **Database:** MySQL
- **Cache & Queue:** Redis
- **Storage:** Amazon S3 (o compatibile)
- **Containerizzazione:** Docker
- **Autenticazione:** Laravel Sanctum
- **Pagamenti:** Stripe
- **Notifiche real-time:** WebSocket (es. Laravel Echo + Pusher o soluzione self-hosted)

### 5.2 Data Models (Schema semplificato)

#### Utente

| Campo         | Tipo     | Obbligatorio | Note                |
|---------------|----------|--------------|---------------------|
| id            | UUID     | Sì           |                     |
| nome          | String   | Sì           |                     |
| cognome       | String   | Sì           |                     |
| email         | String   | Sì           | Unico               |
| password      | String   | Sì           | Hash sicuro         |
| ruolo_id      | FK       | Sì           |                     |
| telefono      | String   | No           |                     |
| immagine      | String   | No           | URL profilo         |

#### Evento

| Campo             | Tipo     | Obbligatorio | Note                          |
|-------------------|----------|--------------|-------------------------------|
| id                | UUID     | Sì           |                               |
| titolo            | String   | Sì           |                               |
| descrizione       | Text     | Sì           |                               |
| data_ora          | DateTime | Sì           |                               |
| tipologia         | Enum     | Sì           | online/fisico                 |
| organizzatore_id  | FK       | Sì           | Utente                        |
| capienza          | Int      | Sì           |                               |
| stato             | Enum     | Sì           | bozza, pubblicato, archiviato |
| indirizzo         | String   | No           | Solo fisico                   |
| città             | String   | No           | Solo fisico                   |
| luogo             | String   | No           | Solo fisico                   |
| lat/lon           | Float    | No           | Solo fisico                   |
| piattaforma       | String   | No           | Solo online                   |
| link_online       | String   | No           | Solo online                   |
| password_online   | String   | No           | Solo online                   |
| istruzioni        | Text     | No           | Solo online                   |
| immagini          | Array    | No           | URL immagini                  |
| prezzo            | Decimal  | No           | 0 = gratuito                  |

#### Prenotazione

| Campo         | Tipo     | Obbligatorio | Note          |
|---------------|----------|--------------|---------------|
| id            | UUID     | Sì           |               |
| evento_id     | FK       | Sì           |               |
| utente_id     | FK       | Sì           |               |
| stato         | Enum     | Sì           | vedi sopra    |
| quantita      | Int      | Sì           |               |
| note          | Text     | No           |               |

#### Biglietto

| Campo            | Tipo     | Obbligatorio | Note              |
|------------------|----------|--------------|-------------------|
| id               | UUID     | Sì           |                   |
| prenotazione_id  | FK       | Sì           |                   |
| qr_code          | String   | Sì           | univoco           |
| stato            | Enum     | Sì           | attivo, annullato |
| posto_assegnato  | String   | No           |                   |

#### Ruolo

| Campo        | Tipo     | Obbligatorio | Note    |
|--------------|----------|--------------|---------|
| id           | UUID     | Sì           |         |
| nome         | String   | Sì           |         |
| descrizione  | Text     | No           |         |

#### Relazioni chiave

- Un utente può avere più ruoli (tramite tabella pivot per team organizzatori).
- Un evento può avere più organizzatori (ruolo principale/collaboratore).
- Un evento ha molte prenotazioni.
- Una prenotazione genera uno o più biglietti.

### 5.3 API Contracts (REST)

#### Eventi

- `GET /api/events` — Lista eventi (filtri: stato, tipologia, data)
- `GET /api/events/{id}` — Dettaglio evento
- `POST /api/events` — Creazione evento
- `PUT /api/events/{id}` — Modifica evento
- `DELETE /api/events/{id}` — Eliminazione evento

#### Prenotazioni

- `POST /api/bookings` — Nuova prenotazione
- `GET /api/bookings` — Lista prenotazioni utente
- `GET /api/bookings/{id}` — Dettaglio prenotazione
- `DELETE /api/bookings/{id}` — Annulla prenotazione

#### Pagamenti

- `POST /api/payments/create` — Avvia pagamento (Stripe)
- `POST /api/payments/webhook` — Ricezione callback provider
- `GET /api/payments/{id}` — Stato pagamento

#### Codici di risposta standard

- 200 OK, 201 Created, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 422 Unprocessable Entity, 500 Internal Server Error

---

## 6. Non-Functional Requirements

- **Interfaccia responsive** (mobile-first).
- **Performance**: tempo medio risposta API < 2s; supporto a carichi elevati tramite caching e code.
- **Sicurezza**:
  - HTTPS/TLS obbligatorio.
  - Hashing password (Argon2/bcrypt).
  - Cifratura dati sensibili a riposo.
  - Rate limiting su endpoint critici.
  - Audit log dettagliato per azioni amministrative e sensibili.
  - Validazione e sanitizzazione input.
  - Backup periodici e disaster recovery.
- **Scalabilità**: architettura modulare, containerizzazione (Docker), storage esterno (S3).
- **API RESTful**: per frontend e integrazioni future.
- **Notifiche real-time**: WebSocket con fallback polling.
- **Disponibilità**: uptime ≥ 99.5%.

---

## 7. Out of Scope (v1)

- Supporto multilingua (solo italiano in v1).
- Integrazione con social login (Google, Facebook, ecc.).
- Marketplace pubblico di eventi (solo eventi creati dagli utenti registrati).
- App mobile nativa (solo web responsive).
- Gestione avanzata seating (posti numerati, mappe interattive).
- Integrazione con sistemi di fatturazione elettronica.
- API pubbliche per terze parti (solo API interne/autenticate).
- Personalizzazione avanzata dei biglietti (branding organizzatore).

---

## 8. Open Questions

- **Gestione privacy e GDPR**: definire policy privacy, gestione consensi e diritto all’oblio.
- **Limiti su capienza evento**: prevedere overbooking o lista d’attesa?
- **Gestione coupon/sconti**: prevista in v1 o post-lancio?
- **Supporto multi-valuta**: Stripe