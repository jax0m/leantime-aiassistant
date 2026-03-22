# Leantime AI Assistant - Entwicklerhandbuch

## Einführung / Introduction

Dieses Handbuch dient Entwicklern, die Leantime AI Assistant erweitern, debuggen oder in andere Leantime-Plugins integrieren möchten.

This guide is for developers who want to extend, debug, or integrate Leantime AI Assistant into other Leantime plugins.

---

## Projektstruktur / Project Structure

```
AIAssistant/
├── Bootstrap.php              # Plugin-Bootstrap (nicht verwendet)
├── register.php               # Menü-Registrierung
├── composer.json              # Dependencies
├── README.md                  # README
├── CHANGELOG.md               # Änderungsprotokoll
├── INSTALLATION.md            # Installationsanleitung (DE)
├── LICENSE                    # MIT License
│
├── Controllers/               # HTTP-Controller
│   ├── QuickCapture.php       # Quick Capture UI
│   └── Settings.php           # Settings UI
│
├── Services/                  # Business Logic
│   ├── AIAssistant.php        # Haupt-Service (AI Kommunikation)
│   ├── TaskGenerator.php      # Task-Erstellung
│   └── CategoryManager.php    # Kategorien-Management
│
├── Repositories/              # Datenbankzugriff
│   ├── Settings.php           # Settings DB
│   └── Categories.php         # Categories DB
│
├── Models/                    # Datenstrukturen
│   ├── AIRequest.php          # AI-Anfragedaten
│   └── TaskStructure.php      # Task-Struktur (AI-Output)
│
├── Templates/                 # UI-Templates
│   ├── quickcapture.tpl.php   # Quick Capture View
│   └── settings.tpl.php       # Settings View
│
└── Language/                  # Lokalisierung
    ├── de-DE.ini              # Deutsch
    └── en-US.ini              # Englisch
```

---

## Entwicklungsumgebung / Development Environment

### Anforderungen / Requirements

- **PHP:** 8.1+
- **Composer:** Für Dependencies
- **Leantime:** 3.x
- **Git:** Für Versionskontrolle

### Setup / Setup

1. **Plugin kopieren:**
   ```bash
   cp -r AIAssistant /pfad/zu/leantime/app/Plugins/
   ```

2. **Composer installieren:**
   ```bash
   cd /pfad/zu/leantime
   composer install
   ```

3. **Plugin aktivieren:**
   - Admin login
   - Settings → Plugins → AIAssistant aktivieren

4. **Development-Modus:**
   ```bash
   # Xdebug aktivieren (optional)
   # Error logging in Leantime aktivieren
   ```

---

## Extension Points / Extension Points

### 1. Neue Kategorien hinzufügen / Add new categories

**Repository:** `Repositories/Categories.php`

**Beispiel: Kategorie "Support" hinzufügen:**

```php
// SQL-Eintrag
INSERT INTO zp_aiassistant_categories (
    name, 
    name_display, 
    icon, 
    color, 
    keywords, 
    is_default
) VALUES (
    'support', 
    'Support', 
    '🎧', 
    '#5bc0de', 
    'support,hilfe,frage,unterstützung', 
    0
);
```

**Service:** `Services/CategoryManager.php`

Die Kategorie wird automatisch geladen und gecacht.

### 2. AI-Prompt anpassen / Customize AI prompt

**Service:** `Services/AIAssistant.php`

**Methode:** `getDefaultSystemPrompt()`

**Beispiel: Prompt für Baufirma:**

```php
public function getDefaultSystemPrompt(): string
{
    return <<<PROMPT
Du bist ein Assistent für eine Baufirma. Erstelle Aufgaben für:
- Neue Aufträge
- Materialbestellungen
- Terminerinnerungen

KATEGORIEN:
- auftrag: Kunde möchte Arbeiten beauftragen
- material: Material muss bestellt werden
- termin: Termin vereinbaren/erinnern

Prioritäten:
- critical: Notfälle, Schäden
- high: Zeitkritisch
- normal: Standard-Aufgaben
- low: Keine Eile
PROMPT;
}
```

### 3. Neue Priority-Levels / New priority levels

**Model:** `Models/TaskStructure.php`

**Methode:** `mapPriority()`

**Beispiel:**

```php
private static function mapPriority(string $priority): int
{
    return match(strtolower($priority)) {
        'critical', 'dringend', 'urgent' => 1,
        'high', 'hoch' => 2,
        'medium', 'normal', 'mittel' => 3,
        'low', 'niedrig' => 4,
        'lowest', 'sehr niedrig' => 5,
        'backlog', 'warteschlange' => 6,  // Neue Priority
        default => 3
    };
}
```

**Hinweis:** Leantime standardmäßig nur 1-5 unterstützt. Priority 6+ wird als 5 behandelt.

### 4. Custom Tag-Format / Custom tag format

**Service:** `Services/TaskGenerator.php`

