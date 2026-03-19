# CLAUDE.md — ScrollStop

## Project Overview

**ScrollStop** is a zero-dependency Progressive Web App (PWA) for social media content optimization. It provides real-time feedback on posts including character counts, readability scores, hook strength analysis, hashtag suggestions, and mobile/desktop previews. Built by Dr. Rob Voss / Voss AI Consulting.

**Live deployment:** Hosted on Vercel with automatic deploys from `main`.

## Tech Stack

- **Pure HTML/CSS/JavaScript** — no frameworks, no build step, no npm dependencies
- **Single-file architecture** — the entire app lives in `index.html` (~1000 lines)
- **PWA** — service worker (`sw.js`) + manifest (`manifest.json`) for offline/installable support
- **Optional AI** — Claude API (Anthropic) for hook generation, called directly from browser
- **Hosting** — Vercel (configured via `vercel.json`)

## File Structure

```
index.html          # Entire application (HTML + CSS + JS inline)
sw.js               # Service worker for offline caching
manifest.json       # PWA manifest
vercel.json         # Vercel routing & headers
icon.svg            # Vector logo
icon-192.png        # PWA icon 192x192
icon-512.png        # PWA icon 512x512
LICENSE             # Custom license (commercial use restricted)
README.md           # User-facing documentation
```

## Architecture

### Single-File App (`index.html`)

The app is structured within one HTML file:

- **Lines 1–179:** `<head>` with all CSS (design system via CSS custom properties in `:root`)
- **Lines 180–521:** `<body>` HTML structure — splash screen, help overlay, sidebar, 5 page divs
- **Lines 522–997:** `<script>` — all JavaScript logic

### Five Tool Pages

Each tool is a `<div class="page">` toggled by sidebar navigation:

1. **Text Formatter** (`page-formatter`) — Unicode text styling with 14 format types
2. **Post Analyzer** (`page-analyzer`) — character count, Flesch-Kincaid readability, hook scoring, optimization tips
3. **Hook Generator** (`page-hooks`) — template-based + optional Claude API hook generation
4. **Hashtag Analyzer** (`page-hashtags`) — content-aware hashtag suggestions from curated database
5. **Post Preview** (`page-preview`) — mobile/desktop preview with platform-specific "see more" truncation

### Five Supported Platforms

LinkedIn, X/Twitter, Instagram, Facebook, Threads — each with unique config for character limits, hashtag rules, fold points, and preview behavior. All stored in the `PLATFORMS` object.

## Code Conventions

### Naming

- **DOM IDs** use short prefixes by feature: `fmt-*` (formatter), `a-*` (analyzer), `h-*` (hooks), `ht-*` (hashtags), `p-*` (preview)
- **Global state:** `currentPlatformId`, `currentFormat`, `htSel[]`
- **Platform config accessor:** `plt()` returns current platform object

### CSS

- Dark theme with CSS custom properties (`--bg-*`, `--blue-*`, `--gold-*`, `--text-*`)
- Button classes: `btn-gold`, `btn-blue`, `btn-ghost`, `btn-sm`, `btn-lg`
- Card-based layout with `.card` and `.card-label`
- Responsive breakpoint at 768px (sidebar hidden on mobile, mobile header shown)

### JavaScript

- No module system — all code in a single inline `<script>` block
- DOM access via `getElementById()` / `querySelector()`
- Event handlers mostly via inline `onclick` attributes in HTML
- `async/await` for Claude API calls
- Section separators: `// ==== SECTION NAME ====`

### Key Principle: Zero Dependencies

All functionality is built with vanilla web technologies. **Do not introduce npm packages, build tools, or external libraries.** This is an intentional design constraint.

## Development Workflow

### Local Development

```bash
# No install or build step needed
# Simply open index.html in a browser

# For service worker testing (requires HTTPS):
npx serve .
```

### Deployment

Push to `main` triggers automatic Vercel deployment. Routing is configured in `vercel.json` — all routes rewrite to `index.html`.

### Testing

No automated test framework. Test manually in the browser:
- All 5 tools across all 5 platforms
- PWA install on iOS/Android/desktop
- Offline functionality via service worker
- Claude API integration (optional, needs API key)

## Key Data Structures

### `PLATFORMS` object

Each platform entry contains: `id`, `name`, `icon`, `maxChars`, `warnChars`, `sweetMin`, `sweetMax`, `maxHashtags`, `hashtagPosition`, `mobileFold`, `desktopFold`, readability/tip strings, `hookSystemPrompt`, `showDesktopPreview`.

### `MAPS` object

Unicode character maps for text formatting (bold, italic, script, fraktur, double-struck, circled, squared, fullwidth, small-caps, strikethrough, underline).

### `htDB` object

Hashtag database organized by topic category (ai, education, leadership, business, productivity, training, data, tech). Each entry has `tag`, `size` (Broad/Mid/Niche), and `f` (follower count).

## Key Algorithms

- **Flesch-Kincaid Readability:** Custom syllable counter + FL/FK grade formulas
- **Hook Strength:** 4-point scoring (numbers, questions, length, strong openers, trigger words)
- **Unicode Formatting:** Maps ASCII to mathematical alphanumeric Unicode blocks (U+1D000–U+1D7FF) plus combining characters for strikethrough/underline

## Claude API Integration

- Endpoint: `https://api.anthropic.com/v1/messages`
- Model: `claude-sonnet-4-5-20250514`
- API key provided by user in the Hook Generator UI (never stored)
- Uses `anthropic-dangerous-direct-browser-access: true` header for browser requests
- Falls back to template-based hooks if API unavailable

## Common Tasks for AI Assistants

### Adding a new platform

1. Add entry to `PLATFORMS` object with all required fields
2. Add platform button in sidebar HTML (`#sidebar`) and mobile dropdown
3. Update service worker cache version in `sw.js` if adding assets

### Adding a new tool/page

1. Add `<div class="page" id="page-newname">` in the HTML body
2. Add sidebar navigation button with `onclick="showPage('newname')"`
3. Add corresponding JavaScript logic in the `<script>` block
4. Follow existing ID prefix convention (pick a short prefix)

### Modifying the design system

All colors/spacing are in CSS custom properties at the top of `index.html` in the `:root` selector.

## Important Constraints

- **No build step** — everything must work by opening `index.html` directly
- **No external dependencies** — vanilla HTML/CSS/JS only
- **Single file** — keep the app in `index.html` (inline CSS + JS)
- **Offline-capable** — update `sw.js` cache version when changing assets
- **License** — custom license; commercial use requires a separate license from Voss AI Consulting
