---
name: frost-deck
description: Build, edit or restyle slides in the Frost Design Group system - navy #0B1464, night black, Sailec - on a fixed 1920x1080 stage. Use when adding a slide, changing a deck's look, or starting a new deck in this project, so the output matches design/frost/design-system.html instead of inventing a new style.
---

# Decks in this project

The deliverable is `figma-to-web-deck/` — 6 slides on the conversion accuracy of the
`frost-figma-to-web` plugin, built in the **Frost Design Group** brand.

> **A note on history.** This folder used to hold a second design system (the Claude marketing
> surface: cream, coral, EB Garamond) specified in a root `DESIGN.md`. That spec and its two
> main decks have been deleted. `archive/claude-system-deck.html` and
> `.frontend-slides/slide-previews/` are what survive of it — archive, read-only, and **not**
> governed by this skill. Nothing below applies to them.
>
> **`index.html` at the root is not a deck either.** It is the responsive landing page that lists
> the presentations. It uses the Frost tokens, but none of the fixed-stage rules or the
> verification checklist below apply to it — media queries are correct there, not a defect.

Two skills apply here and they do not overlap:

- **`frontend-slides`** (vendored, sibling directory) owns the **mechanics** — the fixed 16:9
  stage, `viewport-base.css`, the `SlidePresentation` class, PDF export. Follow it for *how* a
  deck is built.
- **This skill** owns the **look** — the Frost system. Follow it for *what* a slide looks like.

Where they conflict, this skill wins on visual decisions and `frontend-slides` wins on
structure.

Do not run `frontend-slides` Phases 0-2 (style exploration) for a deck in this project. The
style is already chosen and documented. Start at its Phase 3.

## The source of truth

`design/frost/design-system.html` — extracted from the live frostdesigngroup.com with the
`robonuggets/design-system` toolkit. `design/frost/brand-book-a4.pdf` is the one-page version.
Read it before changing any visual value.

Two things about the primary are easy to get wrong:

- **`#0B1464` is a decision by Frost, not an extracted value.** The live site paints `#0074FF`
  on its CTAs, and the site's variable set *declares* a third blue, `#0569FF`. All three are
  recorded distinctly in the spec; only `#0B1464` is normative. Do not "correct" it to the
  painted value.
- It measures **1.24:1** on the `#040808` canvas, so it is a **surface colour only** — never
  small text. White on the navy is 16.2:1.

Design principles are deliberately **absent**. They were not extracted, and the toolkit's rule
is "never invent brand details; better to skip a section than fill it with fiction." Do not add
them.

## Tokens — the shipped values

The `:root` block in `figma-to-web-deck/index.html` is the only place a literal hex belongs;
everything downstream uses `var(--…)`.

```
--navy      #0b1464    --canvas  #040808    --black #000000
--white     #ffffff    --tint    #daebfa    --muted #8fa0ab
--hairline  #212629    --rule-navy #1b2478
--pass      #5bceff    --wait    #f0b45a    --fail  #ef5466
```

The three status colours carry consistent meaning across the deck — keep it that way:
`--pass` is building / passed, `--wait` is waiting on a human / rework / unfinished, `--fail`
is the gate's reject loop. The chart slide and the stage rail both rely on this reading.

Type is **Sailec**, self-hosted from the deck's own `fonts/` (Thin 100 / Light 300 / Regular 400
/ Medium 500 / Bold 700 / Regular Italic). It is a **licensed typeface** — internal use here;
check the licence before publishing publicly. Radius ladder: `6 / 8 / 12 / 45px (pill)`.

## Non-negotiables

- **Three surface tones only** — navy, canvas, black. No fourth.
- **No two consecutive slides in the same surface mode.** The alternation *is* the pacing.
  Shipped sequence: `navy · canvas · black · navy · canvas · navy`. Inserting or removing a
  slide means re-checking the whole sequence, not just appending.
- **The navy never carries small text.** Accent text on a dark surface is `--tint`.
- `--fail` is scarce: the gate's reject loop and the limits dots, nothing else.

## Layout