**Methode:** `saveTags()`

**Beispiel: Tags mit Projekt-Präfix:**

```php
private function saveTags(int $ticketId, array $tags, ?string $category = null): void
{
    // Projekt-Präfix hinzufügen
    $projectId = $this->getTicketProjectId($ticketId);
    $projectName = $this->getProjectName($projectId);
    
    $prefixedTags = array_map(function($tag) use ($projectName) {
        return "{$projectName}: {$tag}";
    }, $tags);
    
    // Weiter wie üblich...
    $this->ticketRepository->patchTicket($ticketId, ['tags' => implode(',', $prefixedTags)]);
}
```

### 5. Subtask-Logik anpassen / Customize subtask logic

**Service:** `Services/TaskGenerator.php`

**Methode:** `createSubtasks()`

**Beispiel: Nur bei bestimmten Kategorien Subtasks:**

```php
private function createSubtasks(int $parentTaskId, array $subtasks, int $projectId, int $userId): array
{
    // Nur bei komplexen Aufgaben Subtasks erstellen
    $task = $this->getTask($parentTaskId);
    if ($task->category !== 'einfache-aufgabe') {
        return [];
    }
    
    // Weiter wie üblich...
}
```

### 6. Event Listener hinzufügen / Add event listener

**Datei:** `register.php`

**Beispiel: Nach Task-Erstellung etwas tun:**

```php
use Leantime\Core\Events\EventDispatcher;

// Event nach Task-Erstellung
$dispatcher = new EventDispatcher();
$dispatcher->on('ticket.created', function($ticket) {
    // Custom Logik nach Task-Erstellung
    error_log("Custom: Ticket $ticket->id erstellt");
});
```

---

## Debugging / Debugging

### Logging / Logging

Alle Services verwenden `error_log()`:

```php
error_log("AIAssistant: Message");
error_log("AIAssistant: Error - " . $e->getMessage());
```

**Log-Pfad:**
- Linux: `/var/www/html/cache/logs/`
- Windows: `C:\xampp\tmp\`

### Debug-Modus aktivieren / Enable debug mode

**In Leantime Konfiguration:**
```php
// /var/www/html/app/Core/Framework.php
$debug = true;
```

### Xdebug / Xdebug

**Für PHP-Debugging:**

1. Xdebug installieren
2. Xdebug aktivieren in php.ini
3. Leantime für Localhost konfigurieren

**Breakpoints setzen:**
```php
// In Services/TaskGenerator.php
// Cursor hier setzen und "F9" drücken
$this->ticketService->quickAddTicket($values);
```

### Fehler simulieren / Simulate errors

**API-Test:**

```bash
# Ollama nicht erreichbar simulieren
curl http://localhost:11434/api/tags
# → {"error": "connection refused"}

# OpenAI Rate Limit
curl -H "Authorization: Bearer sk-invalid" https://api.openai.com/v1/models
# → 401 Unauthorized
```

---

## Testing / Testing

### Unit Tests / Unit Tests

**Empfohlene Struktur:**

```
tests/
├── Unit/
│   ├── TaskStructureTest.php
│   ├── CategoryManagerTest.php
│   └── AIAssistantTest.php
└── Integration/
    ├── QuickCaptureTest.php
    └── TaskGeneratorTest.php
```

**Beispiel: TaskStructureTest.php**

```php
<?php

namespace Tests\Unit;

use Leantime\Plugins\AIAssistant\Models\TaskStructure;

class TaskStructureTest extends TestCase
{
    public function testFromAIResponseValid()
    {
        $json = '{"title": "Test", "category": "anfrage", "priority": 3}';
        $structure = TaskStructure::fromAIResponse($json);
        
        $this->assertEquals('Test', $structure->title);
        $this->assertEquals('anfrage', $structure->category);
        $this->assertEquals(3, $structure->priority);
    }
    
    public function testFromAIResponseInvalid()
    {
        $json = 'invalid json';
        $structure = TaskStructure::fromAIResponse($json);
        
        $this->assertNull($structure);
    }
}
```

### Integration Tests / Integration Tests

**QuickCapture Test:**

```bash
# Test Quick Capture endpoint
curl -X POST http://leantime/AIAssistant/quickCapture \
  -H "Content-Type: application/json" \
  -d '{
    "note": "Test notiz",
    "project_id": 1
  }'

# Erwartet: { "success": true, "mainTaskId": 123 }
```

### Manuelle Tests / Manual tests

1. **Quick Capture Test:**
   - Öffne Quick Capture
   - Gib Test-Notiz ein
   - Prüfe AI-Ausgabe
   - Prüfe Task-Erstellung

2. **Settings Test:**
   - Öffne AI Settings
   - Konfiguriere Ollama
   - Teste Verbindung
   - Lade Modelle

3. **Categories Test:**
   - Öffne Kategorien-Verwaltung
   - Füge neue Kategorie hinzu
   - Teste AI mit Kategorie-Keywords

---

## Best Practices / Best Practices

### 1. Dependency Injection / Dependency Injection

```php
// ✅ Gut / Good
class TaskGenerator {
    private TicketService $ticketService;
    
