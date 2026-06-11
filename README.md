# CADDIE — Web Deploy (Netlify)

Full-stack golf caddie app: landing page, GPS course detection, OSM auto-download, strokes-gained engine, and AI caddie powered by Claude.

---

## Option A — Drag-and-drop deploy (no account, 2 minutes)

1. Install Node 20+ if you haven't: https://nodejs.org
2. In this folder:
```bash
npm install
npm run build          # produces dist/
```
3. Go to https://app.netlify.com/drop
4. Drag the `dist/` folder onto the page
5. Netlify gives you a live URL immediately (e.g. `https://graceful-caddie-abc123.netlify.app`)

To add your Anthropic API key (needed for AI Caddie):
- In Netlify dashboard → Site configuration → Environment variables
- Add `VITE_ANTHROPIC_API_KEY = sk-ant-...`
- Redeploy (Site → Deploys → Trigger deploy)

---

## Option B — Git-connected deploy (auto-deploy on every push)

1. Push this folder to a GitHub repo
2. Go to https://netlify.com → "Add new site" → "Import from Git"
3. Connect your repo, set build command `npm run build`, publish dir `dist`
4. Add `VITE_ANTHROPIC_API_KEY` in Environment variables
5. Every `git push` auto-deploys

---

## Local development

```bash
npm install
cp .env.example .env          # add your API key
npm run dev                    # http://localhost:5173
```

The dev server has no Overpass/GPS restrictions — all features work immediately.

---

## What works in the web deploy (vs Android)

| Feature | Web (Netlify) | Android (Capacitor) |
|---|---|---|
| GPS nearby course detection | ✅ Browser asks permission | ✅ Native GPS |
| OSM auto-download | ✅ Full Overpass access | ✅ |
| Esri satellite tiles | ✅ | ✅ |
| AI Caddie (Claude) | ✅ Needs API key env var | ✅ |
| Background GPS (screen off) | ❌ Browser limitation | ✅ |
| Offline / cached courses | ✅ Session only (no localStorage) | ✅ Persistent |
| Install to home screen (PWA) | ✅ Browser prompts | ✅ Native APK |

---

## Adding to home screen (PWA-style)

On Android Chrome: tap ⋮ → "Add to Home screen"
On iOS Safari: tap Share → "Add to Home Screen"

This gives a full-screen app experience without the Android build step.

---

## API key security note

`VITE_ANTHROPIC_API_KEY` is embedded in the client bundle — fine for personal use,
visible to anyone who inspects the source. For a public-facing app, proxy the AI
calls through a Netlify Function (serverless) so the key never ships to the browser.
The code is structured for this: `askClaude()` in the engine is a single function
to redirect.
