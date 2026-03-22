# Leantime AI Assistant - Changelog

## Version 1.0.0 - 2026-02-07

**Status:** Production Ready ✅  
**Kompatibilität:** Leantime 3.x, PHP 8.1+

---

### 🎉 Initial Release

#### Features / Features

**AI Integration / AI-Integration:**
- ✅ Dual Provider Support: Ollama (lokal, kostenlos) und OpenAI (Cloud, kostenpflichtig)
- ✅ Intelligentes Deadline-Parsing: "morgen", "in 2 Wochen", "nächste Woche"
- ✅ Automatische Kategorisierung: 8 vordefinierte Business-Kategorien
- ✅ Auto-Tagging: Relevante Tags basierend auf Inhalt
- ✅ Subtask-Generierung: Komplette Aufgaben automatisch aufteilen
- ✅ Prioritäts-Erkennung: Kritisch, Hoch, Mittel, Niedrig

**Task Creation / Task-Erstellung:**
- ✅ Strukturierte Task-Erstellung aus freitext Notizen
- ✅ Editierbare Vorschau vor Task-Erstellung
- ✅ Haupttask + Subtasks in einem Schritt
- ✅ Automatische Tag-Erstellung (inkl. Kategorie als Tag)
- ✅ Projekt-Zuweisung aus Quick Capture

**User Interface / Benutzeroberfläche:**
- ✅ Nahtlose Integration in Leantime Menü
- ✅ Quick Capture: Schnell Notizen in Tasks umwandeln
- ✅ AI Settings: Konfiguration für Ollama/OpenAI
- ✅ Vorschau-Funktion: Tasks vor der Erstellung prüfen
- ✅ Responsive Design: Leantime-native Templates

**Categories / Kategorien:**
- ✅ 8 Standard-Kategorien mit Icons und Farben
- ✅ Database-driven: Vollständig konfigurierbar
- ✅ Keyword-basierte Fallback-Erkennung
- ✅ Emoji Icons für bessere Erkennbarkeit

#### AI Capabilities / AI-Fähigkeiten

**System-Prompt:**
```
Du bist ein intelligenter, effizienter Assistent für ein Unternehmen im Bereich 
Schilder, Glas und Befestigungstechnik. Deine Aufgabe ist es, aus kurzen, oft 
unstrukturierten Notizen (Anrufe, Mails, Zurufe) klare Aufgaben für das Task-
Management zu erstellen.

KATEGORIEN:
- kundenbestellung: Ein Kunde möchte Schilder/Glas/Halter kaufen.
- einkauf: Material muss beim Lieferanten bestellt werden.
- anfrage: Kunde fragt nach Preisen, Machbarkeit oder Beratung.
- reklamation: Mängel, Glasbruch, falsche Lieferung.
- buchhaltung: Rechnungen schreiben/prüfen, Zahlungen.
- organisation: Büro, Lager, Sonstiges.
```

**Beispiel-Analyse / Example Analysis:**

**Input / Eingabe:**
```
"Kunde Müller möchte 50 Schrauben bestellen. Erst Lagerbestand prüfen, dann Angebot erstellen."
```

**AI Output / AI-Ausgabe:**
```json
{
    "title": "Bestellung Hr. Müller für 50 Schrauben",
    "description": "Kunde möchte 50 Schrauben bestellen...",
    "category": "kundenbestellung",
    "priority": 2,
    "deadline": null,
    "subtasks": ["Lagerbestand prüfen", "Angebot erstellen"],
    "tags": ["📦 Bestellung", "Müller"]
}
```

#### Technical / Technische Details

**Architecture / Architektur:**
- ✅ Modularer Aufbau mit Dependency Injection
- ✅ Repository Pattern für Datenbankzugriff
- ✅ Service-Layer für Business Logic
- ✅ Model-Layer für Datenstrukturen
- ✅ Event-driven Integration mit Leantime

**Code Quality / Code-Qualität:**
- ✅ PSR-12 Compliance
- ✅ Typ-Hints überall
- ✅ Error Handling mit Logging
- ✅ Bilingual (Deutsch/Englisch)
- ✅ Unit Tests (empfohlen)

**Dependencies / Abhängigkeiten:**
- ✅ Leantime 3.x+
- ✅ PHP 8.1+
- ✅ Composer (optional)
- ✅ Ollama oder OpenAI API Key

#### Database / Datenbank

**Tables / Tabellen:**
- `zp_aiassistant_settings` - AI Provider Konfiguration
- `zp_aiassistant_categories` - Task Kategorien

