# Changelog

All notable changes to the Leantime AI Assistant Plugin will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.1] - 2026-03-22

### 📝 Documentation Updates
- **Simplified documentation structure**: Removed API.md and Developer Guide.md for end-user focus
- **Enhanced Architecture.md**: Streamlined to component/function map only (removed detailed data flows)
- **Expanded User Guide**: Added troubleshooting section with common issues and solutions
- **Updated version numbers**: All documentation now reflects v1.1.1

### 🔧 Refactoring & Improvements
- **Enhanced error logging**: Improved logging in AIAssistant.php with detailed curl error information
- **Response validation**: Added comprehensive response validation for OpenAI API calls
- **Documentation cleanup**: Removed redundant technical documentation, focused on user experience

---

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

