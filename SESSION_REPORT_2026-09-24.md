# xMasterCenter (xMC) - Session-Bericht

**Datum:** 24.09.2026 - 27.09.2026
**Projekt:** TolgaxBektas/xmc
**Branch:** `claude/review-backup-data-8Cji0`
**PR:** https://github.com/TolgaxBektas/xmc/pull/3

---

## 1. Projektübersicht

**xMasterCenter** ist eine webbasierte, auf GitHub Pages gehostete Business-Management-Anwendung für die Verwaltung mehrerer Mandanten (Firmen), deren Aufgaben, Rechnungen, Kunden und Zahlungsbestätigung.

- **Betreiber:** Ilexis Medya Ltd. Sti. (Istanbul, Türkei)
- **Nutzer:** Tolga Bektas
- **Technologie:** Vanilla HTML/CSS/JS (Single-File, ca. 1826 Zeilen)
- **Hosting:** GitHub Pages (statisch)
- **Datenhaltung:** localStorage (`xMasterCenter_data`)
- **Build-System:** Keines (kein npm, kein Framework)

---

## 2. Projektstruktur

```
/home/user/xmc/
  index.html                          -- Haupt-App (xMasterCenter Dashboard)
  cm830-baleo/
    index.html                        -- Payment Confirmation Portal (Baleo, ältere Version)
  cm830-baleo-acc/
    index.html                        -- Invoice Overview Portal (Baleo, neuere Version mit i18n)
    status.json                       -- Zahlungsstatus aller Baleo-Rechnungen
    docs/
      BA060058.pdf ... BA060068.pdf   -- 8 PDF-Rechnungen
  cm830-nordlink/
    index.html                        -- Payment Confirmation Portal (Nordlink, ältere Version)
  cm830-nordlink-acc/
    index.html                        -- Invoice Overview Portal (Nordlink, neuere Version mit i18n)
    status.json                       -- Zahlungsstatus aller Nordlink-Rechnungen
    docs/
      ND26-0011.pdf ... ND26-0015.pdf -- 5 PDF-Rechnungen
```

---

## 3. Implementierte Features

### 3.1 Authentifizierung
- PIN-basierter Login (4-stelliger PIN)
- SHA-256 Hash-basierte Verifizierung (via Web Crypto API)
- Logout-Funktion mit Session-Bereinigung

### 3.2 Mandantenverwaltung (7 Mandanten)

| Mandant | Standort |
|---|---|
| Baleo Services | Nordmazedonien, Skopje |
| Nordlink GmbH | Deutschland |
| Conrad Media | Österreich |
| Ilexis Medya | Türkei |
| Maximus Medya | Türkei |
| Nilay Bektas | Türkei |
| Quantia GmbH | Deutschland |

### 3.3 xActionCentral (Dashboard)
- Übersicht aller offenen Aufgaben über alle Mandanten hinweg
- KPI-Leiste: Offen, Hohe Priorität, Überfällig, Rechnungen offen, Bezahlt
- Wiedervorlage-Tabelle mit Prioritätssortierung

### 3.4 xWorkCoordination (Leistungsnetz)
- Mandantenübergreifende Aufgabenverwaltung
- 3 Ansichten: Listenansicht, Kanban-Board (Drag & Drop), Ablaufdiagramm (Flow-Editor)
- Suche, Filter nach Mandant/Typ/Status
- Sticky Notes (Schnellnotizen)
- **Aufgabentypen:** TODO, NOTIZ, AUFGABE, MEETING, ANRUF, E-MAIL, MAHNUNG, ERINNERUNG, SAMMELRECHNUNG, RECHNUNG, PRÜFUNG, ERFASSUNG, VERTRAG, BERICHT
- **Status:** OPEN, OVERDUE, READY, PENDING, DONE

### 3.5 xTASKmanager
- Persönliche Aufgaben (ohne Mandantenzuordnung)
- Listen- und Kanban-Ansicht