`.inner { position: absolute; inset: 0; padding: 148px 132px 96px; }` — a flex column, centred.
That leaves a **content box of 836px**. Overrun is the single most common defect; measure it.

The generous top padding exists to clear the fixed brand bar. **A bounds check will not catch
that overlap** — centred content on a dense slide rides up underneath the bar while still being
"in bounds". Check leaf text rects against the `.brandbar` rect specifically.

Reveal animation: children marked `.reveal` start at `opacity: 0; translateY(…)` and transition
when the slide gets `.visible`, staggered `.06 / .17 / .28 / .39 / .50 / .61`. Only
**direct-child** position drives the stagger — nesting a `.reveal` deeper gets it no delay.

## Layout traps that have each cost real time

- **Give every headline a per-slide `max-width` in `ch`.** A shared value orphans a word on some
  slides ("wrong.", "yet." alone on a line). Tune per slide.
- **`minmax(0, 1fr)`, not `1fr`,** for grid columns whose children might not wrap. A
  `white-space: nowrap` tag forced its column open and knocked a nine-card rail out of
  alignment.
- **`flex: none` on a nested column that must keep its height.** A flex item collapsed to 33px
  and every child computed to width 0 — the element rendered, but empty.
- **`box-sizing: border-box` on percentage-width flex children that have padding.** Otherwise
  flex-shrink pulls each under its stated percentage; a bar labelled 86% painted 85.0%.
- **Grep a class name before and after moving markup between slides.** `.win` once styled both
  a browser mock and the improvement cards, drawing stray dark boxes on an unrelated slide.
- **Optical spacing is not nominal spacing.** A 112px headline carries ~18px of leading above
  its caps and a descender ~21px below its box, so an even gap above and below paints as 53/23.
  Measure the painted ink (canvas `TextMetrics.actualBoundingBoxAscent`), then set margins to
  the optical target and leave a comment saying why they look uneven.

## Verifying a change

Screenshot **every** slide and look at it. `scrollHeight` alone misses panel overlap, which is
the failure mode that actually ships. Then confirm:

- no slide overflows its 1920×1080 bounds, and no leaf text intersects the brand bar;
- no container has `scrollHeight > clientHeight` with non-visible overflow;
- content height per slide is under **836px**;
- only `print` and `prefers-reduced-motion` media queries exist — anything else means content
  reflows per viewport, which breaks the fixed-stage rule;
- the surface sequence still alternates with no consecutive repeats;
- every Sailec weight loads; no `<img>` is broken; every path is **relative** (base64 breaks
  PDF export and deploy);
- the console is clean apart from `[deck] slide N of M`.

**Two harness quirks when driving this with browser automation**, both of which will waste your
time if you don't know them:

1. The first screenshot after a JS slide change returns a **stale frame**. Capture twice.
2. A background tab **freezes CSS transitions**, and a stuck transition overrides even inline
   `!important` — so width-animated elements measure at their start value (0) and no amount of
   style-setting moves them. Check `document.hidden`; disable transitions before measuring, or
   drive `getAnimations()` directly to sample a frame.

Avoid `alert`/`confirm`/`prompt` in deck scripts — they block browser automation. These decks
log `[deck] slide N of M` to the console instead, deliberately, so navigation can be verified.

## If you are editing the deck's words

Claims are sourced from a separate evidence repo (`frost-workflow-testing-center`, a sibling of
`presentation/`). **Never modify the originals there.** Several obvious-sounding claims are
**forbidden by the plugin's own documentation** — notably any end-to-end speed win, or beating a
manual conversion, because **that control test has never been run.**

The deck presents timings as *where the time went*, not as a speed claim, and devotes a slide to
what the tool still cannot do.

- Do not add a number that is not attested in a source file.
- Do not soften the limitations slide — it is what makes the rest credible to a senior reviewer.
- Two caveats must survive any rewrite: *"never been run"* on the chart slide and *"Never run"*
  in the status matrix.
- Recompute derived figures from the raw timestamps rather than trusting a generated summary —
  the timing files have `paused` intervals that must be subtracted, and one run's wall clock
  spans six days for 2h 18m of work.
