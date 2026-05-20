# Open Design Skill — Marketing Website Design

**Date:** 2026-05-20
**Status:** Approved (content brief)
**Output:** `index.html` at project root (single self-contained file)
**Audience:** Developers using AI coding agents (Claude Code, Codex, Cursor, Gemini CLI)

## 1. Goal

A single-page marketing landing page for the `open-design-skill` repo
(github.com/sugarforever/open-design-skill). Visitors arrive from the
repo README, a tweet, or a peer's recommendation and should leave with:

1. A one-sentence understanding of what the skill does.
2. The install command in their clipboard.
3. A sense of the catalog's depth (150+ systems, 110+ templates).
4. Confidence that this is not a maintenance liability (MIT, Node, no daemon).

## 2. Design system

**Template:** `design-templates/kami-landing` (Open Design catalog).
**Design system:** none picked — `kami-landing` declares
`od.design_system.requires: false` and ships its own baked-in style
(kami / 紙 / 纸: warm parchment canvas, ink-blue accent, serif at one
weight, four warm grays, no italic, no cool tones).

**Craft references opted in by the template:** `typographic-rhythm`,
`pixel-discipline`. Neither file exists under `craft/` in the current
OD checkout. The kami `DESIGN.md` already encodes the same constraints
(line-heights 1.10–1.55, tabular-nums, solid-hex tags, 1px rings) so
the missing files are not blocking — kami `DESIGN.md` is authoritative.

**Authority order (per Open Design skill Phase 3):**
1. kami `DESIGN.md` — tokens, type rules, ten invariants
2. craft `*.md` — universal rules (covered by `DESIGN.md` here)
3. kami-landing `SKILL.md` body — page structure + self-check list

## 3. Page structure (kami canonical, six bands)

```
1. Eyebrow row     — locale meta · edition · version (12px sans uppercase)
2. Hero            — display headline (96–106px serif 500), tagline,
                     three hero-token chips
3. Manifesto       — pull paragraph, serif 400, 20px, ink-blue left rule,
                     signature footer
4. Metrics row     — 5 cells (value · label · sub)
5. Chapters        — 5 numbered chapters
6. Footer          — kicker word, license · year · contact, 3-column index
```

Tag every editable element with `data-od-id="<unique-slug>"` per the
template contract.

## 4. Content brief

### 4.1 Brand identity

| Field | Value |
|---|---|
| Name | Open Design Skill |
| Tagline | Bring the Open Design catalogue into any agent session |
| Edition · Version | v1 · Edition 2026 |
| Primary URL | github.com/sugarforever/open-design-skill |
| Language stack | `en` |

### 4.2 Hero

- **Eyebrow:** `AGENT SKILL · v1 · EDITION 2026 · MIT`
- **Headline:** *Design systems for your agent.* (5 words, fits ≤ 6 limit)
- **Tagline:** *150+ brand systems, 110+ rendering templates, 130+ functional skills, 11 craft references — composed at agent runtime, no daemon required.*
- **Meta chips (3):**
  - `Claude Code · Codex · Cursor`
  - `Node 16+`
  - `npx skills add`

### 4.3 Manifesto

> Design systems should live where the work happens — inside the agent
> writing your decks, your landing pages, your dashboards. Open Design
> Skill brings the full Open Design catalogue into any agent session.
> Pick a brand. Pick a template. Layer the universal craft rules. The
> same prompt composition Open Design's daemon does — without needing
> the daemon to run.

Signature: *Maintained by sugarforever · 2026*

### 4.4 Metrics (5 tiles)

| Value | Label | Sub |
|---|---|---|
| 150+ | design systems | DESIGN.md per brand |
| 110+ | rendering templates | decks · dashboards · pages |
| 130+ | functional skills | briefs · audits · copy |
| 11 | craft references | typography · color · a11y |
| 0 | daemons required | plain Node + git |

### 4.5 Chapters (5 numbered)

