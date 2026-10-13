# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A no-build, no-dependency presentation project. Every deliverable is a single self-contained
`.html` file: inline `<style>`, inline `<script>`, inline SVG, fonts served from a sibling
`fonts/` directory. There is no `package.json`, no bundler, no test suite, and the directory is
not a git repo.

`index.html` at the root is the **entry point** — a landing page listing every presentation.
The current deliverable is **`figma-to-web-deck/`**, a 6-slide deck about the conversion accuracy
of the `frost-figma-to-web` plugin, built in the **Frost Design Group** brand. Everything else in
the folder either supports those (`design/frost/`, `.claude/skills/`, `md/`) or is archive.

More decks are expected to land beside `figma-to-web-deck/`. Adding one means creating its
folder and adding a card to `index.html` — see *The entry point* below.

## Running / previewing

Open a file directly in the browser — no server needed:

```powershell
start index.html                      # the entry point - links to everything
start figma-to-web-deck\index.html
start design\frost\design-system.html
```

Deck controls: arrow keys / space / PageUp-PageDown, Home, End, click anywhere to advance, dot
buttons, touch swipe, mouse wheel. The deck logs `[deck] slide N of 6` to the console on every
transition — useful for verifying navigation via browser automation.

Export to PDF: `figma-to-web-deck/index.html` has print rules (one 1920×1080 page per slide, chrome
hidden) — use the browser's Print → Save as PDF. For a rasterised image-per-page export instead,
the vendored skill ships a script:

```powershell
bash .claude/skills/frontend-slides/scripts/export-pdf.sh figma-to-web-deck/index.html
```

It needs Node (`npx`) and downloads Playwright + Chromium to a temp dir on first run.

## Files

| Path | Role |
|---|---|
| `figma-to-web-deck/index.html` | **The deliverable.** 6 slides on a 1920×1080 stage, Frost brand. Self-contained: its own `fonts/` and `logo.svg`. |
| `design/frost/` | **Frost Design Group design system**, extracted from frostdesigngroup.com: `design-system.html` (the spec), `brand-book-a4.pdf` (one-page reference), `fonts/`, `logo.svg`. |
| `md/HANDOFF.md` | Human-facing note for whoever receives this folder: how to view, present, export, edit. |
| `md/archived-14-slide-deck-prompt.md` | The brief an earlier 14-slide deck was written to. That deck has been deleted; this is **not** the brief for the current `figma-to-web-deck/`. Kept for reference. |
| `.claude/skills/` | Two skills, auto-loaded. See *Skills in this folder* below. |
| `index.html` | **Entry point.** A responsive landing page listing every deck, plus the design system and the archive. Not a deck — see *The entry point* below. |
| `archive/` | **Archive.** `claude-system-deck.html`, a 3-slide 1280×720 deck in the old Claude system — see *Archive* below. |
| `.frontend-slides/slide-previews/` | **Archive.** Three typographic directions explored for that old system. Disposable. |

The `design/frost/` nesting is deliberate: the design system is filed under the brand it belongs
to, leaving room for a second brand beside it later. Every asset reference inside that folder is
relative to the folder itself, so it moves as a unit.

`figma-to-web-deck/` carries its **own copy** of the Sailec woff2 files and the logo rather than
pointing at `design/frost/`. That is on purpose — the deck is meant to be sent to someone as a
single folder and still render. Changing a brand value means changing it in `design/frost/` *and*
in the deck's `:root`.

## Archive — the old Claude design system

This folder used to hold a second design system: the Claude marketing surface (cream `#faf9f5`,
coral `#cc785c`, EB Garamond + Inter), specified in a root `DESIGN.md`, with three decks built in
it. **`DESIGN.md`, `skill-deck/` and `figma-to-web-deck/` have been deleted.**

What survives of it is `archive/claude-system-deck.html` and `.frontend-slides/slide-previews/`. Both
still open and render — they are self-contained and load their fonts from the Google Fonts CDN —
but **the spec they were built against no longer exists in this folder.** Treat them as read-only
artefacts. Do not try to reconcile them with `design/frost/`; they are a different system, not an
out-of-date version of this one.

If you are asked to revive that system, the values are recoverable from
`archive/claude-system-deck.html`'s own `:root` block, not from any spec file still present.

## The entry point

`index.html` at the root is **a normal responsive web page, not a deck.** It has no fixed stage,
no `SlidePresentation`, and it is the one file here where media queries beyond `print` and
`prefers-reduced-motion` are correct rather than a bug. Do not copy the deck stage rules into it,
and do not apply the deck verification checklist to it.

It is built in the Frost tokens and loads its fonts from `design/frost/fonts/` rather than
carrying a copy. That asymmetry is deliberate: a **deck** must be sendable on its own, so it
duplicates the fonts; the **index** is only a list of links into this folder and cannot travel
alone anyway.

**To add a presentation:** create the deck folder, then copy one `<a class="deck">` block in the
*Decks* section of `index.html` and edit five fields — `href`, the status chip (`tag live` or
`tag draft`), the title, the one-sentence description, and the `.deck-meta` line. The grid
reflows on its own. A commented-out template block sits directly under the existing card.
Newest deck first.

## The Frost design system

`design/frost/design-system.html` is the source of truth for every visual value in
`figma-to-web-deck/`. Tokens as shipped:

```
--navy      #0b1464   brand primary, surfaces and fills
--canvas    #040808   night black, the second surface
--black     #000000   the third surface
--tint      #daebfa   small accent text
--muted     #8fa0ab
--rule-navy #1b2478   hairlines on navy
--pass      #5bceff   "building", things that passed
--wait      #f0b45a   waiting on a human, rework, unfinished
--fail      #ef5466   failure, the gate's reject loop
```

