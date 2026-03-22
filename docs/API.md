# Leantime AI Assistant - API Documentation

## REST API Endpoints

Dieses Plugin nutzt keine traditionellen REST-Endpoints, sondern integriert sich direkt in das Leantime Router-System. Die Endpunkte werden automatisch über `register.php` registriert.

This plugin uses no traditional REST endpoints but integrates directly into the Leantime Router system. Endpoints are automatically registered via `register.php`.

---

## Registerierte Endpunkte / Registered Endpoints

| Endpoint | Method | Beschreibung / Description | Berechtigung / Permission |
|----------|--------|---------------------------|--------------------------|
| `/AIAssistant/quickCapture` | GET/POST | Quick Capture Interface | Alle Benutzer / All users |
| `/AIAssistant/settings` | GET/POST | AI Settings Interface | Administrator / Administrator |

---

## Quick Capture API

### GET /AIAssistant/quickCapture

**Zweck:** Lädt das Quick Capture Interface  
**Purpose:** Loads the Quick Capture interface

**Parameter / Parameters:**
- `project_id` (int, optional) - Voreingestelltes Projekt / Default project

**Response / Antwort:**
- HTML-Template mit Quick Capture Formular
- HTML template with Quick Capture form

### POST /AIAssistant/quickCapture

**Zweck:** Analysiert Notiz und erstellt Tasks  
**Purpose:** Analyzes note and creates tasks

**Request Body / Request Body:**
```json
{
    "note": "Kunde Müller möchte 50 Schrauben bestellen. Erst Lagerbestand prüfen, dann Angebot erstellen.",
    "project_id": 1
}
```

**Response / Antwort:**
```json
{
    "success": true,
    "mainTaskId": 123,
    "subtaskIds": [456, 789],
    "message": "Tasks created successfully",
    "preview": {
        "title": "Bestellung Hr. Müller für 50 Schrauben",
        "description": "Kunde möchte 50 Schrauben bestellen...",
        "category": "kundenbestellung",
        "categoryName": "Bestellung",
        "categoryIcon": "📦",
        "priority": 3,
        "priorityLabel": "Medium",
        "deadline": null,
        "subtasks": ["Lagerbestand prüfen", "Angebot erstellen"],
        "tags": ["📦 Bestellung", "Müller"]
    }
}
```

---

## Settings API

### GET /AIAssistant/settings

**Zweck:** Lädt AI-Einstellungen  
**Purpose:** Loads AI settings

**Response / Antwort:**
```json
{
    "settings": {
        "provider": "ollama",
        "ollama_url": "http://localhost:11434",
        "ollama_model": "llama3.1",
        "timeout": 60,
        "system_prompt": "Du bist ein intelligenter...",
        "openai_api_key": "sk-...",
        "openai_base_url": "https://api.openai.com/v1",
        "openai_model": "gpt-4"
    },
    "ollama_models": ["llama3.1", "mistral"],
    "openai_models": ["gpt-4", "gpt-3.5-turbo"]
}
```

### POST /AIAssistant/settings

**Zweck:** Speichert AI-Einstellungen  
**Purpose:** Saves AI settings

**Request Body / Request Body:**
```json
{
    "provider": "openai",
    "openai_api_key": "sk-new-key-...",
    "openai_base_url": "https://api.openai.com/v1",
    "openai_model": "gpt-4-turbo",
    "timeout": 90,
    "system_prompt": "Custom system prompt..."
}
```

**Response / Antwort:**
```json
{
    "success": true,
    "message": "Settings saved successfully",
    "connection_test": true
}
```

---

## AI Service API (Internal)

### AIAssistant::analyzeText()

**Zweck:** Analysiert Text mit konfiguriertem AI-Provider  
**Purpose:** Analyzes text with configured AI provider

**Parameter / Parameters:**
- `text` (string) - Text zum Analysieren / Text to analyze

**Response / Antwort:**
```json
{
    "title": "Bestellung Hr. Müller",
    "description": "Kunde möchte 50 Schrauben bestellen...",
    "category": "kundenbestellung",
    "priority": 3,
    "deadline": "2026-03-25",
    "subtasks": ["Lagerbestand prüfen", "Angebot erstellen"],
    "tags": ["Müller", "50 Schrauben"]
}
```

---

### AIAssistant::getOllamaModels()

