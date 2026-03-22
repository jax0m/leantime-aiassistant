# Changelog

All notable changes to the Leantime AI Assistant Plugin will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-02-07

### 🐛 Fixed
- **Tags now correctly saved**: Tags were passed to `quickAddTicket()` but ignored by Leantime. Now saved separately via `patchTicket()` after task creation. Works for both main tasks and subtasks.
- **Leantime quickAddTicket() Return-Bug umgangen**: Workaround implemented for Leantime's buggy return type

### ✨ Added
- **Category as tag**: Categories are now saved as the first tag with emoji (e.g., "🛒 Kundenbestellung") instead of a description badge. This makes them filterable and searchable via Leantime's native tag system.
  - *Kategorien als Tag*: Kategorien werden als erstes Tag mit Emoji gespeichert
- **Dynamic date replacement**: System prompt now supports `{{CURRENT_DATE}}` placeholder which is replaced with the current date before sending to AI. This helps AI calculate relative deadlines accurately.
  - *Dynamisches Datums-Ersetzen*: System-Prompt unterstützt `{{CURRENT_DATE}}` Platzhalter

### 🔄 Changed
- **New industry-specific system prompt**: Rewritten default prompt specialized for signage/glass/fastening industry with 6 precise categories (kundenbestellung, einkauf, anfrage, reklamation, buchhaltung, organisation).
  - *Neuer branchenspezifischer System-Prompt*: Neuschriebener Standard-Prompt für Schilder/Glas/Befestigungstechnik mit 6 präzisen Kategorien
- **Cleaner task descriptions**: Category badge removed from description field. Descriptions now contain only the actual task content + AI-generated note.
  - *Sauberere Task-Beschreibungen*: Kategorie-Badge entfernt, nur Task-Inhalt + AI-Notiz

### 🔧 Technical
- `Services/AIAssistant.php`: `getSystemPrompt()` now replaces `{{CURRENT_DATE}}` placeholder
- `Services/AIAssistant.php`: `getDefaultSystemPrompt()` completely rewritten with industry focus
- `Services/TaskGenerator.php`: `saveTags()` extended with category parameter
- `Services/TaskGenerator.php`: `formatDescription()` simplified (no category badge)
- `Services/TaskGenerator.php`: `createMainTask()` passes category to `saveTags()`

---

## [1.0.0] - 2026-02-07

### 🎉 Initial Release / Initial Release

**AI Integration / AI-Integration:**
- Dual Provider Support: Ollama (lokal, kostenlos) und OpenAI (Cloud, kostenpflichtig)
- Dual Provider Support: Ollama (local, free) and OpenAI (cloud, paid)
- Intelligentes Deadline-Parsing: "morgen", "in 2 Wochen", "nächste Woche"
- Intelligent deadline parsing: "tomorrow", "in 2 weeks", "next week"
- Automatische Kategorisierung: 8 vordefinierte Business-Kategorien
- Auto categorization: 8 predefined business categories
- Auto-Tagging: Relevante Tags basierend auf Inhalt
- Auto-tagging: Relevant tags based on content
- Subtask-Generierung: Komplette Aufgaben automatisch aufteilen
- Subtask generation: Automatically split complex tasks
- Prioritäts-Erkennung: Kritisch, Hoch, Mittel, Niedrig
- Priority recognition: Critical, High, Medium, Low

**Task Creation / Task-Erstellung:**
- Strukturierte Task-Erstellung aus freitext Notizen
- Structured task creation from free-text notes
- Editierbare Vorschau vor Task-Erstellung
- Editable preview before task creation
- Haupttask + Subtasks in einem Schritt
- Main task + subtasks in one step
- Automatische Tag-Erstellung (inkl. Kategorie als Tag)
- Auto tag creation (including category as tag)
- Projekt-Zuweisung aus Quick Capture
- Project assignment from Quick Capture

