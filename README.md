# VAIC ScrollStop

**Stop the scroll. Optimize your content before you post.**

ScrollStop is a free, open-source content optimizer built by [Voss AI Consulting](https://www.vossaiconsulting.com). It gives creators real-time feedback on their social media posts — character count, readability score, hook strength, hashtag suggestions, and a side-by-side mobile vs. desktop preview — all before they hit publish.

Built as a Progressive Web App (PWA). Installable on iOS, Android, and desktop. Works offline. No account required.

![ScrollStop](icon-512.png)

---

## Tools

### Text Formatter
Transform plain text into eye-catching Unicode styles — bold, italic, monospace, script, fraktur, double-struck, circled, squared, and more. LinkedIn doesn't support native text formatting, but these Unicode characters render as styled text on any platform. Type or paste your text, select a style, and copy the formatted output. Use sparingly for emphasis to maintain readability and accessibility.

### Post Analyzer
Real-time character counter with LinkedIn's 3,000-character limit, Flesch-Kincaid readability scoring, average sentence and word length metrics, and smart optimization tips that adapt as you write.

### Hook Generator
AI-powered opening line generator with five tone presets (Professional, Bold/Contrarian, Storytelling, Data-Driven, Vulnerable). Supports Claude API integration for AI-generated hooks or works offline with 40+ smart templates. Click any hook to copy.

### Hashtag Analyzer
Content-aware hashtag suggestions. Paste your post and the analyzer detects topics (AI, education, leadership, business, productivity, training, data, tech) and surfaces relevant hashtags with follower counts and reach categories. Select up to 5 and copy to clipboard.

### Post Preview
Side-by-side mobile and desktop rendering that shows exactly where your post gets truncated behind "see more." Customizable display name and headline. Know what your audience sees before they click.

---

## Install as an App

### Android
Visit the site in Chrome. Tap the install banner or use Menu → "Add to Home Screen."

### iOS
Visit the site in Safari. Tap Share → "Add to Home Screen."

### Desktop
Visit the site in Chrome or Edge. Click the install icon in the address bar.

The app runs full-screen with no browser bar, caches assets for offline use, and updates automatically when new versions are deployed.

---

## Tech Stack

- **Pure HTML/CSS/JS** — zero dependencies, zero build step
- **Progressive Web App** — manifest, service worker, offline caching
- **Claude API** (optional) — for AI-powered hook generation
- **Hosted on Vercel** — automatic deployments from this repo

---

## Project Structure

```
├── index.html        # Full app with sidebar navigation
├── manifest.json     # PWA manifest (app name, icons, display mode)
├── sw.js             # Service worker for offline caching
├── vercel.json       # Vercel routing and header config
├── icon-192.png      # App icon (192x192)
├── icon-512.png      # App icon (512x512)
├── icon.svg          # Vector source icon
└── README.md
```

---

## Local Development

No build tools required. Open `index.html` in a browser.

For service worker testing, serve locally over HTTPS:

```bash
npx serve .
```

---

## Deployment

This repo is connected to Vercel. Every push to `main` triggers an automatic production deployment.

To deploy manually:

```bash
npm i -g vercel
vercel --prod
```

---

## Environment Variables

| Variable | Required | Description |
|---|---|---|
| `ANTHROPIC_API_KEY` | No | Claude API key for AI-powered hook generation. If not set, the Hook Generator uses smart templates instead. Users can also enter their own key in the app. |

---

## Contributing

Contributions welcome. To add a new tool:

1. Fork this repo
2. Add your tool as a new `<div class="page">` section in `index.html`
3. Add a nav button in the sidebar
4. Follow the existing blue/gold design system (CSS variables are in `:root`)
5. Submit a pull request

Please keep tools as pure HTML/CSS/JS with no external dependencies.

---

## Standalone Versions

Individual tools are also available as standalone HTML files for embedding or separate deployment:

- `VAIC_Text_Formatter.html`
- `VAIC_Post_Analyzer.html`
- `VAIC_Hook_Generator.html`
- `VAIC_Hashtag_Analyzer.html`
- `VAIC_Post_Preview.html`

These are maintained separately and may not reflect the latest unified app version.

---

## License

MIT License. Free to use, modify, and distribute.

---

## About

Built by [Dr. Rob Voss](https://www.robvoss.com) at **Voss AI Consulting**.

Rob is an Associate Professor of History at Northwest Missouri State University, OpenAI Academy fellow, and AI consultant specializing in evidence-based training programs with measurable outcomes.

- **Web:** [vossaiconsulting.com](https://www.vossaiconsulting.com)
- **LinkedIn:** [Rob Voss](https://www.linkedin.com/in/ai-robvoss/)
- **Newsletter:** Smart Teaching Evolved

*Transforming Potential into Performance with AI*
