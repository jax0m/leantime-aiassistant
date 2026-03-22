# Leantime AI Assistant - Quick Reference Guide

## Schneller Überblick / Quick Overview

| Feature | Ollama | OpenAI |
|---------|--------|--------|
| **Kosten** | Kostenlos ✅ | Pay-per-Use 💰 |
| **Server** | Lokal | Cloud |
| **Daten** | Privat | OpenAI |
| **Setup** | Mittel | Einfach |
| **Performance** | Gut | Sehr gut |
| **Timeout** | Konfigurierbar | API-Limits |

---

## Common Tasks / Häufige Aufgaben

### 1. Quick Capture verwenden / Use Quick Capture

```
1. Menü → ⚡ Quick Capture
2. Notiz eingeben
3. Projekt wählen (optional)
4. "AI analysieren" klicken
5. Vorschau prüfen
6. "Tasks erstellen" klicken
```

### 2. Provider konfigurieren / Configure provider

**Ollama:**
```
Settings → AI Settings → Ollama
URL: http://localhost:11434
Model: llama3.1
Test Connection → Save
```

**OpenAI:**
```
Settings → AI Settings → OpenAI
API Key: sk-...
Base URL: https://api.openai.com/v1
Model: gpt-4
Test Connection → Save
```

### 3. Kategorie anpassen / Customize category

```
Settings → AI Assistant → Categories
1. Kategorie auswählen
2. Name, Icon, Farbe ändern
3. Keywords hinzufügen
4. Speichern
```

---

## Priority Mapping / Prioritäts-Abbildung

| AI-Priority | Leantime | Value | Color |
|-------------|----------|-------|-------|
| Critical | Critical | 1 | Red 🔴 |
| High | High | 2 | Orange 🟠 |
| Normal | Medium | 3 | Yellow 🟡 |
| Low | Low | 4 | Green 🟢 |

---

## Deadline Examples / Deadline-Beispiele

| Input / Eingabe | Output / Ausgabe |
|-----------------|------------------|
| "morgen" | Tomorrow + 1 day |
| "übermorgen" | Tomorrow + 2 days |
| "in 3 Tagen" | Today + 3 days |
| "nächste Woche" | Today + 7 days |
| "bis Freitag" | Next Friday |
| "am 15. März" | 2026-03-15 |
| "dringend" | Today (0 days) |

---

## Category Keywords / Kategorie-Keywords

| Category / Kategorie | Keywords / Keywords |
|---------------------|---------------------|
| kundenbestellung | bestellung, kunden, kaufen, order |
| einkauf | einkauf, material, lieferant, supplier |
| anfrage | anfrage, frage, preis,咨询 |
| reklamation | reklamation, mangel, defekt, broken |
| buchhaltung | buchhaltung, rechnung, zahlung, invoice |
| organisation | organisation, büro, lager, office |
| design | design, ui, frontend, styling |
| development | development, code, api, backend |

---

## Troubleshooting / Fehlerbehebung

### ❌ "AI provider not configured"

**Solution / Lösung:**
```
Settings → AI Settings
→ Choose provider (Ollama/OpenAI)
→ Fill required fields
→ Test Connection
```

### ❌ Ollama connection fails

**Solution / Lösung:**

**Local:**
```
URL: http://localhost:11434
```

**Docker:**
```
URL: http://host.docker.internal:11434
# Add to docker-compose.yml:
services:
  leantime:
    extra_hosts:
      - "host.docker.internal:host-gateway"
```

### ❌ OpenAI API error

**Common causes / Häufige Ursachen:**
1. Invalid API key → Check OpenAI console
2. Rate limit → Upgrade plan
3. Model unavailable → Check model status

### ❌ AI timeout

**Solution / Lösung:**
```
Settings → AI Settings → Timeout
→ Increase to 90-120 seconds
→ For 70B+ models: 120s
```

---

## API Commands / API-Befehle

### Quick Capture (cURL)
```bash
curl -X POST http://leantime/AIAssistant/quickCapture \
  -H "Content-Type: application/json" \
  -d '{"note": "Test notiz"}'
```

### Settings Get
```bash
curl http://leantime/AIAssistant/settings
```

### Settings Update
```bash
curl -X POST http://leantime/AIAssistant/settings \
  -H "Content-Type: application/json" \
  -d '{"provider": "openai", "openai_api_key": "sk-..."}'
```

---

## Database Queries / Datenbankabfragen

### Check Settings
```sql
SELECT * FROM zp_aiassistant_settings;
```

### Check Categories
```sql
SELECT * FROM zp_aiassistant_categories;
```

### Add Category
```sql
INSERT INTO zp_aiassistant_categories (
    name, name_display, icon, color, keywords, is_default
) VALUES (
    'support', 'Support', '🎧', '#5bc0de', 
    'support,hilfe,frage', 0
);
```

### Check Tasks Created
```sql
SELECT COUNT(*) FROM zp_tickets 
WHERE tags LIKE '%Bestellung%';
```

---

## Log Messages / Log-Nachrichten

### Success Messages / Erfolg
```
AIAssistant: Tasks created successfully
AIAssistant: Connection tested successfully
AIAssistant: Settings saved successfully
```

### Error Messages / Fehler
```
AIAssistant: Connection failed. Please check URL and configuration.
AIAssistant: No models found. Please check Ollama server.
AIAssistant: AI analysis failed. Please try again.
AIAssistant: Error creating tasks: ...
```

---

## Performance Tips / Performance-Tipps

### Optimizing / Optimieren

1. **Categories cached** → Fast loading
2. **Timeout 60s** → Balance speed/quality
3. **Small models** (7B/13B) → Faster
4. **Large models** (70B+) → More accurate, slower

### Monitoring / Überwachung

```bash
# Check Ollama
curl http://localhost:11434/api/tags

# Check OpenAI
curl https://api.openai.com/v1/models \
  -H "Authorization: Bearer sk-..."

# Check Leantime logs
tail -f /var/www/html/cache/logs/error.log
```

---

## Security Checklist / Sicherheits-Checkliste

- [ ] API Keys nicht in Git
- [ ] Ollama nicht öffentlich erreichbar
- [ ] OpenAI Key rotieren regelmäßig
- [ ] Timeout angemessen konfigurieren
- [ ] Rate Limits beachten

---

## Support Contacts / Support-Kontakte

| Service | URL |
|---------|-----|
| GitHub Issues | https://github.com/samir-brkic/leantime-aiassistant/issues |
| GitHub Discussions | https://github.com/samir-brkic/leantime-aiassistant/discussions |
| Leantime Community | https://leantime.io/community |
| Ollama Docs | https://ollama.ai |
| OpenAI Docs | https://platform.openai.com/docs |

---

## Version / Version

- **Version:** 1.0.0
- **Release:** 2026-02-07
- **Status:** Production Ready ✅