**User Interface / Benutzeroberfläche:**
- Nahtlose Integration in Leantime Menü
- Seamless integration into Leantime menu
- Quick Capture: Schnell Notizen in Tasks umwandeln
- Quick Capture: Quickly convert notes to tasks
- AI Settings: Konfiguration für Ollama/OpenAI
- AI Settings: Ollama/OpenAI configuration
- Vorschau-Funktion: Tasks vor der Erstellung prüfen
- Preview function: Check tasks before creation
- Responsive Design: Leantime-native Templates
- Responsive design: Leantime-native templates

**Categories / Kategorien:**
- 8 Standard-Kategorien mit Icons und Farben
- 8 default categories with icons and colors
- Database-driven: Vollständig konfigurierbar
- Database-driven: Fully configurable
- Keyword-basierte Fallback-Erkennung
- Keyword-based fallback detection
- Emoji Icons für bessere Erkennbarkeit
- Emoji icons for better recognition

**AI Capabilities / AI-Fähigkeiten:**

**System-Prompt Example / System-Prompt Beispiel:**
```
Du bist ein intelligenter, effizienter Assistent für ein Unternehmen im Bereich 
Schilder, Glas und Befestigungstechnik. Deine Aufgabe ist es, aus kurzen, oft 
unstrukturierten Notizen (Anrufe, Mails, Zurufe) klare Aufgaben für das Task-
Management zu erstellen.
```

**Example Analysis / Beispiel-Analyse:**

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

**Architecture / Architektur:**
- Modularer Aufbau mit Dependency Injection
- Modular architecture with Dependency Injection
- Repository Pattern für Datenbankzugriff
- Repository pattern for database access
- Service-Layer für Business Logic
- Service layer for business logic
- Model-Layer für Datenstrukturen
- Model layer for data structures
- Event-driven Integration mit Leantime
- Event-driven integration with Leantime

**Code Quality / Code-Qualität:**
- PSR-12 Compliance
- Typ-Hints überall
- Type hints everywhere
- Error Handling mit Logging
- Error handling with logging
- Bilingual (Deutsch/Englisch)
- Bilingual (German/English)

**Dependencies / Abhängigkeiten:**
- Leantime 3.x+
- PHP 8.1+
- Composer (optional)
- Ollama oder OpenAI API Key
- Ollama or OpenAI API Key

**Database / Datenbank:**

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

**Localization / Lokalisierung:**
- Deutsch (de-DE)
- Englisch (en-US)
- 100% UI-Strings
- 100% Error Messages
- 100% Settings
- 100% Help Texts

**Bug Fixes / Fehlerbehebungen:**
- ✅ Leantime quickAddTicket() Return-Bug umgangen
- ✅ Tags separat via Repository gespeichert
- ✅ Kategorie als erstes Tag hinzugefügt
- ✅ Subtask-Erstellung ohne ID zurückgegeben (workaround)
- ✅ Ollama Docker Verbindung (host.docker.internal)

**Known Issues / Bekannte Probleme:**
- ⚠️ Leantime quickAddTicket() gibt bei Erfolg `true` statt ID zurück
- ⚠️ Tags werden nicht automatisch gespeichert (workaround implementiert)
- ⚠️ API-Timeouts bei großen Modellen (konfigurierbar)

### 📦 Documentation / Dokumentation
- ✅ README.md - Übersicht
- ✅ INSTALLATION.md - Installationsanleitung (German)
- ✅ LEANTIME_INTEGRATION.md - Technische Integration
- ✅ docs/ARCHITECTURE.md - System-Architektur
- ✅ docs/API.md - API-Dokumentation
- ✅ docs/USER_GUIDE.md - Benutzerhandbuch
- ✅ docs/DEVELOPER_GUIDE.md - Entwicklerhandbuch
- ✅ docs/CHANGELOG.md - Änderungsprotokoll
- ✅ docs/QUICK_REFERENCE.md - Schnelle Übersicht
- ✅ docs/CONTRIBUTING.md - M
