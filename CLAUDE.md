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
- Three modes, switched by the tabs under the header: Spin (random exercise plus
  a call sheet), Browse (search the Exicon by name or filter by tag, tap through
  to the full detail), and Build a Q (assemble a weinke, saved in localStorage).

## Hard constraints (do not break these)

- Keep it a single self-contained file with zero build step. The whole point is
  that it stays trivially shareable and hostable on any static host.
- Never use em dashes anywhere, in code comments, copy, or commit messages.
- Stay authentic to F3: use real Exicon exercises and the real Q calling format.
  Do not invent exercises or cadence rules.
- No network calls at all. The fonts are inlined as base64 `@font-face` rules so
  the app works fully offline at the AO. System fonts still cover the fallback.

## Data model

Exercises live in the `EXERCISES` array (807 entries). Each entry:

    { name, aka?, count, tags:[...], desc }

`count` is one of:
- "cadence" - Q counts the movements, PAX call the reps
- "hold"    - held for time, no rep count
- "oyo"     - on your own / routine, PAX work at their own pace

`buildScript(ex)` generates the step-by-step leading instructions from `count`.
`COUNT_META` maps each count type to its label and blurb.

`tags` use a fixed vocabulary of 11 categories (the official F3 codex set):
Full Body, Arms, Core, Legs, Routine, Cardio, Coupon, Mary, Run, Music, Warm-Up.

### Provenance of the data

The 807 entries come from two sources, merged at author time (not at runtime):
- The official F3 codex export, `F3-Nation/the-codex` -> `exicon.csv` (780 rows).
  Descriptions were de-mojibaked, the title prefix and trailing tag-words were
  stripped, and `count` was inferred heuristically from keywords. Tags came from
  the CSV where present, else a conservative keyword tagger.
- The original hand-curated staples (Merkin, SSH, Air Squat, Plank, etc.), which
  the official Exicon omits. These keep their hand-written `desc`, `aka`, and
  `count` and are authoritative where names overlap.

Because `count` was inferred for the imported rows, some are approximate. A wrong
`count` only changes which call-sheet variant shows, never the exercise itself.
Hand-correct any that are off.

## Build a Q

`Q_SECTIONS` defines the weinke structure (Warm-Up, The Thang, Mary, Finisher).
The plan is `state.qPlan` (array of `{sec, name, count, note}`), persisted to
localStorage under `gloom_qplan`. The picker re-renders on each keystroke and
restores focus to `#pickerInput` afterward (the app rebuilds innerHTML wholesale).

## Sharing and deep links

`shareExercise` / `shareTextUrl` use the Web Share API with a clipboard fallback.
A shared move links back with `#ex=<name>`; `initFromHash` opens that exercise on
load and on `hashchange`.

## Roadmap (likely next tasks)

Shipped: full Exicon import, Crown Valley header branding, Share an exercise,
Build a Q, inlined offline fonts.

1. Custom home-screen icon for Crown Valley (still the generic "G" glyph).
2. Reorder items within a weinke section (drag, or up/down controls).
3. Save and name multiple weinkes, not just the one working draft.
4. Hand-correct inferred `count` values and thin out the busiest tags.

## Deploy

GitHub Pages via `.github/workflows/deploy.yml` (deploys `index.html` on every
push to `main`). One-time: repo Settings > Pages > Source = "GitHub Actions".
Also works on any static host (Netlify, Cloudflare Pages). Serve over HTTPS so
home-screen install behaves.
