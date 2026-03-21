# 📚 Learning Hub

**Learning Hub** is a fully self-contained, single-file personal knowledge base and study tool built with pure HTML, CSS, and JavaScript. It runs entirely in the browser with no backend required — all data is stored locally in `localStorage` with optional cloud sync powered by [Puter.js](https://puter.com).

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Getting Started](#getting-started)
- [Data Structure](#data-structure)
- [Storage & Sync](#storage--sync)
- [User Interface](#user-interface)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Modes](#modes)
- [Export Options](#export-options)
- [Media Library](#media-library)
- [Practice & Study Tools](#practice--study-tools)
- [Progress Tracking](#progress-tracking)
- [Dependencies](#dependencies)
- [Customization](#customization)

---

## Overview

Learning Hub organizes knowledge into a three-level hierarchy:

```
Modules
  └── Submodules
        └── Entries (Question + Answer + Subtopic)
```

Each entry consists of a **question** (or topic heading), a **Markdown-formatted answer**, and an optional **subtopic** tag for grouping. The app is designed for personal study, interview preparation, note-taking, and reviewing technical content.

---

## Features

### Core Knowledge Management
- **Three-tier hierarchy** — Modules → Submodules → Entries
- **Markdown-powered answers** — Full GitHub-flavored Markdown rendering via `marked.js` with `DOMPurify` sanitization
- **Syntax highlighting** — Code blocks for JavaScript, CSS, Python, and Java via `Prism.js`
- **Subtopic grouping** — Entries are grouped by subtopic in both the sidebar index and the content area
- **Drag-and-drop reordering** — Reorder entries and subtopic groups via `SortableJS`; a drag lock prevents accidental reordering

### Editing
- **Inline Markdown editor** — Live split-screen preview while writing
- **Fullscreen editor mode** — Expands to a full-viewport split editor/preview
- **Image upload support** — Images can be embedded in answers; they display responsively with a lightbox zoom on click
- **Image alignment controls** — Left, center, right alignment options in the editor
- **Custom dialogs** — Styled prompt/confirm modals replace native browser dialogs

### Search
- **In-submodule search** — Filters entries as you type; match counter shows X of Y results with next/prev navigation
- **Module-level search** — Searches across all submodules in the current module
- **Global search** — Searches all entries across all modules and submodules
- **Keyword highlighting** — Matched terms are highlighted in yellow within results
- **`/` shortcut** — Press `/` anywhere on the Q&A page to instantly focus the search bar

---

## Getting Started

1. **Download** the `index.html` file.
2. **Open** it directly in any modern browser (Chrome, Edge, Firefox, Safari).
3. On first load, a welcome splash screen appears briefly.
4. Click **"+ New Module"** on the dashboard to create your first module.
5. Inside a module, click **"+ Add Submodule"** to create a submodule.
6. Inside a submodule, use **"New Entry"** (in the ⋯ menu or sidebar) to add Q&A entries.

No installation, no server, no build step required.

---

## Data Structure

All knowledge data is stored in `localStorage` under the key `lh_data` as a JSON object:

```json
{
  "Module Name": {
    "Submodule Name": [
      {
        "id": 1700000000000,
        "q": "What is a closure?",
        "a": "A **closure** is a function that retains access to its lexical scope...",
        "subtopic": "Functions"
      }
    ]
  }
}
```

Additional `localStorage` keys used by the app:

| Key | Contents |
|-----|----------|
| `lh_data` | All Q&A entry data |
| `lh_mastery` | Per-entry mastery state (`learning` / `mastered`) |
| `lh_streak` | Study streak dates and best streak |
| `lh_theme` | Current theme (`light` or `dark`) |
| `ebooks_db` | Saved ebook entries (title, author, Google Drive link) |
| `videos_db` | Saved video entries (title, topic, YouTube link) |
| `learninghub_last_state` | Last viewed module/submodule/entry for "Continue Reading" |

---

## Storage & Sync

### Local Storage
All data is automatically persisted to `localStorage` on every change. No manual save step is needed.

### Cloud Sync (Puter.js)
The app integrates [Puter.js](https://js.puter.com/v2/) for optional cloud backup and cross-device sync:

- **Auto-sync** — Changes are debounced and synced to Puter cloud automatically after a short delay
- **Manual sync** — A dedicated sync modal lets you push or pull data manually
- **Sync status indicator** — The header shows sync state (idle, syncing, synced, error)
- Data is stored in Puter's cloud filesystem, so you can access your notes on any device after signing in

---

## User Interface

### Dashboard
The main dashboard displays all modules as cards in a responsive grid. Each module card shows:
- Module name and entry count
- A **Question of the Day (QOTD)** banner at the top, randomly picked from all entries
- **Continue Reading** button — restores the last viewed entry
- **Study Streak widget** — shows current streak with a 7-day dot indicator

### Submodule View
Clicking a module opens its submodule grid. Each submodule card shows:
- Submodule name and entry count
- A progress bar reflecting mastery percentage
- A percentage badge showing how much has been read/marked

### Q&A View (Module Workspace)
The main reading/editing workspace has:
- **Sidebar** (collapsible) — scrollable index of all entries grouped by subtopic, with drag-to-reorder support
- **Content area** — entry cards rendered with Markdown; cards dim when hovering to focus reading
- **Reading focus** — the card currently in the viewport reading zone is auto-highlighted
- **Scroll buttons** — top/bottom quick-scroll

### Header
The header adapts contextually:
- **Dashboard**: Logo, streak widget, Continue Reading button, global actions
- **Submodule page**: Back button, module/submodule breadcrumb, ⋯ actions menu
- **Q&A page**: Hamburger sidebar toggle, breadcrumb, submodule actions (New Entry, Progress, Lock, Book Mode, Export)

### Theme
- **Dark mode** (default) and **Light mode** are supported
- Theme preference is persisted to `localStorage` and applied before paint to prevent flash

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `/` | Focus the search bar (on Q&A page) |
| `Escape` | Blur/close the search bar |

---

## Modes

### Read Mode
Toggled from the dashboard or module settings. In Read Mode:
- All editing controls are hidden (new entry, edit, delete, rename, export buttons)
- Search is disabled
- A toast notification confirms the mode is active
- Useful for distraction-free review or sharing

### Book Mode
Toggled from the ⋯ menu on the Q&A page. In Book Mode:
- The content area is centered on the viewport like a reading column
- Width can be set to **S** (780px), **M** (900px), or **L** (1100px) via pills
- Font size and line height increase for comfortable long-form reading
- Cards use a left-border accent style instead of a box style
- A **Book Mode** indicator badge is fixed in the corner with width controls

### Drag Lock
Toggle from the ⋯ menu. When unlocked, entries and subtopic groups can be reordered via drag and drop. When locked, dragging is disabled to prevent accidental reordering.

---

## Export Options

Both export types are available from the ⋯ submodule actions menu.

### Visual PDF (`Export PDF`)
- Renders each entry card to canvas via `html2canvas`
- Assembles pages in `jsPDF`
- Includes a header on each page with module and submodule name
- Shows a cancellable progress bar during export
- Best for preserving the visual look of formatted cards

### Copyable PDF (`Copyable PDF`)
- Parses Markdown to HTML, then renders it into jsPDF natively
- Supports headings, paragraphs, bullet lists, blockquotes, horizontal rules, tables (via `jspdf-autotable`), and code blocks
- Text is selectable and copyable in the resulting PDF
- Handles Unicode/emoji substitution for PDF compatibility
- Best for text-heavy content or when you need to copy text from the PDF

---

## Media Library

### Ebooks
- Accessible from the dashboard sidebar under **Ebooks**
- Add PDFs via Google Drive share links (auto-converted to embed links)
- Books display as thumbnail cards (Drive thumbnail API)
- Opens PDFs in a full-screen embedded viewer
- Supports bulk delete mode and search

### Videos
- Accessible from the dashboard sidebar under **Videos**
- Add YouTube videos with a title and optional topic tag
- Displays as a grid of thumbnail cards (YouTube thumbnail API)
- Opens videos in a full-screen embedded iframe player
- Supports bulk delete mode and search

---

## Practice & Study Tools

### Flashcard Mode
- Launched from the practice toolbar on the Q&A page
- Displays entries as 3D flip cards (CSS perspective transform)
- Click/tap to flip between question (front) and answer (back)
- Navigate with Previous/Next buttons or keyboard
- Shows a progress bar and card counter
- Optional shuffle mode

### Interview Practice Mode
- Simulates a timed interview scenario
- Select a number of random questions from the current submodule
- Countdown timer with color-coded warning states (yellow → red)
- Reveal answer for each question individually
- Useful for mock interview drills

### Question of the Day (QOTD)
- Shown as a banner at the top of the dashboard
- A random entry is selected each day and persisted so it stays consistent within the day
- Shows the module/submodule breadcrumb path
- Reveal answer inline; click path segments to navigate directly to that entry
- Refresh button picks a new random question

### Weak Topics / Unread Tracker
- Launched from the dashboard
- Lists all submodules sorted by percentage of unread entries
- Heat-mapped progress bars (green → red)
- "Study →" button navigates directly to the weakest submodule

---

## Progress Tracking

### Mastery States
Each entry has one of three states, toggled via the mastery button on each card:

| State | Meaning |
|-------|---------|
| **New** | Not yet interacted with |
| **Learning** | Seen or actively studying |
| **Mastered** | Fully understood |

### Auto-Read Timer
- Uses `IntersectionObserver` to detect when a card is 50% visible in the viewport
- Calculates estimated read time based on word count (at 200 WPM, min 5 seconds)
- Automatically marks an entry as **Learning** after the estimated read time has elapsed
- Timers are paused when cards scroll out of view and resume where they left off

### Progress Modal
- Opened via the ⋯ menu → Progress
- Shows summary statistics (total, mastered, learning, new) as summary cards
- Bar chart breakdown per submodule
- Context-aware: shows submodule detail when inside a submodule, module breakdown when at module level, and global stats from the dashboard

### Study Streak
- Records the current date each time you open the app and view content
- Calculates the current consecutive-day streak
- Stores up to 365 days of history
- Shows best-ever streak
- Displayed as a widget with 7-day dot indicators (one per day of the week)

---

## Text-to-Speech

Each entry card has a 🔊 TTS button that reads the question and answer aloud using the Web Speech API:

- **Voice priority list** — prefers high-quality neural voices (Microsoft Edge natural voices, Google voices, macOS voices) with automatic fallback
- **Chunked playback** — splits long text into sentence-sized chunks to work around Chrome's utterance length bug
- **Pause/Resume** — a floating TTS control bar appears during playback
- **Speed control** — Slow (0.8×), Normal (1×), Fast (1.25×)
- **Code block handling** — replaces `<pre>` and `<code>` blocks with "[code block]" in the spoken text

---

## Copy Entry

Each card has a 📋 copy button that copies the question and answer as plain text to the clipboard in the format:

```
Q: <question text>

A: <answer text>
```

---

## Dependencies

All dependencies are loaded from CDNs — no `npm install` required.

| Library | Version | Purpose |
|---------|---------|---------|
| [marked](https://cdn.jsdelivr.net/npm/marked/) | latest | Markdown → HTML parsing |
| [DOMPurify](https://cdn.jsdelivr.net/npm/dompurify/) | 3.0.6 | Sanitize rendered HTML |
| [Prism.js](https://cdnjs.cloudflare.com/ajax/libs/prism/) | 1.29.0 | Syntax highlighting (JS, CSS, Python, Java) |
| [prism-one-dark](https://cdnjs.cloudflare.com/ajax/libs/prism-themes/) | 1.9.0 | Dark code theme |
| [github-markdown-css](https://cdnjs.cloudflare.com/ajax/libs/github-markdown-css/) | 5.2.0 | GitHub-style Markdown rendering |
| [SortableJS](https://cdnjs.cloudflare.com/ajax/libs/Sortable/) | 1.15.0 | Drag-and-drop reordering |
| [jsPDF](https://cdnjs.cloudflare.com/ajax/libs/jspdf/) | 2.5.1 | PDF generation |
| [jspdf-autotable](https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/) | 3.5.31 | Table support in PDF export |
| [html2canvas](https://cdnjs.cloudflare.com/ajax/libs/html2canvas/) | 1.4.1 | Screenshot-based PDF export |
| [Lucide Icons](https://unpkg.com/lucide/) | latest | UI icon set |
| [Puter.js](https://js.puter.com/v2/) | v2 | Cloud storage and sync |
| [Inter](https://fonts.google.com/specimen/Inter) | — | UI font |
| [Fira Code / Source Code Pro](https://fonts.google.com/) | — | Monospace code font |
| [Noto Sans](https://fonts.google.com/specimen/Noto+Sans) | — | Supplementary UI font |
| [Flaticon UI Icons (Brands)](https://cdn-uicons.flaticon.com/) | 2.6.0 | Brand icon set |

---

## Customization

### Theme Colors
CSS custom properties in `:root` and `.dark` control all colors:

```css
:root {
  --bg: #f8fafc;
  --text: #1e293b;
  --card: #ffffff;
  --sidebar: #f1f5f9;
  --primary: #2563eb;
  --border: #cbd5e1;
  --accent: #6366f1;
  --danger: #ef4444;
  --highlight: #facc15;
  --question-color: #0d9488;
}
```

Modify these values to retheme the entire app instantly.

### Logo
The logo image is loaded from a GitHub CDN URL:
```
https://cdn.jsdelivr.net/gh/anupamroy2021/Learning-Hub/logo.png
```
Replace this URL with your own hosted image to use a custom logo.

### Auto-Read Speed
The words-per-minute constant for the auto-read timer can be adjusted at the top of the relevant script section:
```js
const WPM = 200;         // reading speed
const MIN_READ_MS = 5000; // minimum time before marking as read
```

---

## Browser Compatibility

The app uses standard modern browser APIs:

- **`localStorage`** — data persistence
- **`IntersectionObserver`** — auto-read detection
- **`SpeechSynthesisUtterance`** — text-to-speech (Web Speech API)
- **`navigator.clipboard`** — copy to clipboard
- **CSS `perspective` / `transform-style: preserve-3d`** — flashcard flip animation

Recommended browsers: **Chrome 90+**, **Edge 90+**, **Firefox 90+**, **Safari 15+**.

> Note: Text-to-speech neural voice quality varies significantly by browser. Edge (Windows) and Chrome provide the best experience.

---

## License

This project is a single-file personal study tool. Check the source repository for licensing details.