**01 · The pitch.** What the skill is, who it's for, why it exists.
One paragraph followed by a dash-list of trigger phrases ("make me a
deck", "apply BMW brand", "dashboard with Stripe", "set up a design
system"). Closing line: "If you have an agent and an idea, this is
the missing layer."

**02 · How it composes.** The three phases. Setup checks
`$OPEN_DESIGN_ROOT`. Bind writes `.open-design.json`. Compose reads
DESIGN.md + craft + template SKILL.md in that authority order, then
executes the template's workflow against the brief. A compact code
block shows the bound JSON. Closing line: "It is a runbook, not a
runtime."

**03 · Install in one line.** Primary path: `npx skills add
sugarforever/open-design-skill`. Secondary: manual `git clone` into
`~/.claude/skills/open-design`. One-time content clone:
`git clone https://github.com/nexu-io/open-design ~/.open-design-skill/repo`,
or `OPEN_DESIGN_ROOT=/your/path` if already checked out elsewhere.

**04 · Use it without thinking.** Five example prompts laid out as a
two-column list:
- "Make me a pitch deck for my seed round."
- "Build a SaaS landing page with the Stripe design system."
- "Apply the BMW brand to my homepage."
- "Set up a design system for my project."
- "Use Open Design to build a dashboard."

Closing paragraph: "Commit `.open-design.json` to share design
choices across collaborators. Delete it to re-pick. The skill never
copies template bodies into your repo — content stays in
`$OPEN_DESIGN_ROOT`, so `git pull` in the clone takes effect on the
next agent turn."

**05 · Where the line is drawn.** Explicit non-goals — preview
iframe, comment-mode surgical edits, slider parameters, `od.mode`
routing. These live in Open Design's daemon and are out of scope for
this skill. For preview/debug, the agent's own dev-server +
chrome-devtools MCP / playwright are the right tools. Closing line:
"This skill is Stage 1: content. The daemon is Stage 2: surface."

### 4.6 Footer

- **License · Year · Contact:** `MIT · 2026 · github.com/sugarforever`
- **Footer kicker word:** `kami.` (mega serif 500 — sets the tone, ties to the design system)
- **Three-column site index:**
  - **Project:** README · SKILL.md · Issues
  - **Open Design:** Upstream repo · DESIGN.md catalog · Templates
  - **Tools:** Vercel `skills` CLI · Claude Code · Codex

## 5. Visual constraints (from kami `DESIGN.md` — non-negotiable)

- Page background `#f5f4ed` (parchment) — never `#ffffff`.
- Ink-blue `#1B365D` covers ≤ 5% of visible surface.
- All grays warm (R ≈ G > B). No `slate-*`, no `#f3f4f6`.
- Serif weight locked at 500. No `font-weight: 700`/`900` on serif.
- No `font-style: italic` anywhere. Emphasis = ink-blue color or `.tag`.
- All numeric stacks (metric values, dates) use `font-variant-numeric: tabular-nums`.
- Tag fills are solid hex (`#E4ECF5` etc.), never `rgba()`.
- Shadows: only 1px rings or `0 4px 24px rgba(0,0,0,0.05)`. No hard drop shadows.
- Headline ≤ 6 words at display size.
- Responsive collapse to one column at 768px and 560px, no horizontal scroll.

## 6. Output

Single file at project root: `index.html`. Self-contained, all CSS
inline, zero JS, Google Fonts via `<link rel="preconnect">` +
`<link rel="stylesheet">` (Source Serif 4 + Source Sans 3 +
JetBrains Mono, matching the example).

No build step. Open `index.html` in a browser; deploy by serving the
file (Vercel/Netlify/GitHub Pages — any static host).

## 7. Verification

After write, verify against the kami self-check list (§4 of the
template SKILL.md, replicated in §5 above). Open in browser, sanity
check:

1. Parchment background, ink-blue used sparingly.
2. Layout collapses cleanly at 768px and 560px.
3. Tabular-nums alignment on metric values.
4. No italic, no synthetic bold.
5. Hero headline fits one line at desktop width.

## 8. Out of scope

- No multi-page docs site.
- No catalog gallery (would warrant its own template, e.g. a custom mode).
- No interactive demo (would need JS — kami forbids motion).
- No analytics, cookies, or third-party tracking.
- No backend, no API.
