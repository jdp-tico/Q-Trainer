# THE GLOOM

F3 Crown Valley Q Trainer. A single self-contained web app, no build step.

- **Spin** a random move from 807 real F3 Exicon exercises, then get a step-by-step
  call sheet so any PAX can step up and Q it.
- **Build a Q**: string together a warm-up, the thang, a Mary, and a finisher into
  a weinke you can read top to bottom. It saves on your device.
- **Share** any move (or a built weinke) by text or link. A shared link opens
  straight to that exercise.
- Works fully offline at the AO. Fonts are inlined; there are zero network calls.

## Run it

Open `index.html` in a browser. That is the whole app.

## Host it

It is wired for **GitHub Pages**: pushing to `main` runs `.github/workflows/deploy.yml`
and publishes the app. One-time setup in the repo:

> Settings > Pages > Build and deployment > Source = **GitHub Actions**

After that, every push to `main` deploys to your `*.github.io` URL (or a custom
domain). It also works on any static host (Netlify, Cloudflare Pages). Serve over
HTTPS so "Add to Home Screen" installs it like an app.

## Develop it

Built as a single vanilla-JS HTML file, no dependencies. See `CLAUDE.md` for the
data model, the provenance of the exercise data, and the roadmap.
