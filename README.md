# 🚀 PackSense — AI-Powered Package Setup for VS Code

> Stop wasting time on outdated docs. PackSense fetches real-time package versions from official registries, detects breaking changes, and lets AI set up your entire project automatically — all without leaving VS Code.

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

### 🔍 Smart Package Search
- Type any library name — PackSense searches **all major registries simultaneously** in real-time
- Fuzzy matching support — `langchin` → finds `langchain` automatically
- Shows latest version, release date, weekly downloads across ecosystems

### 📡 Multi-Registry Real-Time APIs

| Language | Registry | What's Fetched |
|----------|----------|----------------|
| 🐍 Python | PyPI | Latest version, dependencies |
| 📦 JavaScript | npm | Latest version, changelog |
| 🦀 Rust | Crates.io | Latest stable release |
| 🎯 Dart/Flutter | pub.dev | Latest version, compatibility |
| 💎 Ruby | RubyGems | Latest gem version |
| ☕ Java | Maven Central | Latest artifact |
| 🐙 All | GitHub Releases | Breaking changes, migration guide |

### 📋 What's New — Breaking Changes Alert
- Fetches official GitHub release notes in real-time
- AI summarizes what changed in simple language
- Warns about breaking changes before you upgrade

### 🤖 AI Chat Panel (Inside VS Code)
- Ask anything about any package — AI answers using **verified real-time data**
- No outdated training data — every response backed by official API results
- Explains concepts, migration steps, and best practices

### ⚡ Auto Setup — One Click Project Config
- Generates correct install command for your ecosystem (`pip install` / `npm install`)
- Creates boilerplate starter code with latest syntax
- Auto-generates config files — `.env`, `requirements.txt`, `schema.prisma`, etc.
- Runs everything in terminal with your confirmation

### ✅ Confirmation Popup — Always In Control
- Shows exactly what will be run before executing anything
- Never touches your files without explicit permission

---

## 🏗️ Architecture

### 📸 System Design Diagram

<p align="center">
  <img src="./Screenshot 2026-03-02 142741.png" alt="PackSense System Design" width="650"/>
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

> Built with ❤️ to make developers' lives easier. No more outdated setup headaches.