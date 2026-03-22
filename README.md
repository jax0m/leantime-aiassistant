# Leantime AI Assistant Plugin

[![Leantime](https://img.shields.io/badge/Leantime-3.x-blue.svg)](https://leantime.io)
[![PHP](https://img.shields.io/badge/PHP-8.1+-purple.svg)](https://php.net)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Transform freetext notes into structured tasks using AI - locally with Ollama or via OpenAI.

![AI Assistant Plugin](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)

## ✨ Features

- 🤖 **AI-Powered Task Creation**: Convert freetext notes into structured Leantime tasks
- 🎯 **Smart Categorization**: Automatically assigns tasks to 8 business categories (Design, Development, Bug, etc.)
- 🏷️ **Auto-Tagging**: Generates relevant tags based on content
- 📅 **Intelligent Deadline Parsing**: Understands "tomorrow", "in 2 weeks", "next Monday"
- 📝 **Subtask Generation**: Breaks down complex tasks automatically
- ⚡ **Dual Provider Support**: Use Ollama (local, free) or OpenAI
- ✏️ **Editable Preview**: Review and modify AI suggestions before creating tasks
- 🎨 **Native Design**: Seamlessly integrates with Leantime's UI
- 🌍 **Multilingual**: German and English included

## 🎬 Quick Start

### Installation

1. **Download the plugin:**
   ```bash
   cd /path/to/leantime/app/Plugins/
   git clone https://github.com/samir-brkic/leantime-aiassistant.git AIAssistant
   ```

2. **Set permissions:**
   ```bash
   chown -R www-data:www-data AIAssistant
   chmod -R 755 AIAssistant
   ```

3. **Activate in Leantime:**
   - Login as Administrator
   - Navigate to: **Settings → Plugins**
   - Find **AIAssistant** and click **Activate**
   - Database tables are created automatically

4. **Configure AI Provider:**
   - Navigate to: **AI Assistant → Settings**
   - Choose Provider (Ollama or OpenAI)
   - Enter connection details
   - Select model
   - Save & Test connection

5. **Start using:**
   - Navigate to: **AI Assistant → Quick Capture**
   - Write your note
   - Let AI analyze it
   - Review & edit
   - Create task! 🎉

## 🔧 Configuration

### Option A: Ollama (Local, Free)

1. Install [Ollama](https://ollama.ai)
2. Pull a model: `ollama pull llama3.1`
3. In plugin settings:
   - Provider: **Ollama**
   - URL: `http://localhost:11434` (or `http://host.docker.internal:11434` for Docker)
   - Select model from dropdown

### Option B: OpenAI (Cloud, Paid)

1. Get API key from [OpenAI](https://platform.openai.com/api-keys)
2. In plugin settings:
   - Provider: **OpenAI**
   - URL: `https://api.openai.com/v1`
   - API Key: Your key
   - Select GPT model

## 📋 Requirements

- **Leantime:** 3.x or higher
- **PHP:** 8.1+
- **Database:** MySQL/MariaDB
- **AI Provider:** Ollama Server OR OpenAI API Key

## 🏗️ Architecture

```
AIAssistant/
├── Controllers/         # QuickCapture & Settings
├── Services/           # AI Integration, Task Generation, Categories
├── Repositories/       # Database Access
├── Models/            # Data Structures
├── Templates/         # UI Views
├── Language/          # Translations (DE/EN)
└── Install/           # Database Migrations
```

## 🎯 Categories

The plugin includes 8 pre-configured business categories:

- 🎨 **Design**: UI/UX, Frontend, Styling
- 🔧 **Development**: Code, API, Backend
- 🐛 **Bug**: Errors, Issues, Fixes
- 📋 **Planning**: Concepts, Roadmaps, Meetings
- 📄 **Documentation**: Docs, Wiki, README
- 🧪 **Testing**: QA, Unit Tests, E2E
- 🚀 **Deployment**: Release, CI/CD, DevOps
- 💬 **Communication**: Updates, Reviews

Categories are fully customizable via database.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 🐛 Troubleshooting

### Plugin not showing in menu

```bash
# Clear Leantime cache
rm -rf /path/to/leantime/cache/framework/*
```

### Connection to Ollama fails (Docker)

If Leantime runs in Docker and Ollama on host:
- Use: `http://host.docker.internal:11434`
- Not: `http://localhost:11434`

Add to `docker-compose.yml`:
```yaml
services:
  leantime:
    extra_hosts:
      - "host.docker.internal:host-gateway"
```

### AI response timeout

For large models, increase timeout in settings (default: 60s, recommended for 70B+ models: 90-120s).

## 📚 Documentation

**Documentation is available in the [docs/](docs/) folder:**

- 📖 [**USER_GUIDE.md**](docs/USER_GUIDE.md) - Complete user manual with troubleshooting (DE/EN)
- ⚡ [**QUICK_REFERENCE.md**](docs/QUICK_REFERENCE.md) - Common tasks and quick reference (DE/EN)
- 🏗️ [**ARCHITECTURE.md**](docs/ARCHITECTURE.md) - System architecture overview (DE/EN)

**Additional docs:**
- [INSTALLATION.md](INSTALLATION.md) - Installation guide (German)
- [LEANTIME_INTEGRATION.md](LEANTIME_INTEGRATION.md) - Technical integration details


## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Credits

- Built for [Leantime](https://leantime.io)
- Based on [Leantime Plugin Template](https://github.com/Leantime/plugin-template)
- Supports [Ollama](https://ollama.ai) and [OpenAI](https://openai.com)

## 📧 Support

- **Issues**: [GitHub Issues](https://github.com/samir-brkic/leantime-aiassistant/issues)
- **Discussions**: [GitHub Discussions](https://github.com/samir-brkic/leantime-aiassistant/discussions)
- **Leantime Community**: [leantime.io/community](https://leantime.io/community)

---

**Version:** 1.1.1  
**Compatibility:** Leantime 3.x  
**Status:** ✅ Production Ready

Made with ❤️ for the Leantime community