### 3.6 xAccounts (Untermodule)
- **Rechnungen:** CRUD, PDF-Generierung (jsPDF), E-Mail-Versand (mailto), Bezahlt-Markierung
- **Kunden:** Verwaltung mit Kontaktdaten, Leistungsbeschreibungen, E-Mail-Vorlagen
- **Schriftverkehr:** Platzhalter-Modul ("im Aufbau")
- **Mandant-Aufgaben:** Pro Mandant gefilterte Aufgabenansicht

### 3.7 Delegationssystem
- Aufgaben an Personen delegieren (Name, E-Mail, WhatsApp, Telegram)
- Automatische Erinnerungen (6h, täglich, 3 Tage, wöchentlich)
- Bestätigungslinks via URL-Token-System
- Browser-Benachrichtigungen

### 3.8 Einstellungen
- Briefvorlage pro Mandant (Firma, Adresse, Bank, IBAN, etc.)
- Mandantenübersicht
- JSON Import/Export (Briefvorlage und Gesamtdaten)
- Remote-Steuerung-Hinweis (geplante alexis.tr-Integration)
- Benachrichtigungseinstellungen

### 3.9 Mobile Responsiveness (PR #3)
- Media Queries für max-width 768px
- Collapsible Sidebar mit Hamburger-Menü
- Kanban-Board stapelt vertikal auf Mobile

### 3.10 Datenspeicherung
- Ausschließlich localStorage (`xMasterCenter_data`)
- 20 vorkonfigurierte Seed-Aufgaben + 1 Seed-Rechnung
- Migrations-Logik zum Ergänzen fehlender Seed-Einträge

---

## 4. Kunden-Portale

### 4.1 Baleo Services - Rechnungsportal

**Portal-URL:** `cm830-baleo-acc/index.html`
**Absender:** Conrad Media LTD

| Rechnungsnr. | Betrag (EUR) | Status |
|---|---|---|
| BA 060/058 | 3.425,25 | pending |
| BA 060/059 | 1.465,82 | pending |
| BA 060/063 | 7.390,30 | pending |
| BA 060/064 | 8.115,30 | pending |
| BA 060/065 | 13.303,65 | pending |
| BA 060/066 | 8.756,25 | pending |
| BA 060/067 | 13.308,18 | pending |
| BA 060/068 | 7.913,25 | pending |
| **Gesamt** | **63.678,00** | |

### 4.2 Nordlink GmbH - Rechnungsportal

**Portal-URL:** `cm830-nordlink-acc/index.html`
**Absender:** Conrad Media LTD

| Rechnungsnr. | Betrag (EUR) | Status |
|---|---|---|
| ND26-0011 | 17.838,00 | pending |
| ND26-0012 | 11.575,20 | pending |
| ND26-0013 | 10.472,80 | pending |
| ND26-0014 | 11.024,00 | pending |
| ND26-0015 | 8.268,00 | pending |
| **Gesamt** | **59.178,00** | |

### 4.3 Portal-Features
- Summary-Grid (Total, Received, Pending)
- EN/DE Sprachumschaltung (vollständige i18n)
- Lokalisierte Referenzen (KW/CW, Dez/Dec, Okt/Oct)
- PDF-Download-Links
- "Mark as Paid"-Button mit mailto-Vorlage
- `status.json`-Integration

---

## 5. Pull Request #3

**Titel:** "Add mobile responsiveness, PIN security, and logout functionality"
**Status:** Open, mergeable, clean
**CI:** Keine Checks konfiguriert
**Reviews:** Keine
**Änderungen:** +100 / -27 Zeilen, 1 Datei

