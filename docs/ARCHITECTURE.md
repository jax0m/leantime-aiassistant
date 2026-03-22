# Leantime AI Assistant - Architecture Documentation

## Systemüberblick / System Overview

Leantime AI Assistant ist ein Plugin, das freitextbasierte Notizen automatisch in strukturierte Leantime-Tasks umwandelt. Das Plugin verwendet zwei AI-Provider (Ollama oder OpenAI) und integriert nahtlos in das Leantime-Ökosystem.

Leantime AI Assistant is a plugin that automatically converts free-text notes into structured Leantime tasks. The plugin uses two AI providers (Ollama or OpenAI) and integrates seamlessly into the Leantime ecosystem.

---

## Architekturdiagramm / Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                    Leantime AI Assistant                        │
├─────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │   Controllers│  │   Templates  │  │      Services        │  │
│  ├──────────────┤  ├──────────────┤  ├──────────────────────┤  │
│  │ QuickCapture │  │ quickcapture │  │  ┌────────────────┐  │  │
│  │ Settings     │  │ settings     │  │  │ AIAssistant    │  │  │
│  └──────┬───────┘  └──────┬───────┘  │  │  - Ollama API  │  │  │
│         │                 │         │  │  - OpenAI API   │  │  │
│         └────────┬────────┘         │  │  - Model Mgmt   │  │  │
│                  │                  │  │  - Connection   │  │  │
│                  ▼                  │  └────────┬────────┘  │  │
│  ┌──────────────────────────────────┘          │           │  │
│  │         Repositories                 │           │  │
│  ├─────────────────────────────────────┤           │  │
│  │  - Settings (DB)                    │           │  │
│  │  - Categories (DB)                  │           │  │
│  └─────────────────────────────────────┘           │  │
│                                                     │  │
│  ┌─────────────────────────────────────────────┐   │  │
│  │             Models                         │   │  │
│  │  - TaskStructure (AI Output)               │   │  │
│  │  - AIRequest                               │   │  │
│  └─────────────────────────────────────────────┘   │  │
│                                                     │  │
│  ┌─────────────────────────────────────────────┐   │  │
│  │         External Services (via DI)         │   │  │
│  │  - Leantime\Tickets\Services\Tickets       │   │  │
│  │  - Leantime\Tickets\Repositories\Tickets   │   │  │
│  │  - Leantime\Core\Language                  │   │  │
│  └─────────────────────────────────────────────┘   │  │
└─────────────────────────────────────────────────────┘
```

---

## Komponenten / Components

### 1. Controllers / Controller

**Zweck:** Handhabt HTTP-Anfragen und rendert UI-Views  
**Purpose:** Handles HTTP requests and renders UI views

#### QuickCapture.php
- **Route:** `/AIAssistant/quickCapture`
- **Funktionen:**
  - Zeigt Quick Capture Interface (Notizeingabe, Projektwahl)
  - Sendet Notizen an AIAssistant Service
  - Zeigt AI-Vorschau an
  - Erstellt Tasks basierend auf AI-Ausgabe
- **Functions:**
  - Display Quick Capture interface (note input, project selection)
  - Send notes to AIAssistant Service
  - Display AI preview
  - Create tasks based on AI output

#### Settings.php
- **Route:** `/AIAssistant/settings`
- **Funktionen:**
  - Lädt AI Provider Einstellungen aus DB
  - Zeigt Ollama/OpenAI Konfiguration
  - Lädt verfügbare Modelle (Ollama/OpenAI)
  - Testet Verbindung
  - Speichert Einstellungen
- **Functions:**
  - Load AI provider settings from DB
  - Display Ollama/OpenAI configuration
  - Load available models (Ollama/OpenAI)
  - Test connection
  - Save settings

---

### 2. Services / Services

#### AIAssistant.php (Haupt-Service / Main Service)
**Verantwortlichkeiten:**
- Kommunikation mit Ollama API
- Kommunikation mit OpenAI API
- Modell-Management
- Verbindungstests
- System-Prompt-Management

**Responsibilities:**
- Ollama API communication
- OpenAI API communication
- Model management
- Connection tests
- System prompt management

**Wichtige Methoden / Important Methods:**
- `getOllamaModels()` - HOLT verfügbare Ollama-Modelle
- `getOpenAIModels()` - HOLT verfügbare OpenAI-Modelle
- `testOllamaConnection()` - Testet Ollama-Verbindung
- `testOpenAIConnection()` - Testet OpenAI-Verbindung
- `analyzeText()` - ANALYSIERT Text mit konfiguriertem Provider
- `getSystemPrompt()` - HOLT/ersetzt System-Prompt

#### TaskGenerator.php (Task-Generator)
**Verantwortlichkeiten:**
- Wandelt AI-JSON in Leantime-Tasks um
- Erstellt Haupttasks
- Erstellt Subtasks
- Speichert Tags (inklusive Kategorie als Tag)
- Handhabt Leantime quickAddTicket()-Bug

**Responsibilities:**
- Converts AI JSON to Leantime tasks
- Creates main tasks
- Creates subtasks
- Saves tags (including category as tag)
- Handles Leantime quickAddTicket() bug

**Wichtige Methoden / Important Methods:**
- `createTaskFromAI()` - ERSTELLT Task aus AI-Antwort
- `createMainTask()` - Erstellt Haupttask
- `createSubtasks()` - Erstellt Subtasks
- `saveTags()` - Speichert Tags (BUGFIX: Kategorie als Tag)
- `getTaskPreview()` - HOLT Vorschau ohne Task-Erstellung
- `getLastCreatedTicketId()` - Workaround für Leantime-Bug

#### CategoryManager.php (Kategorien-Manager)
**Verantwortlichkeiten:**
- Lädt Kategorien aus DB (mit Icons/Colors)
- Caching für Performance
- Übersetzung von Kategorienamen
- Keyword-basierte Kategorie-Erkennung

**Responsibilities:**
- Loads categories from DB (with icons/colors)
- Caching for performance
- Translates category names
- Keyword-based category detection

**Wichtige Methoden / Important Methods:**
- `getAllCategories()` - HOLT alle Kategorien
- `getCategory()` - HOLT Kategorie-Details
- `getCategoryIcon()` - HOLT Kategorie-Icon (Emoji/FontAwesome)
- `getCategoryColor()` - HOLT Kategorie-Farbe
- `getCategoryName()` - HOLT Übersetzten Kategorienamen
- `detectCategory()` - ERKENNT Kategorie aus Text (Fallback)
- `isValidCategory()` - PRÜFT Kategorie-Gültigkeit

---

### 3. Repositories / Repositories

**Zweck:** Datenbankschnittstellen (nicht für externe Services)  
**Purpose:** Database interfaces (not for external services)

#### Settings.php
- **SQL-Tabelle:** `zp_aiassistant_settings`
- **Methoden:**
  - `getAllSettings()` - HOLT alle Einstellungen
  - `getSetting()` - HOLT einzelne Einstellung
  - `saveSetting()` - SPEICHERT Einstellung
  - `deleteSetting()` - LÖSCHT Einstellung

#### Categories.php
- **SQL-Tabelle:** `zp_aiassistant_categories`
- **Methoden:**
  - `getAllCategories()` - HOLT alle Kategorien
  - `getCategory()` - HOLT einzelne Kategorie
  - `saveCategory()` - SPEICHERT Kategorie
  - `deleteCategory()` - LÖSCHT Kategorie

---

### 4. Models / Models

#### TaskStructure.php
**Zweck:** Repräsentiert die von AI extrahierten Task-Daten  
**Purpose:** Represents task data extracted by AI

**Eigenschaften / Properties:**
- `title` (string) - Task-Titel
- `description` (string) - Task-Beschreibung
- `category` (string) - Kategorie-Schlüssel (z.B. "kundenbestellung")
- `priority` (int) - Priorität (1-5, 1=Critical)
- `deadline` (string|null) - Deadline (YYYY-MM-DD oder null)
- `subtasks` (array) - Array von Subtask-Texten
- `tags` (array) - Array von Tags
- `projectId` (int) - Projekt-ID

**Wichtige Methoden / Important Methods:**
- `fromAIResponse()` - KREATIERT TaskStructure aus AI-JSON
- `mapPriority()` - KONVERTIERT Prioritäts-String zu Integer
- `parseDeadline()` - PARST Deadline-String zu Datum
- `toArray()` - KONVERTIERT zu Array für Task-Erstellung
- `isValid()` - PRÜFT Task-Gültigkeit

#### AIRequest.php
**Zweck:** Hält AI-Anfrage-Daten  
**Purpose:** Holds AI request data

---

## Datenfluss / Data Flow

### 1. Quick Capture Workflow / Quick Capture Workflow

```
┌────────────────────────────────────────────────────────────────────┐
│  1. User öffnet Quick Capture UI                                   │
│     User opens Quick Capture UI                                    │
├────────────────────────────────────────────────────────────────────┤
│  2. User gibt Notiz ein, wählt Projekt                    
