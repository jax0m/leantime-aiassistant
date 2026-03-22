# Leantime AI Assistant - Benutzerhandbuch

## Einleitung / Introduction

Leantime AI Assistant hilft Ihnen, freitextbasierte Notizen (aus Anrufen, E-Mails, Zurufen) automatisch in strukturierte Leantime-Tasks umzuwandeln. Das Plugin spart Zeit und sorgt für konsistente Datenerfassung.

Leantime AI Assistant helps you automatically convert free-text notes (from calls, emails, shouts) into structured Leantime tasks. The plugin saves time and ensures consistent data entry.

---

## Installation / Installation

### Schritt 1: Plugin installieren / Step 1: Install Plugin

```bash
cd /pfad/zu/leantime/app/Plugins/
git clone https://github.com/samir-brkic/leantime-aiassistant.git AIAssistant
```

Oder manuell kopieren:

```bash
cp -r AIAssistant /pfad/zu/leantime/app/Plugins/
```

### Schritt 2: Berechtigungen setzen / Step 2: Set Permissions

```bash
chown -R www-data:www-data /pfad/zu/leantime/app/Plugins/AIAssistant
chmod -R 755 /pfad/zu/leantime/app/Plugins/AIAssistant
```

### Schritt 3: Plugin aktivieren / Step 3: Activate Plugin

1. In Leantime als Administrator einloggen
2. Navigiere zu: **Einstellungen → Plugins**
3. Finde **AIAssistant** in der Liste
4. Klicke auf **Aktivieren**

Die Datenbanktabellen werden automatisch erstellt:
- `zp_aiassistant_settings`
- `zp_aiassistant_categories` (mit 8 Standard-Kategorien)

### Schritt 4: AI-Provider konfigurieren / Step 4: Configure AI Provider

Navigiere zu: **AI Assistant → Settings**

#### Option A: Ollama (lokal, kostenlos) / Option A: Ollama (local, free)

1. **Ollama installieren:** https://ollama.ai
2. **Modell herunterladen:**
   ```bash
   ollama pull llama3.1
   ```
3. **In Leantime:**
   - Provider: **Ollama**
   - URL: `http://localhost:11434` (oder `http://host.docker.internal:11434` bei Docker)
   - Modell: Wählen Sie aus der Dropdown-Liste
   - Speichern

#### Option B: OpenAI (Cloud, kostenpflichtig) / Option B: OpenAI (Cloud, paid)

1. **API-Key erhalten:** https://platform.openai.com/api-keys
2. **In Leantime:**
   - Provider: **OpenAI**
   - API Key: Ihren Key eingeben
   - Base URL: `https://api.openai.com/v1`
   - Modell: z.B. `gpt-4` oder `gpt-4-turbo`
   - Speichern

### Schritt 5: Verbindung testen / Step 5: Test Connection

Klicken Sie auf **Verbindung testen** / **Test Connection**. Ein grünes Häkchen zeigt erfolgreiche Verbindung an.

---

## Quick Capture - Erste Schritte / Quick Capture - Getting Started

### Menü finden / Find Menu

Das Quick Capture Menü ist unter **default → ⚡ Quick Capture** verfügbar.

The Quick Capture menu is available under **default → ⚡ Quick Capture**.

### Notiz eingeben / Enter Note

1. Öffnen Sie **⚡ Quick Capture**
2. Geben Sie Ihre Notiz in das Textfeld ein
3. Wählen Sie das Projekt (optional)
4. Klicken Sie auf **🤖 AI analysieren**

**Beispiel-Notizen / Example Notes:**

- "Kunde Müller möchte 50 Schrauben bestellen. Erst Lagerbestand prüfen, dann Angebot erstellen."
- "Hr. Schmidt ruft an wegen Glasbruch. Neu bestücken, dringend!"
- "Anfrage von Firma Weber: Preis für Edelstahlhalter, 10 Stück"
- "Reklamation: Kunde Klug sagt, Lieferung falsch war"

### AI-Analyse verstehen / Understand AI Analysis

Nach dem Klicken auf **AI analysieren** zeigt das System eine Vorschau an:

After clicking **Analyze with AI**, the system displays a preview:

- **Titel / Title:** Prägnante Zusammenfassung
- **Beschreibung / Description:** Ausgewertete Details mit Kontaktdaten
- **Kategorie / Category:** Automatisch zugeordnete Kategorie
- **Priorität / Priority:** Kritisch, Hoch, Mittel, Niedrig
- **Deadline / Deadline:** Ausgewertet (z.B. "morgen", "in 2 Wochen")
- **Subtasks / Subtasks:** Wenn vorhanden
- **Tags / Tags:** Automatische Stichworte

### Task erstellen / Create Task

Klicken Sie auf **✅ Tasks erstellen**, wenn die Vorschau korrekt ist.

