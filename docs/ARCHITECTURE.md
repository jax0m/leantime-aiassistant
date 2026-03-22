# Leantime AI Assistant - Architecture Overview

## System Overview / Systemüberblick

Leantime AI Assistant ist ein Plugin, das freitextbasierte Notizen automatisch in strukturierte Leantime-Tasks umwandelt. Das Plugin verwendet zwei AI-Provider (Ollama oder OpenAI) und integriert nahtlos in das Leantime-Ökosystem.

Leantime AI Assistant is a plugin that automatically converts free-text notes into structured Leantime tasks. The plugin uses two AI providers (Ollama or OpenAI) and integrates seamlessly into the Leantime ecosystem.

---

## Architecture Diagram / Architekturdiagramm

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
│                  ▼                  │  │  - System Prompt│  │  │
│  ┌──────────────────────────────────┘          ▼           │  │
│  │         Repositories                 │  ┌──────────────┐│  │
│  ├─────────────────────────────────────┤  │ TaskGenerator││  │
│  │  - Settings (DB)                    │  │ CategoryMgmt ││  │
│  │  - Categories (DB)                  │  └──────────────┘│  │
│  └─────────────────────────────────────┘                   │  │
│                                                            │  │
│  ┌─────────────────────────────────────────────┐          │  │
│  │             Models                         │          │  │
│  │  - TaskStructure (AI Output JSON)          │          │  │
│  │  - AIRequest                               │          │  │
│  └─────────────────────────────────────────────┘          │  │
│                                                            │  │
│  ┌─────────────────────────────────────────────┐          │  │
│  │         External (Leantime Core)           │          │  │
│  │  - Tickets Service/Repository              │          │  │
│  │  - Language Service                        │          │  │
│  └─────────────────────────────────────────────┘          │  │
└─────────────────────────────────────────────────────────────┘
```

---

## Component Map / Komponentenübersicht

### 1. Controllers (HTTP Entry Points)

#### QuickCapture Controller
| Method | Purpose | Description |
|--------|---------|-------------|
| `get()` | Display UI | Shows Quick Capture interface with note input and project selection |
| `analyze()` | AJAX | Sends note to AI, returns preview JSON |
| `createTasks()` | AJAX | Creates tasks from AI response |

**Routes:**
- `/AIAssistant/quickCapture` - Main UI
- `/AIAssistant/quickCapture/analyze` - AJAX
- `/AIAssistant/quickCapture/createTasks` - AJAX

#### Settings Controller
| Method | Purpose | Description |
|--------|---------|-------------|
| `get()` | Display UI | Shows AI settings page with provider config |
| `post()` | AJAX/Save | Handles AJAX actions and saves settings |

**Routes:**
- `/AIAssistant/settings` - Settings UI
- `/AIAssistant/settings/loadModels` - AJAX
- `/AIAssistant/settings/loadOpenAIModels` - AJAX
- `/AIAssistant/settings/testConnection` - AJAX

---

### 2. Services (Business Logic)

#### AIAssistant Service
**Purpose:** Communicates with AI providers and manages AI workflow

| Method | Purpose | Description |
|--------|---------|-------------|
| `getOllamaModels()` | Get models | Fetches available Ollama models |
| `getOpenAIModels()` | Get models | Fetches available OpenAI models |
| `testOllamaConnection()` | Test | Tests Ollama API connectivity |
| `testOpenAIConnection()` | Test | Tests OpenAI API connectivity |
| `analyzeText()` | Main | Analyzes text with configured provider |
| `getSystemPrompt()` | Internal | Gets/updates system prompt |
| `getDefaultSystemPrompt()` | Internal | Returns default industry-specific prompt |

**Key Features:**
- Dual provider support (Ollama/OpenAI)
- System prompt with dynamic date replacement (`{{CURRENT_DATE}}`)
- Industry-specific prompt for signage/glass/fastening industry
- Comprehensive error logging

#### TaskGenerator Service
**Purpose:** Converts AI JSON to Leantime tickets

| Method | Purpose | Description |
|--------|---------|-------------|
| `createTaskFromAI()` | Main | Creates main task and subtasks |
| `createMainTask()` | Internal | Creates main ticket via Leantime API |
| `createSubtasks()` | Internal | Creates subtasks linked to main task |
| `saveTags()` | Internal | Saves tags (includes category as first tag) |
| `getTaskPreview()` | Internal | Generates preview without creation |
| `getLastCreatedTicketId()` | Internal | Workaround for Leantime API bug |

**Key Features:**
- Leantime API bug workaround (quickAddTicket returns bool instead of ID)
- Category saved as first tag for filtering
- Logging of all Task creation steps

#### CategoryManager Service
**Purpose:** Manages task categories with icons and colors

| Method | Purpose | Description |
|--------|---------|-------------|
| `getAllCategories()` | Get | Loads all categories from DB |
| `getCategory()` | Get | Gets single category details |
| `detectCategory()` | Fallback | Keyword-based category detection |

**Key Features:**
- Database-driven categories
- Icon/Emoji support
- Color coding
- Translation support

---

### 3. Repositories (Data Access)

#### Settings Repository
- **Table:** `zp_aiassistant_settings`
- **Methods:** `getAllSettings()`, `getSetting()`, `saveSettings()`, `installIfNeeded()`

#### Categories Repository
- **Table:** `zp_aiassistant_categories`
- **Methods:** `getAllCategories()`, `getCategory()`, `saveCategory()`, `deleteCategory()`

---

### 4. Models (Data Structures)

#### TaskStructure
| Property | Type | Description |
|----------|------|-------------|
| `title` | string | Task title |
| `description` | string | Task description |
| `category` | string | Category key (e.g., "kundenbestellung") |
| `priority` | int | 1-5 (1=Critical) |
| `deadline` | string|null | YYYY-MM-DD or null |
| `subtasks` | array | Array of subtask texts |
| `tags` | array | Array of tag strings |
| `projectId` | int | Project ID |

**Methods:**
- `fromAIResponse()` - Parse AI JSON
- `isValid()` - Validate structure
- `toArray()` - Convert for task creation

---

### 5. Data Flow / Datenfluss

#### Quick Capture Flow
```
User Input → QuickCapture Controller → AIAssistant.analyzeText() 
→ AI API Call → AI Response JSON → TaskGenerator.getTaskPreview() 
→ User Review/Edit → QuickCapture.createTasks() 
→ TaskGenerator.createTaskFromAI() → Leantime Tickets → Tasks Created
```

#### Settings Flow
```
User Settings → Settings Controller → Settings Repository (DB) 
→ Test Connection → AIAssistant.getModels() → Model List → UI Display
```

---

## Integration Points

### Leantime Core Dependencies
- `Leantime\Domain\Tickets\Services\Tickets` - Ticket creation
- `Leantime\Domain\Tickets\Repositories\Tickets` - Ticket data access
- `Leantime\Core\Language` - Translation support
- `Leantime\Core\UI\Template` - UI rendering

### Database Tables
- `zp_aiassistant_settings` - AI configuration (provider, URLs, keys)
- `zp_aiassistant_categories` - Task categories with icons/colors

---

## Version / Version

- **Version:** 1.1.1
- **Compatibility:** Leantime 3.x, PHP 8.1+
- **Status:** Production Ready
