# Puzzle Lab

Offline, self-contained logic puzzles for the commute. No build step, no
dependencies, no tracking. Two games so far:

- **Steady State** — balance a process flow sheet (conservation of mass, mental arithmetic).
- **Kashi** — a Suguru / Tectonic mosaic. Fill each plot 1..n; equal tiles never touch.

Everything runs in the browser from plain HTML/CSS/JS. Once installed as a
Progressive Web App it works with no signal.

---

## Files

```
puzzle-lab/
  index.html                 hub / launcher
  manifest.webmanifest       PWA metadata
  sw.js                      service worker (offline cache)
  icons/                     app icons (192, 512, maskable)
  games/
    steady-state.html        Steady State
    kashi.html               Kashi
```

---

## Run it offline on iPhone (GitHub Pages)

A service worker needs HTTPS, which GitHub Pages gives you for free.

1. **Create the repo and push these files.**
   ```bash
   cd puzzle-lab
   git init
   git add .
   git commit -m "Puzzle Lab"
   git branch -M main
   git remote add origin https://github.com/<you>/puzzle-lab.git
   git push -u origin main
   ```

2. **Turn on Pages.** Repo → Settings → Pages → Source: `Deploy from a branch`
   → Branch: `main` / `/ (root)` → Save. After a minute it gives you a URL like
   `https://<you>.github.io/puzzle-lab/`.

3. **Install on the phone.** Open that URL in **Safari** (must be Safari for the
   home-screen install). Tap **Share → Add to Home Screen**.

4. Launch from the new icon. The first open caches everything; after that it runs
   **fully offline** — straight from the icon, no Safari, no signal.

> Paths are all relative, so it works under the `/puzzle-lab/` subpath without changes.

### Local preview (optional)
Service workers don't run from `file://`, so to test the PWA locally use a tiny
server: `python3 -m http.server` in the folder, then open `http://localhost:8000`.
(The individual game files in `games/` still open fine on a desktop by double-click.)

---

## Add a new game later

1. Drop `games/your-game.html` in (self-contained: inline CSS/JS, no external URLs,
   no `localStorage`).
2. Add a card for it in `index.html` (copy a `<a class="game">` block).
3. Add its path to the `ASSETS` list in `sw.js`.
4. **Bump the cache version** in `sw.js` (`puzzle-lab-v1` → `v2`) so phones pick up
   the change on next online load.

That's it — push, and the new game is cached for offline on everyone's installed copy.

---

## Notes

- No analytics, no network calls, no storage. Refreshing a puzzle just regenerates.
- Each game also works standalone (open the HTML directly) — the PWA shell only adds
  offline install + the launcher.
