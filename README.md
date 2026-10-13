# Frost presentations

Presentation decks for Frost Design Group, built as single self-contained HTML files —
no build step, no dependencies, no install.

**Start at [`index.html`](index.html)** — it lists every deck, the design system and the archive.

## Decks

| Deck | What it argues |
|---|---|
| [`figma-to-web-deck/`](figma-to-web-deck/) | How the `frost-figma-to-web` conversion plugin improves accuracy — the nine stages, the seven-row gate, where the time goes, and what it still cannot do. 6 slides. |

Open any deck's `index.html` in a browser and present full-screen (`F11`). Arrow keys, space,
`Home` / `End`, click, swipe and mouse wheel all navigate. Print → *Save as PDF* gives one
1920×1080 page per slide.

Each deck is authored on a **fixed 1920×1080 stage** that is scaled as a whole to fit the
window. It letterboxes on an odd-shaped screen rather than reflowing, which is deliberate: what
you see in rehearsal is what the projector gets.

## Design system

[`design/frost/design-system.html`](design/frost/design-system.html) is the source of truth for
every visual value, with a one-page [brand book PDF](design/frost/brand-book-a4.pdf) beside it.
It was extracted from the live frostdesigngroup.com.

The primary **`#0B1464`** is a decision by Frost, not a value extracted from the site — the live
site paints `#0074FF` on its CTAs. It measures 1.24:1 against the near-black canvas, so it is a
surface colour only, never small text.

## Repository layout

```
index.html              entry point — lists everything
figma-to-web-deck/      the current deck (+ its own fonts/ and logo.svg)
design/frost/           design system, brand book, webfonts, logo
md/                     HANDOFF.md and an archived brief
archive/                an older deck in a design system no longer specified here
.claude/skills/         two skills, auto-loaded by Claude Code
CLAUDE.md               architecture notes
```

Each deck carries **its own copy** of the webfonts and logo rather than pointing at
`design/frost/`. That is on purpose — a deck has to be sendable to someone as a single folder
and still render.

## Editing with Claude Code

Start a session in this folder (or a subfolder) and two skills load automatically — nothing to
install. `frontend-slides` owns *how* a deck is built; `frost-deck` owns *what* a slide looks
like. See [`CLAUDE.md`](CLAUDE.md) for the architecture and
[`md/HANDOFF.md`](md/HANDOFF.md) for handing the folder to a person.

## Notices

- **Sailec is a licensed typeface.** The `.woff2` files here are self-hosted for Frost's use.
  Check the licence before reusing them.
- `.claude/skills/frontend-slides/` is **vendored verbatim** from the MIT-licensed
  [`zarazhangrui/frontend-slides`](https://github.com/zarazhangrui/frontend-slides) (commit
  `9906a34`). Its `LICENSE` and `PROVENANCE.md` travel with it; do not hand-edit it.
- The decks' claims are sourced from a separate internal evidence repository. Several claims a
  pitch deck would normally make are excluded on purpose — in particular any end-to-end speed
  win, because that control test has never been run.