**Zweck:** HOLT verfügbare Ollama-Modelle  
**Purpose:** Gets available Ollama models

**Parameter / Parameters:**
- `url` (string) - Ollama Base URL (optional, aus Settings)

**Response / Antwort:**
```json
{
    "models": ["llama3.1", "mistral", "gemma"]
}
```

---

### AIAssistant::getOpenAIModels()

**Zweck:** HOLT verfügbare OpenAI-Modelle  
**Purpose:** Gets available OpenAI models

**Parameter / Parameters:**
- `apiKey` (string) - OpenAI API Key
- `baseUrl` (string) - OpenAI Base URL

**Response / Antwort:**
```json
{
    "models": ["gpt-4", "gpt-4-turbo", "gpt-3.5-turbo"]
}
```

---

### AIAssistant::testOllamaConnection()

**Zweck:** Testet Ollama-Verbindung  
**Purpose:** Tests Ollama connection

**Parameter / Parameters:**
- `url` (string) - Ollama Base URL
- `model` (string) - Modell-Name / Model name

**Response / Antwort:**
```json
{
    "success": true,
    "message": "Connection successful"
}
```

---

### AIAssistant::testOpenAIConnection()

**Zweck:** Testet OpenAI-Verbindung  
**Purpose:** Tests OpenAI connection

**Parameter / Parameters:**
- `apiKey` (string) - OpenAI API Key
- `baseUrl` (string) - OpenAI Base URL

**Response / Antwort:**
```json
{
    "success": true,
    "message": "Connection successful"
}
```

---

### TaskGenerator::createTaskFromAI()

**Zweck:** Erstellt Leantime-Tasks aus AI-Antwort  
**Purpose:** Creates Leantime tasks from AI response

**Parameter / Parameters:**
- `aiResponse` (string) - JSON-Antwort von AI
- `projectId` (int) - Projekt-ID
- `userId` (int) - User-ID der Task-Ersteller

**Response / Antwort:**
```json
{
    "success": true,
    "mainTaskId": 123,
    "subtaskIds": [456, 789],
    "message": "Tasks created successfully"
}
```

---

### TaskGenerator::getTaskPreview()

**Zweck:** HOLT Task-Vorschau ohne Erstellung  
**Purpose:** Gets task preview without creation

**Parameter / Parameters:**
- `aiResponse` (string) - JSON-Antwort von AI

**Response / Antwort:**
```json
{
    "title": "Bestellung Hr. Müller",
    "description": "Kunde möchte 50 Schrauben bestellen...",
    "category": "kundenbestellung",
    "categoryName": "Bestellung",
    "categoryIcon": "📦",
    "categoryColor": "#3498db",
    "priority": 3,
    "priorityLabel": "Medium",
    "deadline": null,
    "subtasks": ["Lagerbestand prüfen", "Angebot erstellen"],
    "tags": ["📦 Bestellung", "Müller"]
}
```

---

## Fehlercodes / Error Codes

| Code / Code | Beschreibung / Description |
|-------------|---------------------------|
| `0` | Success / Erfolg |
| `1` | Invalid AI response format / Ungültiges AI-Antwort-Format |
| `2` | Failed to create main task / Task-Erstellung fehlgeschlagen |
| `3` | Connection failed / Verbindung fehlgeschlagen |
| `4` | No models available / Keine Modelle verfügbar |
| `5` | Invalid configuration / Ungültige Konfiguration |

---

## Beispiel-Aufrufe / Example Calls

### Quick Capture mit Ollama

```bash
curl -X POST http://leantime/AIAssistant/quickCapture \
  -H "Content-Type: application/json" \
  -d '{
    "note": "Kunde Müller möchte 50 Schrauben bestellen"
  }'
```

### Settings Update

```bash
curl -X POST http://leantime/AIAssistant/settings \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "openai",
    "openai_api_key": "sk-...",
    "openai_base_url": "https://api.openai.com/v1",
    "openai_model": "gpt-4"
  }'
```

---

## Rate Limiting / Rate Limiting

- **Ollama:** Abhängig von lokaler Konfiguration (keine Limits)
- **OpenAI:** Abhängig vom API-Plan (Standard: ~3,000 Token/Min)
- **Empfehlung:** Timeout 60-120 Sekunden für große Modelle

---

## Version / Version

- **Version:** 1.0.0
- **Kompatibilität:** Leantime 3.x
