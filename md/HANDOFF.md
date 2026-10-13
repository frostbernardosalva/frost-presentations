# Handoff — Figma to Web deck

This folder is self-contained. Copy or clone it anywhere and everything needed to **view**,
**present**, **export** and **edit** the deck travels with it.

## Start here

Open **`index.html`** in a browser. It is a one-page index of every presentation in
this folder, with links to the design system and the archive — start there if you are not sure
what you are looking at.

## View the deck

Open `figma-to-web-deck\index.html` in a browser, or click it from the index. No server, no build, no install — it is a single
HTML file, with the Sailec webfonts and the logo beside it in `figma-to-web-deck\fonts\` and
`figma-to-web-deck\logo.svg`.

| | |
|---|---|
| Next / previous | Arrow keys, `Space`, `PageUp` / `PageDown`, mouse wheel, swipe |
| Jump | `Home` (first), `End` (last), or the dots at the bottom |
| Advance by clicking | Click anywhere on the slide |

6 slides. The slide canvas is a fixed 1920x1080 stage scaled to fit the window — it letterboxes
on an odd-shaped screen rather than rearranging itself, which is deliberate: what you see in
rehearsal is what the projector gets.

**Presenting:** full-screen the browser (`F11`). The fonts are local files, so nothing needs a
network connection.

Slide 1 is animated — the page assembles itself onto the design frame over about four seconds.
Press `Home` to replay it from the start before you begin.

## Export a PDF

Easiest: print the page from the browser and choose *Save as PDF* — the deck has print rules
that give one slide per page with the on-screen controls hidden.

For a higher-fidelity, image-per-page export there is a bundled script:

```
bash .claude/skills/frontend-slides/scripts/export-pdf.sh figma-to-web-deck/index.html
```

It needs Node (`npx`), and on first run downloads Playwright and Chromium into a temp
directory — so it wants a few minutes and a network connection. Animations do not survive; the
output is a rasterised page per slide.

## Edit it with Claude Code

**Start the session in this folder** (`presentation\`) or a subfolder of it. Two skills live in
`.claude\skills\` and load automatically — there is nothing to install:

| Skill | Owns |
|---|---|
| `frontend-slides` | How a deck is built: the fixed 16:9 stage, the base CSS, PDF export. A vendored copy of an MIT-licensed open-source skill — see its `PROVENANCE.md`. Do not hand-edit it. |
| `frost-deck` | How a slide looks: the Frost tokens, the surface pacing, the layout traps, and the rules about what the deck may claim. Written for this folder. |

If you start Claude Code somewhere else and point it here, the skills will not load. If you add
a skill mid-session, `/reload-skills` picks it up.

`CLAUDE.md` is also read automatically and explains the architecture in more detail.
`design\frost\design-system.html` is the design spec everything visual derives from — if you
change a brand value, change it there **and** in the deck's `:root` block, which carries its own
copy so the deck can travel alone.

## When you share this on

`.claude\` starts with a dot but is **not** a hidden folder on Windows, so Explorer's
*Send to → Compressed (zipped) folder* includes it. If you move the folder with a tool that
skips dot-directories, check that `.claude\skills\` arrived — without it the next person gets
the deck but none of the instructions.

Note that **Sailec is a licensed typeface**, self-hosted here for internal use. Check the
licence before publishing any of this publicly.

## What is in here

| Path | What |
|---|---|
| `figma-to-web-deck\` | **The deck.** 6 slides on conversion accuracy, in the Frost brand. Open `index.html`. |
| `design\frost\` | Frost design system + one-page brand book PDF, extracted from the live site, plus the webfonts and logo. |
| `md\HANDOFF.md` | This file. |
| `md\archived-14-slide-deck-prompt.md` | The brief an earlier, longer 14-slide deck was written to. That deck has been deleted — this is not the brief for the deck above. Reference only. |
| `CLAUDE.md` | Architecture notes, read automatically by Claude Code. |
| `.claude\skills\` | The two skills described above. |
| `index.html` | **The index.** A one-page list of every presentation, plus the design system and archive. A normal web page, not a deck. |
| `archive\`, `.frontend-slides\` | **Archive.** An older deck and three style studies in a different design system, whose spec is no longer in this folder. They still open; don't edit them against the Frost system. |

## One thing to know about the content

The deck's claims are sourced from a separate evidence repository, and several claims a pitch
deck would normally make are **excluded on purpose** because the plugin's own documentation
forbids them — in particular any end-to-end speed win, or beating a careful manual conversion
(that comparison has never actually been run). Timings are presented as *where the time went*,
and one slide is devoted to what the tool still cannot do.

If you edit the words, do not add a number that is not attested in a source file, and do not
soften the limitations slide — it is what makes the rest credible to a senior reviewer.
