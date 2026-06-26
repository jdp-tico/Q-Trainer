# THE GLOOM - F3 Q Trainer

A single-file web app for F3 Crown Valley. It spins up a random exercise from the
F3 Exicon and then coaches whoever is leading through the standard F3 Q calling
sequence so any PAX can step up and Q.

## How it is built

- One file: `index.html`. Everything (markup, CSS, data, logic) lives in it.
- Vanilla JavaScript. No framework, no build step, no bundler, no dependencies.
- Renders by rebuilding the `#app` innerHTML on each state change, with click
  handling via event delegation on `#app`.
- Installable to a phone home screen (web app meta tags + inline manifest).

## Hard constraints (do not break these)

- Keep it a single self-contained file with zero build step. The whole point is
  that it stays trivially shareable and hostable on any static host.
- Never use em dashes anywhere, in code comments, copy, or commit messages.
- Stay authentic to F3: use real Exicon exercises and the real Q calling format.
  Do not invent exercises or cadence rules.
- No external runtime dependencies. Google Fonts is the only network call, and the
  app must still work if it fails to load (system font fallback).

## Data model

Exercises live in the `EXERCISES` array. Each entry:

    { name, aka?, count, tags:[...], desc }

`count` is one of:
- "cadence" - Q counts the movements, PAX call the reps
- "hold"    - held for time, no rep count
- "oyo"     - on your own / routine, PAX work at their own pace

`buildScript(ex)` generates the step-by-step leading instructions from `count`.
`COUNT_META` maps each count type to its label and blurb.

## Roadmap (likely next tasks)

1. Expand the Exicon. The full F3 Exicon is ~937 entries; this ships with ~70.
   Source: codex.f3nation.com has an Export CSV button. Wire the full list in,
   keeping the same data shape and assigning a sensible `count` per entry.
2. Crown Valley branding: region/AO name in the header, custom home-screen icon.
3. "Share this exercise" button so a PAX can text a single move to a buddy.
4. "Build a Q" mode: string together a warm-up, cadence sets, a Mary, and a
   finisher into a saved weinke the Q can read top to bottom.
5. Inline the fonts so the app is fully offline at the AO with no signal.

## Deploy

Static hosting. Any of: Netlify (drag index.html onto app.netlify.com/drop),
GitHub Pages, or Cloudflare Pages. Must be served over HTTPS for home-screen
install to behave.
