# OWL

## What is OWL?

OWL is a desktop app that brings together the tools you use to think and work — **notes, a task manager, PDF Q&A, time tracking, meetings, and reminders** — with an **AI assistant that runs entirely on your own computer.**

Most "AI note apps" send your notes to someone else's servers. OWL doesn't. The language model runs locally (bundled, or your own [Ollama](https://ollama.com)), your notes are plain Markdown files on your disk, and sensitive notes can be encrypted behind a master passphrase. If you pull the network cable, OWL keeps working.

> **Who it's for:** anyone who wants a capable, AI-powered workspace but isn't comfortable uploading their private notes, client work, or documents to the cloud.

---

## Highlights

- 🔒 **Private by default** — notes, documents, tasks, and AI chats stay on your device. No account, no sync-to-cloud, no telemetry.
- 🤖 **A local AI that actually helps** — ask questions about your own notes and PDFs and get answers *with citations*, powered by a model running on your machine.
- 📝 **Notes you own** — a folder tree of plain Markdown files you can read, back up, or edit with any other tool. Rich-text and raw-Markdown editing, autosave, full-text search.
- 📄 **Chat with your PDFs** — drop in a document; OWL extracts the text locally and lets you ask about it.
- ✅ **Tasks & projects** — projects, subtasks, due dates, blocked flags, drag-to-reorder, auto-archive on completion.
- ⏱️ **Time tracking & My Day** — per-task timers and a daily report of where your hours went, across tasks and meetings.
- 🔗 **Jira integration** — link tasks to issues, push worklogs, post comments (or prepare them to paste manually).
- 🔔 **Reminders & notifications** — due-soon/overdue nudges and long-running-timer alerts, as native desktop notifications.
- 🎨 **Themeable** — Soft Peach, Off-White, and Charcoal (dark) themes.
- 🔐 **Encryption** — protect the vault with a master passphrase; give individual notes their own password.
- ✈️ **Fully offline** — works with no internet connection.

---

## The AI is *actually* local

This is the core promise, so here's exactly how it works:

- The language model runs as a local process on your computer — either a **bundled engine** (llama.cpp) or your own **Ollama** install. You pick the model; it's downloaded once and stored locally.
- "Ask about my notes" uses **retrieval-augmented generation (RAG)** entirely on-device: OWL searches your notes with a hybrid of keyword and semantic search, then asks the local model to answer using only the matching passages — and shows you which notes it used.
- **The only network request OWL makes on its own** is an optional check for app updates (which you can turn off in Settings). Your notes, documents, and conversations are never sent anywhere.
- Encrypted notes are **never** indexed or fed to the AI in plaintext.

---

## Features in detail

| Area | What you get |
|------|--------------|
| **Notes** | Markdown folder/page tree, dual rich-text + Markdown editor, autosave, full-text search, import `.md` files, per-note or master-passphrase encryption, export to Markdown/PDF. |
| **AI Assistant** | One docked panel with four modes — **Chat** (free chat), **Notes** (cited Q&A over your vault), **Documents** (Q&A over PDFs), and **Tasks** (create tasks/subtasks/comments by asking). |
| **Dashboard** | In-progress tasks across projects, today's reminders, and colour-coded sticky notes. |
| **My Day** | Log meetings and see a daily time report (tasks by project + meetings) as proportional bars. |
| **Documents** | Import PDFs into folders, read the extracted text, and ask questions about them. |
| **Tasks & Projects** | Statuses, due dates, blocked reasons, drag-to-reorder, auto-archive, subtasks, comments. |
| **Time tracking** | Per-task timers with per-day records; log straight to Jira. |
| **Costs** | Per-project expenses with CSV export. |
| **Jira** | Per-project setup (API token, session cookie, or manual), worklog push, comments, ticket links. |
| **System tray** | Quick actions, live status, close-to-tray, graceful shutdown. |

---

## Install

> **Platform:** OWL is currently distributed for **macOS** (Apple Silicon and Intel). It's built on Tauri and the codebase targets macOS first; Linux/Windows builds are possible from source but not officially packaged yet.

1. Download the latest **`OWL_x.y.z_<arch>.dmg`** from the [**Releases**](../../releases) page.
2. Open the `.dmg` and drag **OWL** into your **Applications** folder.
3. Launch OWL. On first run you'll set a **master passphrase** for your vault.

### ⚠️ "OWL is damaged and can't be opened" — read this

OWL is **not** code-signed with an Apple Developer certificate yet, so when you download the `.dmg`, macOS quarantines it and Gatekeeper shows a misleading **"OWL is damaged and can't be opened"** message. **The app is not damaged** — macOS is just refusing to run an unsigned, downloaded app.

To open it, run this once in Terminal after moving OWL to Applications:

```bash
xattr -dr com.apple.quarantine /Applications/OWL.app
```

Then open OWL normally. (This is a one-time step; updates installed from within the app are not affected.)

### First run

- OWL downloads a small AI model on first use (you choose which in **Settings → Local AI**). The default is tiny and fast; larger models give better answers at the cost of RAM and disk.
- Prefer to use your own models? Install [Ollama](https://ollama.com), then pick the **Ollama** provider in Settings — OWL will start and stop it for you.
- For "ask my notes" semantic search, download the embedding model in **Settings → Notes Q&A** and click **Rebuild semantic index**. (Keyword search works without it.)

---

## Keeping it updated

OWL can check for new versions and update itself (downloads are verified before installing, and it never installs without asking). You control this in **Settings → Updates**:

- **Check automatically** (default) or only when you click.
- **Download in the background**, then prompt before installing — OWL never restarts on its own.

---

## Privacy & security, briefly

- **Local-only data.** Notes are Markdown files under your app-data folder; structured data (tasks, time, etc.) is a local SQLite database. Nothing is uploaded.
- **Encryption.** A master passphrase (Argon2id-derived key) unlocks the vault. Individual notes can be encrypted with AES-256-GCM, optionally under their own password. A forgotten passphrase can't be recovered — that's the point.
- **No telemetry.** OWL doesn't phone home. The only outbound request it initiates is the optional update check.

---

## Tech stack

Tauri v2 · Rust · Vue 3 + TypeScript · Vite · Pinia · SQLite (FTS5 + sqlite-vec) · llama.cpp / Ollama · Tiptap + CodeMirror.

---

## FAQ

**Is my data sent anywhere?**
No. Everything is local. The only request OWL makes on its own is an optional update check you can disable.

**Do I need a GPU or lots of RAM?**
No GPU required. The default model is small and runs on modest machines; you can choose larger models if you have the RAM.

**Can I use my own models?**
Yes — switch to the Ollama provider in Settings and use any model you've pulled.

**Where are my notes stored?**
As plain `.md` files in OWL's app-data vault, so you can back them up or read them with any editor. On macOS: `~/Library/Application Support/dev.owl.app/vault/`.

**Why does macOS say the app is damaged?**
It isn't — it's unsigned. See the [install note above](#️-owl-is-damaged-and-cant-be-opened--read-this).

**Is there a Windows/Linux version?**
Not packaged yet, but can be made avilable.

---