    public function __construct(TicketService $ticketService) {
        $this->ticketService = $ticketService;
    }
}

// ❌ Schlecht / Bad
class TaskGenerator {
    public function createTask() {
        $service = new TicketService(); // ❌ Hardcoded
    }
}
```

### 2. Repository Pattern / Repository Pattern

```php
// ✅ Gut / Good
$settings = $this->settingsRepo->getAllSettings();

// ❌ Schlecht / Bad
$results = $this->pdo->query("SELECT * FROM zp_aiassistant_settings");
```

### 3. Error Handling / Error Handling

```php
// ✅ Gut / Good
try {
    $result = $this->apiCall($url);
    return $result;
} catch (\Exception $e) {
    error_log("API Error: " . $e->getMessage());
    return null; // Graceful degradation
}

// ❌ Schlecht / Bad
$result = $this->apiCall($url);
return $result; // ❌ Keine Error-Handling
```

### 4. Type Safety / Type Safety

```php
// ✅ Gut / Good
public function analyzeText(string $text): ?string {}

// ❌ Schlecht / Bad
public function analyzeText($text) {}
```

### 5. Bilingual Support / Bilingual Support

```php
// ✅ Gut / Good
$this->language->t('aiassistant.messages.success.saved');

// ❌ Schlecht / Bad
$message = "Settings saved successfully"; // ❌ Harcoded
```

---

## Troubleshooting für Entwickler / Developer troubleshooting

### AIAssistant nicht reagiert

**Checkliste:**
1. Fehlerlog prüfen: `/cache/logs/`
2. PHP-FPM/CLI Logs prüfen
3. Xdebug deaktiviert?
4. Service geladen? (Dependency Injection)

### Task nicht erstellt

**Checkliste:**
1. `error_log()` prüfen
2. `TaskStructure::isValid()` zurück
3. `TicketService::quickAddTicket()` funktioniert
4. ProjectID gültig?

### Kategorien nicht geladen

**Checkliste:**
1. DB-Einträge prüfen: `SELECT * FROM zp_aiassistant_categories`
2. Cache löschen
3. CategoryManager-Constructor korrekt?

### Performance-Probleme

**Checkliste:**
1. Kategorien-Cache prüfen
2. API-Timeouts zu niedrig?
3. Datenbank-Abfragen optimiert?

---

## Extension Ideas / Extension Ideen

### 1. Multi-Project Support

Aktuell: Ein Task pro AI-Aufruf

Idee: Mehrere Tasks aus einer Notiz

```php
// AI-Antwort
{
    "tasks": [
        {"title": "Task 1", "category": "a"},
        {"title": "Task 2", "category": "b"}
    ]
}
```

### 2. AI-Prompt als JSON Schema

```php
public function getDefaultSystemPrompt(): string
{
    return json_encode([
        "role": "system",
        "instructions": "...",
        "categories": [...]
    ]);
}
```

### 3. Batch Processing

Mehrfach-Notizen gleichzeitig verarbeiten

```php
public function analyzeMultiple(array $notes): array
{
    // Parallel API-Aufrufe
    $results = [];
    foreach ($notes as $note) {
        $results[] = $this->analyzeText($note);
    }
    return $results;
}
```

### 4. Custom AI Models

Benutzerdefinierte Modelle registrieren

```php
use Leantime\Plugins\AIAssistant\Services\AIAssistant;

class CustomAIAssistant extends AIAssistant
{
    public function analyzeWithCustomModel($text) {
        // Custom logic
    }
}
```

---

## Version / Version

- **Version:** 1.0.0
- **Kompatibilität:** Leantime 3.x, PHP 8.1+
- **Status:** Production Ready

---

## Beiträge / Contributing

1. Fork das Repository
2. Feature-Branch erstellen (`git checkout -b feature/AIthing`)
3. Commit (`git commit -m 'Add amazing AI feature'`)
4. Branch pushen (`git push origin feature/AIthing`)
5. Pull Request öffnen

### Code Style / Code Style

- PSR-12 für PHP
- 4 spaces for indentation
- One function per method
- Single Responsibility Principle

### Commit Messages / Commit Messages

```
feat: Add support for custom categories
fix: Resolve AI timeout issue
docs: Update README with installation steps
refactor: Extract TaskGenerator logic
```

### Pull Request Checklist / Pull Request Checklist

- [ ] Tests hinzugefügt / Tests added
- [ ] Dokumentation aktualisiert / Documentation updated
- [ ] Bilingual (DE/EN) / Bilingual
- [ ] Type hints verwendet / Type hints used
- [ ] Error handling implementiert / Error handling implemented
- [ ] Keine Hardcoded Strings / No hardcoded strings

---

## License / Lizenz

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