The tasks are created and visible in the selected project.

---

## Einstellungen konfigurieren / Configure Settings

### AI Provider / AI Provider

Wählen Sie zwischen Ollama (lokal) oder OpenAI (Cloud):

Choose between Ollama (local) or OpenAI (cloud):

**Ollama:**
- Keine Kosten, läuft lokal
- Daten bleiben privat
- Benötigt eigenes Modell

**OpenAI:**
- Kostenpflichtig (Pay-per-Use)
- Hochleistungsmodelle
- Kein lokaler Server nötig

### Timeout / Timeout

Standard: 30 Sekunden. Für große Modelle (70B+) empfehlen wir 90-120 Sekunden.

Default: 30 seconds. For large models (70B+) we recommend 90-120 seconds.

### System-Prompt anpassen / Customize System Prompt

Der System-Prompt definiert, wie die AI Notizen analysiert. Standard-Prompt ist auf Deutsch konfiguriert.

The system prompt defines how AI analyzes notes. The default prompt is configured in German.

**Standard-Prompt enthält / Standard prompt includes:**
- Unternehmens-Kontext (Schilder, Glas, Befestigungstechnik)
- Format-Anweisungen (JSON-Antwort)
- Feld-Definitionen (Titel, Beschreibung, Kategorie, etc.)
- Prioritäts-Logik
- Deadline-Parsing-Regeln

**Sie können den Prompt anpassen:**

1. Klicken Sie auf **System-Prompt anpassen**
2. Bearbeiten Sie den Text
3. Wichtig: **JSON-Format muss erhalten bleiben!**
4. Klicken Sie auf **Speichern**

**Reset auf Standard:**

Klicken Sie auf **Auf Standard zurücksetzen**, wenn der angepasste Prompt Probleme verursacht.

---

## Kategorien / Categories

### Standard-Kategorien / Default Categories

Das Plugin unterstützt 8 vordefinierte Kategorien:

The plugin supports 8 predefined categories:

1. **📦 Bestellung:** Kunde möchte Produkte kaufen
2. **🛒 Einkauf:** Material muss beim Lieferanten bestellt werden
3. **❓ Anfrage:** Kunde fragt nach Preisen/Beratung
4. **⚠️ Reklamation:** Mängel, Glasbruch, falsche Lieferung
5. **💰 Buchhaltung:** Rechnungen, Zahlungen
6. **📋 Organisation:** Büro, Lager, Sonstiges
7. **🔧 Entwicklung:** Code, API, Backend
8. **🐛 Bug:** Fehler, Issues, Fixes

### Kategorien anpassen / Customize Categories

Kategorien können im Backend verwaltet werden:

Categories can be managed in the backend:

1. **Einstellungen → AI Assistant → Kategorien**
2. Klicken Sie auf **Kategorie bearbeiten**
3. Ändern Sie:
   - Name (z.B. "Anfrage")
   - Display-Name (z.B. "Kundenanfrage")
   - Icon (Emoji oder FontAwesome)
   - Farbe (Hex-Code)
   - Keywords (komma-getrennt, für Auto-Erkennung)
4. **Speichern**

### Keyword-basierte Erkennung / Keyword-based Detection

Wenn die AI keine Kategorie findet, wird automatisch nach Keywords gesucht:

When AI doesn't find a category, it automatically searches for keywords:

```
"bestellung" → 📦 Bestellung
"anfrage" → ❓ Anfrage
"reklamation" → ⚠️ Reklamation
"glassbruch" → ⚠️ Reklamation
"lieferant" → 🛒 Einkauf
```

---

## Troubleshooting / Fehlerbehebung

### Plugin erscheint nicht im Menü

**Lösung:**
1. Leantime-Cache löschen:
   ```bash
   rm -rf /pfad/zu/leantime/cache/framework/*
   ```
2. Browser-Cache löschen (Ctrl+Shift+R)

### "AI provider not configured" / "AI Provider nicht konfiguriert"

**Lösung:**
1. Navigieren Sie zu **AI Assistant → Settings**
2. Wählen Sie einen Provider (Ollama oder OpenAI)
3. Füllen Sie die erforderlichen Felder aus
4. Testen Sie die Verbindung mit **Verbindung testen**

### Ollama Verbindung fehlschlägt

**Bei Docker:**
- URL muss sein: `http://host.docker.internal:11434`
- Nicht: `http://localhost:11434`

**docker-compose.yml anpassen:**
```yaml
services:
  leantime:
    extra_hosts:
      - "host.docker.internal:host-gateway"
```

### OpenAI API-Fehler

**Häufige Ursachen:**
- Ungültiger API-Key
- Rate-Limit erreicht
- Modell nicht verfügbar

