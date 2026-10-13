# Provenance — vendored copy, not written here

This skill is a **verbatim copy** of a third-party open-source skill. It was vendored into this
folder so the project is self-contained: anyone who opens a Claude Code session at the
`presentation\` root gets the skill with no marketplace, install, or network step.

| | |
|---|---|
| Upstream | https://github.com/zarazhangrui/frontend-slides |
| Commit | `9906a34d640d2111f724544cbc50f7f130569ae1` |
| Commit date | 2026-06-23 |
| Vendored | 2026-10-12 |
| Licence | MIT — see `LICENSE`, © 2025 Zara Zhang |
| Bundled template pack | `bold-template-pack/`, 34 templates, upstream `zarazhangrui/beautiful-html-templates` (also MIT) |

The files are byte-for-byte upstream. `SKILL.md` (28,187 bytes), `viewport-base.css`,
`html-template.md`, `animation-patterns.md` and `STYLE_PRESETS.md` were checksum-matched against
the copy that the `figma-to-web-deck` was actually built from, so nothing shifted underneath the
finished deck. Line endings are **LF**, deliberately — CRLF breaks the bash scripts in
`scripts/`.

## Do not hand-edit these files

Project-specific rules belong in the sibling `frost-deck` skill or in `..\..\..\CLAUDE.md`.
Editing this copy makes it silently diverge from upstream with no way to tell what changed.

## What was intentionally left out

| Omitted | Why |
|---|---|
| `.claude-plugin/` and `plugins/` | Upstream also ships as an installable plugin. A `.claude-plugin/plugin.json` inside `.claude/skills/` would make this load as a **plugin** instead of a skill, which forces the workspace-trust dialog on anyone who opens the folder. |
| Repo-level `README.md` (35 kb) | Install and marketing docs for the repo; irrelevant inside a skill. Read it upstream. |

Nothing in `SKILL.md` references either omission — all 9 of its internal links were verified to
resolve inside this directory.

## If you would rather install it properly

Upstream is a marketplace, so instead of using this copy you can run:

```
/plugin marketplace add zarazhangrui/frontend-slides
/plugin install frontend-slides
```

Then delete this directory. The installed version updates; this copy does not.

## A note on the bundled scripts

`scripts/export-pdf.sh` and `scripts/deploy.sh` are **not** pre-authorised — this skill
declares no `allowed-tools`, so Claude asks before running anything. `export-pdf.sh` needs
Node/`npx` and downloads Playwright plus Chromium into a temp dir on first run;
`deploy.sh` publishes to Vercel. Read either before approving it.