### Enthaltene Änderungen:
1. Mobile Responsiveness (Hamburger-Menü, collapsible Sidebar)
2. PIN-Hashing mit SHA-256 (statt Klartext)
3. Logout-Button
4. Datenvalidierung (Array.isArray, typeof Checks)
5. XSS-Prävention (`esc()` Helper)
6. Flow-Node Connection Fix (Race Condition)
7. Neue Kunden-Records (Cleverer, Conrad Media, PM PROMEDIA, JTG Werbeverlag)
8. PDF-Generierung: mehrzeilige Positionen

---

## 6. Identifizierte Probleme

### 6.1 Sicherheit
- **PIN im Quellcode:** Hash ist sichtbar, bietet nur oberflächlichen Schutz
- **E-Mail-Adressen und Bankdaten** (IBAN, BIC) im Quellcode offen einsehbar
- **Kein HTTPS-erzwungener Login** - GitHub Pages bietet HTTPS, aber kein Server-Auth

### 6.2 Architektur
- **Monolithische Datei:** 1826 Zeilen HTML/CSS/JS in einer Datei
- **Kein Framework:** Vanilla JS, kein State-Management
- **Globaler Scope:** Namespace-Kollisionsgefahr
- **Keine Build-Pipeline:** Kein Minification, Bundling, oder Tree-Shaking

### 6.3 Datenhaltung
- **localStorage = Datenverlust-Risiko:** Browserwechsel, Cache-Bereinigung, Gerätewechsel
- **Keine serverseitige Persistenz** (auf GitHub Pages nicht möglich)
- **Remote-Steuerung (alexis.tr):** Geplant, noch nicht implementiert

### 6.4 Code-Duplikation
- `cm830-baleo-acc/` und `cm830-nordlink-acc/` sind zu ca. 95% identisch
- Nur der CONFIG-Block unterscheidet sich
- Sollte eine Template-Datei mit parametrisierter Konfiguration sein

### 6.5 Status-Inkonsistenz
- Haupt-App: `OPEN`, `PAID`, `OVERDUE`, `DRAFT`
- Kunden-Portale: `pending`, `received`
- Keine gemeinsame Statuslogik

### 6.6 Funktionale Lücken
- Schriftverkehr-Modul nur Platzhalter
- PDF-Generierung: einfaches Text-Layout (kein professionelles Design)
- Kein Dark Mode in der Haupt-App

---

## 7. Verfügbare MCP-Server / Skills (Session-Umgebung)

Folgende externe Dienste waren in dieser Session konfiguriert (Verbindungsprobleme während der Session aufgetreten):

| Dienst | Einsatzgebiet für xMC |
|---|---|
| **Adobe for creativity** | Professionelle PDF-Rechnungen, Design |
| **Airtable** | Datenbank-Backend (Ersatz für localStorage) |
| **Gmail** | Direkter E-Mail-Versand (statt mailto) |
| **Google Drive** | Zentrale Dokumentenablage |
| **DeepL** | Automatische Übersetzung (EN/DE/TR) |
| **Netlify** | Dynamisches Hosting mit Serverless Functions |
| **Cloudflare** | Hosting + D1 Database + KV Storage |
| **Slack** | Team-Benachrichtigungen |
| **PDF.net** | PDF-Bearbeitung und -Generierung |
| **Claude Docs** | Lebende Dokumente |
| **Webflow** | Website-Builder |
| **Twilio** | SMS-Benachrichtigungen |
| **Wispr Flow** | Meeting-/Kalender-Integration |
| **Turkish Airlines** | Reise-Integration |
| **Booking.com** | Unterkunftssuche |
| **Expedia** | Reise-Integration |

---

## 8. Optimierungsvorschläge

### 8.1 Sofort umsetzbar (Quick Wins)

| Maßnahme | Aufwand | Nutzen |
|---|---|---|
| CLAUDE.md erstellen | 30 Min | Dokumentierte Workflows |
| Code-Duplikation Portale beseitigen | 1-2 Std | Wartbarkeit |
| Status-Werte vereinheitlichen | 1 Std | Konsistenz |
| Schriftverkehr-Modul implementieren | 2-3 Std | Funktionslücke schließen |