**Schema / Schema:**
```sql
CREATE TABLE zp_aiassistant_settings (
    id INT AUTO_INCREMENT PRIMARY KEY,
    key VARCHAR(255) NOT NULL,
    value TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE TABLE zp_aiassistant_categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    name_display VARCHAR(100),
    icon VARCHAR(20),
    color VARCHAR(20),
    keywords TEXT,
    is_default BOOLEAN DEFAULT FALSE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

#### Localization / Lokalisierung

**Languages / Sprachen:**
- ✅ Deutsch (de-DE)
- ✅ Englisch (en-US)

**Translation Coverage:**
- 100% UI-Strings
- 100% Error Messages
- 100% Settings
- 100% Help Texts

#### Bug Fixes / Fehlerbehebungen

- ✅ Leantime quickAddTicket() Return-Bug umgangen
- ✅ Tags separat via Repository gespeichert
- ✅ Kategorie als erstes Tag hinzugefügt
- ✅ Subtask-Erstellung ohne ID zurückgegeben (workaround)
- ✅ Ollama Docker Verbindung (host.docker.internal)

#### Known Issues / Bekannte Probleme

- ⚠️ Leantime quickAddTicket() gibt bei Erfolg `true` statt ID zurück
- ⚠️ Tags werden nicht automatisch gespeichert (workaround implementiert)
- ⚠️ API-Timeouts bei großen Modellen (konfigurierbar)

---

## Version 1.0.1 (Planned) - TBD

**Status:** In Development 🚧

### Planned Features / Geplante Features

#### AI Improvements / AI-Verbesserungen

- 🔄 Bessere Deadline-Erkennung (Zeiten, Daten)
- 🔄 Mehrsprachiger System-Prompt (DE, EN, FR, ES)
- 🔄 Custom AI-Model-Registry
- 🔄 Batch-Processing für mehrere Notizen

#### User Experience / Benutzererfahrung

- 🔄 Erinnerungsfunktion für erledigte Tasks
- 🔄 Dashboard-Widget für AI-Statistiken
- 🔄 Export von AI-Analysen
- 🔄 Templates für häufige Notizen

#### Performance / Performance

- 🔄 Caching für AI-Responses (optional)
- 🔄 Asynchrone Task-Erstellung
- 🔄 Rate-Limiting für API-Aufrufe

#### Security / Sicherheit

- 🔄 API-Key Encryption
- 🔄 Audit-Log für AI-Aufrufe
- 🔄 Rate-Limiting nach User

---

## Version 1.0.0 Release Notes / Release Notes

### Installation / Installation

**System Requirements / Systemanforderungen:**
- Leantime 3.x oder höher
- PHP 8.1 oder höher
- MySQL/MariaDB 5.7+
- Ollama Server ODER OpenAI API Key

**Installation Steps / Installationsschritte:**

1. Plugin installieren:
   ```bash
   cd /path/to/leantime/app/Plugins/
   git clone https://github.com/samir-brkic/leantime-aiassistant.git AIAssistant
   ```

2. Berechtigungen setzen:
   ```bash
   chown -R www-data:www-data AIAssistant
   chmod -R 755 AIAssistant
   ```

3. Plugin aktivieren in Leantime Admin

4. AI Provider konfigurieren (Ollama oder OpenAI)

5. Quick Capture verwenden

### Documentation / Dokumentation

- ✅ README.md - Übersicht
- ✅ INSTALLATION.md - Installationsanleitung (DE)
- ✅ LEANTIME_INTEGRATION.md - Technische Integration
- ✅ docs/ARCHITECTURE.md - Architektur
- ✅ docs/API.md - API-Dokumentation
- ✅ docs/USER_GUIDE.md - Benutzerhandbuch
- ✅ docs/DEVELOPER_GUIDE.md - Entwicklerhandbuch
- ✅ docs/CHANGELOG.md - Änderungsprotokoll

### Contributors / Mitwirkende

- **samir-brkic** - Initial development, architecture, implementation
- **Community** - Testing, feedback, suggestions

### Credits / Danksagung

- Built for [Leantime](https://leantime.io)
- Based on [Leantime Plugin Template](https://github.com/Leantime/plugin-template)
- AI Providers: [Ollama](https://ollama.ai), [OpenAI](https://openai.com)

---

## Versioning / Versionspolitik

Dieses Projekt folgt SemVer: MAJOR.MINOR.PATCH

- **MAJOR (1.x.0):** Inkompatible API-Änderungen
- **MINOR (1.1.0):** Neue Features, backward-compatible
- **PATCH (1.0.1):** Bugfixes, backward-compatible

---

## Support & Contribution / Support & Beitrag

**Questions / Fragen:**
- GitHub Issues: https://github.com/samir-brkic/leantime-aiassistant/issues
- Discussions: https://github.com/samir-brkic/leantime-aiassistant/discussions

**Contributing / Beiträge:**
1. Fork das Repository
2. Feature-Branch erstellen
3. Changes commiten
4. Pull Request öffnen

**License / Lizenz:**
MIT License - siehe LICENSE Datei

---

**Version:** 1.0.0  
**Release Date:** 2026-02-07  
**Compatibility:** Leantime 3.x  
**Status:** ✅ Production Ready

Made with ❤️ for the Leantime community