Two things about the primary that are easy to get wrong:

- **`#0B1464` is a decision by Frost, not an extracted value.** The live site paints `#0074FF`
  on its CTAs, and its variable set *declares* a third blue, `#0569FF`. All three are recorded
  distinctly in `design-system.html`; only `#0B1464` is normative.
- It measures **1.24:1** against the `#040808` canvas, so it is a **surface colour only**. Small
  accent text uses the tint `#daebfa` instead. White on the navy is 16.2:1.

Sailec is a **licensed typeface**, self-hosted here for internal use. Check the licence before
publishing any of this publicly.

Design principles are deliberately **absent** from the spec — they were not extracted, and the
extraction toolkit's rule is "never invent brand details; better to skip a section than fill it
with fiction." Do not add them.

## Deck architecture

**Fixed stage, scaled as a whole.** Slides are authored at 1920×1080 and the entire stage is
transformed to fit the window — content never reflows per viewport. This is why layout can use
absolute pixel values freely. `setupStageScale()` computes
`Math.min(innerWidth/1920, innerHeight/1080)` and writes
`transform: translate(x,y) scale(f)` onto the stage with `transform-origin: 0 0`.

The leading comment block (`FIXED 16:9 STAGE: MANDATORY BASE STYLES`, 8 numbered rules) is
boilerplate meant to be copied verbatim into any new deck — keep it intact and add deck-specific
CSS below it.

**Slide switching** uses `visibility` + `opacity` + `pointer-events` toggled by `.active` /
`.visible` — never `display`, which would kill the reveal transitions. `class="slide"` is
mandatory: the PDF export script queries `.slide`.

**The content box is 836px** (1080 − 148 top padding − 96 bottom). `.inner` carries
`padding: 148px 132px 96px`; the generous top value exists to clear the fixed brand bar, and
**a bounds check will not catch that overlap** — centred content on a dense slide rides up
underneath the bar while still being "in bounds". Check leaf text rects against the `.brandbar`
rect specifically.

**Surface pacing.** One surface mode per slide, and no two consecutive slides may share one. The
shipped sequence is `navy · canvas · black · navy · canvas · navy`. Inserting or removing a slide
means re-checking the whole sequence, not just appending.

**Reveal animation.** Children marked `.reveal` start at `opacity: 0; translateY(…)` and animate
in when the slide gets `.visible`, staggered by `:nth-child(1..6)` transition delays
(`.06 / .17 / .28 / .39 / .50 / .61`). Only direct-child position drives the delay — nesting a
`.reveal` deeper gets it no stagger. A `prefers-reduced-motion` block near the top collapses all
of it.

## Constraints worth knowing before editing

- **Only two media queries may exist** — `print` and `prefers-reduced-motion`. Any other means
  content reflows per viewport, which breaks the fixed-stage rule.
- **Images are relative paths, never base64** — base64 breaks both PDF export and deploy.
- **Avoid `alert`/`confirm`/`prompt`** in deck scripts — they block browser automation. The deck
  uses `console.log` for state, deliberately.
- **Measure ink, not boxes, for optical spacing.** Slide 1's eyebrow/headline/lede margins are
  deliberately uneven (`7px` / `42px`) because the 112px headline carries 18px of leading above
  its caps and a 21px descender below its box. An even gap *paints* as 53px above and 23px below.
  There is a comment on the rule saying so; don't "tidy" it back to a single value.
- **Percentage-width flex children need `box-sizing: border-box`** if they also have padding, or
  flex-shrink pulls each one under its stated percentage. This bit the slide-5 bar chart: 86%
  painted as 85.0%.
- **Check for class collisions when moving markup between slides.** `.win` once styled both a
  browser mock and the improvement cards, which drew stray dark boxes. Grep a class name before
  and after you move anything that uses it.

## Editing the deck's words

The deck's claims are sourced from a separate evidence repository
(`frost-workflow-testing-center`, a sibling of this folder). **Its originals are never
modified.** Several claims a pitch deck would normally make are excluded on purpose because the
plugin's own documentation forbids them — in particular any end-to-end speed win, or beating a
careful manual conversion, because **that control test has never been run.**

Timings are presented as *where the time went*, not as a speed claim, and one slide is devoted
to what the tool still cannot do. Rules for edits:

- Do not add a number that is not attested in a source file.
- Do not soften the limitations slide — it is what makes the rest credible to a senior reviewer.
- Both caveats must survive any rewrite: *"never been run"* on the chart slide and *"Never run"*
  in the status matrix.

## Skills in this folder

`.claude/skills/` holds two skills. They load automatically for any session started here or in a
subfolder — nothing is installed, and neither declares `allowed-tools`, so no tool is
pre-authorised.

| Skill | Owns | Notes |
|---|---|---|
| `frontend-slides` | Mechanics: the fixed 16:9 stage, `viewport-base.css`, the `SlidePresentation` class, PDF export, PPTX conversion, 34 bold templates. | **Vendored verbatim** from the MIT-licensed `zarazhangrui/frontend-slides` (commit `9906a34`). Do not hand-edit — see its `PROVENANCE.md`. |
| `frost-deck` | The look: the Frost tokens, surface pacing, the layout traps, and the sourcing rules above. | Written for this folder. Put project-specific rules here, not in the vendored skill. |

Division of labour: `frontend-slides` decides *how* a deck is built, `frost-deck` decides *what*
a slide looks like.

Because the style is already settled, skip `frontend-slides` Phases 0–2 (style exploration) for
decks in this project and start at its Phase 3.

The skill was vendored rather than installed so the folder can be handed to someone else intact.
If you would rather install it properly, `PROVENANCE.md` gives the two commands and you can
delete the directory.