**Lösung:**
1. API-Key in OpenAI Console prüfen
2. Modell-Verfügbarkeit prüfen
3. Rate-Limit erhöhen (höherer Plan)

### AI-Analyse Timeout

**Lösung:**
1. Timeout in Einstellungen erhöhen (empfohlen: 90-120s)
2. Für 70B+ Modelle: 120s
3. Kleinere Modelle (7B/13B): 60s reichen

### Tags nicht gespeichert

**Lösung:**
Dies ist ein bekanntes Leantime-Bug. Das Plugin umgeht dies durch direkten Datenbank-Zugriff.

The plugin bypasses this by direct database access.

---

## Best Practices / Best Practices

### 1. Notizen formulieren / Phrase notes clearly

**Gut / Good:**
- "Kunde Müller möchte 50 Schrauben bestellen. Lagerbestand prüfen, dann Angebot."
- "Hr. Schmidt anrufen wegen Glasbruch. Neu bestücken."

**Schlecht / Bad:**
- "Test"
- "Notiz 1"
- "Müll"

### 2. Projekte nutzen / Use projects

Weisen Sie Notizen dem richtigen Projekt zu, damit Tasks automatisch dort erstellt werden.

Assign notes to the correct project so tasks are created there automatically.

### 3. Kategorien anpassen / Customize categories

Passen Sie Kategorien an Ihren Geschäftsbedarf an:

Customize categories to match your business needs:

```
Kategorie: "Projektanfrage"
Keywords: "anfrage,neuprojekt,angebot"
Icon: "📋"
Color: "#3498db"
```

### 4. System-Prompt optimieren / Optimize system prompt

Passen Sie den Prompt an Ihre spezifischen Anforderungen an:

Customize the prompt to your specific requirements:

```
Du bist ein Assistent für ein Baufirma. Erstelle Aufgaben für:
- Neue Aufträge
- Materialbestellungen
- Terminerinnerungen
```

---

## Integration mit Leantime / Integration with Leantime

### Tags als Kategorien verwenden

Kategorien werden als erste Tags gespeichert. Das ermöglicht:

Categories are saved as the first tags. This enables:

- **Filterung nach Kategorie** in Leantime
- **Suche nach Kategorie** in der Ticket-Liste
- **Dashboard-Widgets** nach Kategorie

### Prioritäts-System

Das Plugin verwendet Leantimes Prioritäts-System:

The plugin uses Leantime's priority system:

| AI-Wert | Leantime | Farbe |
|---------|----------|-------|
| critical | 1 | Rot |
| high | 2 | Orange |
| normal | 3 | Gelb |
| low | 4 | Grün |

### Subtasks

Komplexe Aufgaben werden in Subtasks aufgeteilt:

Complex tasks are split into subtasks:

```
Haupttask: "Angebot erstellen"
├─ Subtask: "Preise kalkulieren"
├─ Subtask: "PDF zusammenstellen"
└─ Subtask: "E-Mail senden"
```

---

## Performance / Performance

### Caching

Kategorien werden einmal geladen und im RAM gecacht:

Categories are loaded once and cached in RAM:

- **Vorteil:** Schnellere Ladezeiten
- **Nachteil:** Neue Kategorien erfordern Reload

**Reload nach Kategorie-Änderung:**
- AI Assistant neu laden (F5)
- oder
- Plugin deaktivieren/aktivieren

### AI-Verbindungen

- **Ollama:** Lokaler Server, keine Netzwerk-Latenz
- **OpenAI:** API-Aufrufe, abhängig von Internet

---

## Sicherheit / Security

### API-Keys

**OpenAI:**
- Speichern Sie Keys nie in Git
- Nutzen Sie Umgebungsvariablen oder Leantime Secrets
- Regelmäßig rotieren

**Ollama:**
- Kein API-Key benötigt
- Server muss nicht öffentlich erreichbar sein

### Datenschutz / Privacy

- **Ollama:** Daten bleiben lokal
- **OpenAI:** Daten gehen an OpenAI (siehe OpenAI Datenschutz)

---

## Weiterführende Ressourcen / Further Resources

- [Leantime Plugin Template](https://github.com/Leantime/plugin-template)
- [Ollama Dokumentation](https://ollama.ai)
- [OpenAI API Documentation](https://platform.openai.com/docs/api-reference)
- [Leantime Community](https://leantime.io/community)

---

## Support / Support

Bei Problemen:

For issues:

- **GitHub Issues:** https://github.com/samir-brkic/leantime-aiassistant/issues
- **Discussions:** https://github.com/samir-brkic/leantime-aiassistant/discussions
- **Leantime Community:** https://leantime.io/community

---

## Version / Version

- **Version:** 1.0.0
- **Kompatibilität:** Leantime 3.x, PHP 8.1+
- **Status:** Production Ready
