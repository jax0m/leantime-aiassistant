# Contributing to Leantime AI Assistant

Vielen Dank, dass Sie beitragen möchten! 🎉  
Thank you for contributing!

---

## Wie Sie beitragen können / How to contribute

### Reportieren Sie Bugs / Report bugs

1. **Check existing issues:** https://github.com/samir-brkic/leantime-aiassistant/issues
2. **Create new issue** mit folgenden Details:
   - Beschreibung des Problems / Problem description
   - Schritte zur Reproduktion / Steps to reproduce
   - Erwartetes vs. tatsächliches Verhalten / Expected vs actual behavior
   - Umgebungsdaten / Environment details:
     - Leantime Version
     - PHP Version
     - AI Provider (Ollama/OpenAI)
     - Modell / Model

### Vorschläge für neue Features / Feature suggestions

1. **Check existing discussions:** https://github.com/samir-brkic/leantime-aiassistant/discussions
2. **Create issue** mit:
   - Problem, das gelöst wird / Problem being solved
   - Vorschlag für Lösung / Proposed solution
   - Warum wichtig / Why important
   - Beispiele / Examples (falls zutreffend)

### Code-Beiträge / Code contributions

#### Fork und Branch erstellen / Fork and create branch

```bash
# Repository forken
git clone https://github.com/samir-brkic/leantime-aiassistant.git
cd leantime-aiassistant

# Remote hinzufügen
git remote add upstream https://github.com/samir-brkic/leantime-aiassistant.git

# Feature-Branch erstellen
git checkout -b feature/ihre-feature
```

#### Code schreiben / Write code

Folgen Sie den Best Practices:

Follow the best practices:

- ✅ PSR-12 für PHP
- ✅ Typ-Hints überall
- ✅ Dependency Injection via Constructor
- ✅ Error Handling mit Logging
- ✅ Bilingual (DE/EN) für alle Benutzer-Strings
- ✅ Kommentare für komplexe Logik

```php
<?php

namespace Leantime\Plugins\AIAssistant\Services;

use Leantime\Plugins\AIAssistant\Repositories\Settings;

/**
 * Custom Service Class
 */
class MyService
{
    private Settings $settings;

    public function __construct(Settings $settings)
    {
        $this->settings = $settings;
    }

    /**
     * Method with proper documentation
     * 
     * @param string $param
     * @return string
     */
    public function doSomething(string $param): string
    {
        try {
            // Code here
            return $result;
        } catch (\Exception $e) {
            error_log("MyService: " . $e->getMessage());
            return 'default';
        }
    }
}
```

#### Tests schreiben / Write tests

```php
<?php

namespace Tests\Unit\Services;

use Leantime\Plugins\AIAssistant\Services\MyService;
use PHPUnit\Framework\TestCase;

class MyServiceTest extends TestCase
{
    public function testDoSomething()
    {
        $service = new MyService($this->createMock(Settings::class));
        $result = $service->doSomething('test');
        
        $this->assertEquals('expected', $result);
    }
}
```

#### Commit mit gutem Message / Commit with good message

```bash
# Commit
git add .
git commit -m "feat: Add custom service for X

- Implement Y functionality
- Add Z test coverage
- Update documentation"
```

Commit Message Format:
- `feat:` Neue Features
- `fix:` Bugfixes
- `docs:` Dokumentation
- `refactor:` Code-Umstrukturierung
- `test:` Tests
- `chore:` Andere Änderungen

#### Pull Request öffnen / Open Pull Request

1. **Branch pushen:**
   ```bash
   git push origin feature/ihre-feature
   ```

2. **Pull Request auf GitHub öffnen:**
   - Titel klar und deskriptiv
   - Beschreibung mit:
     - Was wurde geändert / What was changed
     - Warum / Why
     - Screenshots (bei UI-Änderungen)
     - Checkliste

PR Checklist:
- [ ] Tests hinzugefügt / Tests added
- [ ] Tests durchlaufen / Tests pass
- [ ] Dokumentation aktualisiert / Documentation updated
- [ ] Bilingual (DE/EN) / Bilingual
- [ ] Keine Hardcoded Strings / No hardcoded strings
- [ ] Type hints verwendet / Type hints used
- [ ] Error handling implementiert / Error handling implemented
- [ ] Keine Secrets / No secrets
- [ ] Code Review durchlaufen / Code review passed

---

## Review Guidelines / Review Guidelines

### Was wir prüfen / What we check