### 8.2 Mittelfristig (mit MCP-Integration)

| Maßnahme | Vorteil |
|---|---|
| **Airtable als Backend** | Persistente Daten, mehrbenutzerfähig |
| **Gmail-Integration** | Automatischer Rechnungsversand |
| **DeepL-Integration** | Automatische Übersetzung aller Texte |
| **Rechnungsportal mit Token-Links** | Empfänger können Rechnungen einsehen und bestätigen |

### 8.3 Langfristig (Architektur-Upgrade)

| Maßnahme | Vorteil |
|---|---|
| **Modularisierung** | Wartbarkeit, Testbarkeit |
| **Cloudflare/Netlify Hosting** | Serverless Functions, DB |
| **Dark Mode Haupt-App** | Benutzerfreundlichkeit |
| **Professionelle PDF-Vorlagen** | Adobe/PDF.net Integration |

---

## 9. Offene Rechnungen - Gesamtübersicht

### Gesamtforderungen: EUR 122.856,00

| Kunde | Anzahl Rechnungen | Gesamtbetrag (EUR) | Status |
|---|---|---|---|
| Baleo Services | 8 | 63.678,00 | Alle pending |
| Nordlink GmbH | 5 | 59.178,00 | Alle pending |
| **Gesamt** | **13** | **122.856,00** | |

---

## 10. Git-Historie (letzte Commits)

| Datum | Beschreibung |
|---|---|
| 07.04.2026 | i18n-Lokalisierung der Rechnungsreferenzen |
| 07.04.2026 | Nordlink Invoice Overview Portal |
| 07.04.2026 | status.json Integration |
| 07.04.2026 | Mark-as-Paid Feature |
| 07.04.2026 | PDF-Download Links |
| 07.04.2026 | Baleo Invoice Overview Portal |
| 07.04.2026 | Baleo Payment Confirmation Portal |
| 07.04.2026 | Nordlink Payment Confirmation Portal |
| 03.2026 | Initial Upload und HTML-Strukturierung |

---

## 11. Konfigurationsstatus

- **CLAUDE.md:** Nicht vorhanden
- **.claude/ Verzeichnis:** Nicht vorhanden
- **package.json:** Nicht vorhanden
- **CI/CD Pipeline:** Nicht konfiguriert
- **Hooks:** Keine
- **Skills:** Keine projektspezifischen

---

## 12. Session-Diskussionen

### 12.1 Rechnungsportal-Konzept
Es wurde die Frage eines Rechnungsportals diskutiert, bei dem Agenturen Rechnungen im Portal der Webseite aufgelistet bekommen zum Download und Bezahlung. Das bestehende Token-basierte Bestätigungssystem (für delegierte Aufgaben) könnte als Grundlage dienen. Die Portale `cm830-baleo-acc` und `cm830-nordlink-acc` sind bereits funktionierende Implementierungen dieses Konzepts für zwei Kunden.

### 12.2 Skills/MCP-Server Audit
Es wurde geprüft, ob der Ablauf angesichts der vielen neu installierten Skills (Adobe, Airtable, Gmail, etc.) neu generiert werden sollte. Ergebnis: Ja, die neuen Tools ermöglichen signifikante Verbesserungen insbesondere bei:
- E-Mail-Versand (Gmail statt mailto)
- Datenhaltung (Airtable statt localStorage)
- PDF-Erstellung (Adobe/PDF.net statt jsPDF)
- Übersetzung (DeepL statt manuell)

### 12.3 MCP-Server Verbindungsprobleme
Alle 18 konfigurierten MCP-Server haben während der Session Verbindungsprobleme gezeigt (HTTP 400: CLIENT_HTTP_NOT_IMPLEMENTED). Dies ist ein temporäres Verbindungsproblem, keine fehlende Konfiguration.

---

*Erstellt am 27.09.2026 | Session: claude/review-backup-data-8Cji0*
