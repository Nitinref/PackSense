# 🚀 PackSense — AI-Powered Package Setup for VS Code

> Never get outdated setup instructions again. PackSense verifies package versions in real-time and auto-sets up your project using AI.

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![Build Passing](https://img.shields.io/badge/build-passing-brightgreen.svg)
![VS Code Marketplace](https://img.shields.io/badge/VS%20Code-Marketplace-blueviolet.svg?logo=visual-studio-code)

---

## 📌 The Problem

When developers ask AI tools like ChatGPT or Claude how to set up a package (e.g., Prisma, Next.js), the AI often gives **outdated commands** because:

- LLMs are trained on static data with a knowledge cutoff
- Web search may pull from unofficial blogs instead of official docs
- There is no built-in version verification in standard AI chat

**PackSense solves this** by adding a deterministic verification layer before the AI ever generates a response.

---

## ✨ Features

- 📦 **Auto package.json scanner** — detects all installed packages and versions on project open
- 🌐 **Real-time version check** — fetches latest versions from the official npm Registry API
- 📋 **GitHub release notes** — pulls breaking changes and migration guides automatically
- 🤖 **AI Chat Panel** — built directly inside VS Code, no external website needed
- ⚡ **Auto Setup** — runs terminal commands and creates config files automatically
- ✅ **Confirmation Popup** — always asks before touching your files or running commands

---

## 🏗️ Architecture

### 📸 System Design Diagram

<p align="center">
  <img src="./Screenshot 2026-03-02 125744.png" alt="PackSense System Design" width="650"/>
</p>

### 📝 Flow Breakdown

```
Developer (VS Code)
        │
        ▼
   Chat Panel  ──────►  package.json Scanner
   (Side Panel)                │
                               ▼
                    npm Registry API  +  GitHub Releases API
                               │
                               ▼
                    Structured Context Builder
                    (verified version data)
                               │
                               ▼
                       Claude / GPT API
                    (explanation engine only)
                               │
                               ▼
                    Confirmation Popup  ──► [Allow / Cancel]
                               │
                               ▼
              ┌────────────────┴─────────────────┐
              ▼                                   ▼
       Terminal Runner                      File Creator
       (npm install, npx)            (.env, schema.prisma, etc.)
```

---

## 🎬 How It Works

### Step 1 — You type a request
```
"Latest Prisma setup kardo"
```

### Step 2 — Extension fetches verified data
```
npm Registry  →  prisma@5.22.0 (latest)
GitHub        →  Breaking changes from v4 → v5
```

### Step 3 — Structured prompt is sent to AI
```
User Request: Prisma setup
Verified Context:
  - Installed: 4.0.0
  - Latest: 5.22.0
  - Breaking Changes: [migration guide]
```

### Step 4 — AI generates accurate setup steps

### Step 5 — Confirmation popup appears
```
┌─────────────────────────────────────┐
│  PackSense wants to:                │
│                                     │
│  ⚡ Run: npm install prisma@5.22.0  │
│  ⚡ Run: npx prisma init            │
│  📁 Create: schema.prisma           │
│  📁 Create: .env                    │
│                                     │
│     [✅ Allow]      [❌ Cancel]     │
└─────────────────────────────────────┘
```

### Step 6 — Done! Your project is set up with the correct version ✅

---

## 🖥️ UI Preview

```
┌─────────────────────────────────────┐
│  📦 PackSense                       │
├─────────────────────────────────────┤
│  ✅ react         18.2  →  18.3     │
│  ⚠️  prisma        4.0  →  5.22     │
│  ❌ next          13.0  →  15.1     │
├─────────────────────────────────────┤
│  💬 Ask AI                          │
│  > Latest prisma setup kardo...     │
│                                     │
│  [Send]  [Auto Setup]               │
└─────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|------------|
| VS Code Extension | TypeScript + VS Code Extension API |
| Package Registry | npm Registry API (`registry.npmjs.org`) |
| Release Notes | GitHub REST API |
| AI Layer | Claude API / OpenAI GPT API |
| File Operations | VS Code Workspace FS API |
| Terminal Runner | VS Code Terminal API |

---

## 📁 Project Structure

```
packsense/
├── src/
│   ├── extension.ts          # Entry point
│   ├── scanner/
│   │   └── packageScanner.ts # package.json reader
│   ├── api/
│   │   ├── npmRegistry.ts    # npm latest version fetcher
│   │   └── githubReleases.ts # breaking changes fetcher
│   ├── context/
│   │   └── contextBuilder.ts # builds structured AI prompt
│   ├── ai/
│   │   └── aiClient.ts       # Claude / GPT API calls
│   ├── executor/
│   │   ├── terminalRunner.ts # runs terminal commands
│   │   └── fileCreator.ts    # creates config files
│   └── ui/
│       ├── chatPanel.ts      # VS Code webview chat
│       └── confirmation.ts   # confirmation popup
├── package.json
├── tsconfig.json
└── README.md
```

---

## ⚙️ Installation

```bash
# Clone the repo
git clone https://github.com/yourusername/packsense.git
cd packsense

# Install dependencies
npm install

# Build the extension
npm run build

# Open in VS Code
code .

# Press F5 to launch Extension Development Host
```

---

## 🔑 Configuration

Add your API key in VS Code settings:

```json
{
  "packsense.aiProvider": "claude",
  "packsense.apiKey": "your-api-key-here",
  "packsense.autoScanOnOpen": true,
  "packsense.confirmBeforeExecute": true
}
```

---

## 🗺️ Roadmap

- [x] npm package version checker
- [x] GitHub release notes fetcher
- [x] VS Code chat panel
- [x] Auto terminal command runner
- [x] Auto file creator
- [ ] PyPI support (Python packages)
- [ ] Support for private registries
- [ ] package.json auto-update on confirm
- [ ] Multi-package bulk setup

---

## 🤝 Contributing

Pull requests are welcome! Please open an issue first to discuss what you'd like to change.

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

> Built with ❤️ to make developers' lives easier. No more outdated setup headaches.# PackSense