- **Funktionalität:** Funktioniert es? / Does it work?
- **Code Quality:** PSR-12, Typ-Hints, etc.
- **Tests:** Abgedeckt? / Covered?
- **Dokumentation:** Aktualisiert? / Updated?
- **Bilingual:** DE/EN Strings?
- **Security:** Keine Sicherheitslücken / No security issues
- **Performance:** Keine Performance-Probleme

### Fragen, die beantwortet werden müssen / Questions to answer

- Löst das Problem / Does it solve the problem?
- Ist es notwendig / Is it necessary?
- Gibt es bessere Lösungen / Are there better solutions?
- Ist es backward-compatible?
- Wurden Tests hinzugefügt?

---

## Bilingual Support / Bilingual Support

### Lokalisierung / Localization

**Alle Benutzer-Strings müssen lokalisiert sein:**

```php
// ✅ Gut / Good
$this->language->t('aiassistant.messages.success.saved');

// ❌ Schlecht / Bad
$message = "Settings saved successfully";
```

### Neue Strings hinzufügen / Add new strings

1. **Language/de-DE.ini:**
   ```ini
   ; New string
   aiassistant.mynewstring = "Neuer String"
   ```

2. **Language/en-US.ini:**
   ```ini
   ; New string
   aiassistant.mynewstring = "New String"
   ```

**Naming convention:**
- Präfix: `aiassistant.`
- Kleinschreibung / lowercase
- Bindestriche für Wörter / hyphens for words
- Klare, deskriptive Namen / Clear, descriptive names

---

## Dokumentation / Documentation

### Dokumentations-Verzeichnis / Documentation folder

```
docs/
├── ARCHITECTURE.md   # System-Architektur
├── API.md            # API-Dokumentation
├── USER_GUIDE.md     # Benutzerhandbuch
├── DEVELOPER_GUIDE.md # Entwicklerhandbuch
├── CHANGELOG.md      # Änderungsprotokoll
├── QUICK_REFERENCE.md # Schnelle Übersicht
└── CONTRIBUTING.md   # Dieses Dokument
```

### Dokumentations-Standard / Documentation standard

- ✅ Bilingual (DE/EN)
- ✅ Markdown Format
- ✅ Clear headings mit Emojis
- ✅ Code examples
- ✅ Screenshots bei UI-Änderungen

---

## Code of Conduct / Verhaltenskodex

### Grundsätze / Principles

- **Respekt:** Seien Sie respektvoll zu allen
- **Kontext:** Verwenden Sie angemessene Kommunikation
- **Empathie:** Denken Sie über andere Perspektiven nach
- **Zusammenarbeit:** Arbeiten Sie zusammen, nicht gegen andere

### Was zu vermeiden / What to avoid

- ❌ Toxisches Verhalten / Toxic behavior
- ❌ Diskriminierung / Discrimination
- ❌ Persönliche Angriffe / Personal attacks
- ❌ Unangemessene Sprache / Inappropriate language

---

## Build & Deploy / Build & Deploy

### Lokales Testen / Local testing

```bash
# Plugin installieren
cd /path/to/leantime/app/Plugins/
git clone <your-branch> AIAssistant

# Berechtigungen
chown -R www-data:www-data AIAssistant
chmod -R 755 AIAssistant

# Aktivieren
# Admin login → Settings → Plugins → AIAssistant
```

### Versionierung / Versioning

SemVer: MAJOR.MINOR.PATCH

- **1.0.0 → 2.0.0:** Inkompatible Änderungen
- **1.0.0 → 1.1.0:** Neue Features (compatible)
- **1.0.0 → 1.0.1:** Bugfixes (compatible)

### Release-Prozess / Release process

1. Feature-Branch mergen in `main`
2. Version in CHANGELOG.md aktualisieren
3. Git Tag erstellen: `git tag -a v1.0.0 -m "Version 1.0.0"`
4. Tag pushen: `git push origin v1.0.0`
5. GitHub Release erstellen

---

## Fragen / Questions

### Support / Support

- **GitHub Issues:** https://github.com/samir-brkic/leantime-aiassistant/issues
- **Discussions:** https://github.com/samir-brkic/leantime-aiassistant/discussions
- **Leantime Community:** https://leantime.io/community

### Hilfsressourcen / Help resources

- [Leantime Documentation](https://docs.leantime.io)
- [Leantime Plugin Template](https://github.com/Leantime/plugin-template)
- [PHP Documentation](https://php.net)
- [PSR-12](https://www.php-fig.org/psr/psr-12/)

---

## Credits / Danksagung

- Built for [Leantime](https://leantime.io)
- Based on [Leantime Plugin Template](https://github.com/Leantime/plugin-template)
- AI Providers: [Ollama](https://ollama.ai), [OpenAI](https://openai.com)

---

## License / Lizenz

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
