# Open Design Skill Website — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Ship a single-page marketing landing for `open-design-skill` at the project root (`index.html`), built via the `kami-landing` template from Open Design.

**Architecture:** Static HTML, all CSS inline, zero JS. Scaffold from the canonical kami example (`$OPEN_DESIGN_ROOT/design-templates/kami-landing/example.html`), preserve all kami CSS tokens, swap in the project-specific content from the design spec. Two structural tweaks needed: `.metrics` grid goes 4→5 columns, chapter list extends 3→5.

**Tech Stack:** HTML5, inline CSS, Google Fonts (Source Serif 4 · Source Sans 3 · JetBrains Mono). No build step, no JS runtime. Verification via grep-based kami invariant checks + browser visual sweep using `chrome-devtools` MCP.

**Spec:** `docs/superpowers/specs/2026-05-20-open-design-skill-website-design.md`.

**Reference files (read-only, do not edit):**
- `$OPEN_DESIGN_ROOT/design-templates/kami-landing/SKILL.md` — template contract
- `$OPEN_DESIGN_ROOT/design-templates/kami-landing/example.html` — visual reference
- `$OPEN_DESIGN_ROOT/design-systems/kami/DESIGN.md` — token + invariant source of truth

`$OPEN_DESIGN_ROOT` resolves to `~/.open-design-skill/repo` (already cloned).

---

## File Structure

| Path | Status | Responsibility |
|---|---|---|
| `.open-design.json` | create | Per-project binding (Phase 2e of open-design skill). Records template + designSystem choice. |
| `index.html` | create | The full marketing page. Self-contained, ~700 lines. |
| `docs/superpowers/specs/2026-05-20-open-design-skill-website-design.md` | exists | Read-only — the approved spec. |

No other files. No JS, no CSS file, no images (font files are loaded from Google Fonts CDN at runtime). One artifact.

---

### Task 1: Write `.open-design.json` binding

**Files:**
- Create: `.open-design.json`

This is Phase 2e of the open-design skill — the per-project binding that lets subsequent agent turns short-circuit the pick flow.

- [ ] **Step 1: Write the binding file**

```json
{
  "version": 1,
  "designSystem": null,
  "skill": {
    "slug": "kami-landing",
    "path": "design-templates/kami-landing",
    "kind": "design-template",
    "mode": "prototype"
  },
  "boundAt": "2026-05-20T00:00:00Z"
}
```

Note `designSystem: null` because the kami-landing template declares `od.design_system.requires: false` — it ships its own baked-in style.

- [ ] **Step 2: Verify file is valid JSON**

Run: `node -e "JSON.parse(require('fs').readFileSync('.open-design.json','utf8')); console.log('ok')"`
Expected: `ok`

- [ ] **Step 3: Stage but do not commit yet**

Run: `git add .open-design.json`
(Commit happens in Task 5 with everything else.)

---

### Task 2: Generate `index.html`

**Files:**
- Create: `index.html`

This is one big file. Scaffold structure is identical to the kami example except:
1. `<head>` metadata changes (title, description).
2. `.metrics` grid: 4 → 5 columns at desktop; 2 → 3 at tablet; 1 at mobile.
3. Chapters: 3 → 5 articles, content per spec §4.5.
4. Hero, manifesto, footer content per spec.
5. No `<em>` italic anywhere — use `<span class='tag standard'>` instead (the kami example violates its own no-italic rule with `<em class='tag standard'>`; do not repeat that mistake).

- [ ] **Step 1: Write `index.html`**

Use the Write tool. Full file content below (copy verbatim — every byte matters for the kami self-check).

```html
<!DOCTYPE html>
<html lang='en'>
<head>
<meta charset='utf-8' />
<meta name='viewport' content='width=device-width, initial-scale=1' />
<title>Open Design Skill — Design systems for your agent.</title>
<meta name='description' content='Bring the Open Design catalogue into any agent session. 150+ brand systems, 110+ rendering templates, 130+ functional skills, 11 craft references — composed at agent runtime, no daemon required.' />
<link rel='preconnect' href='https://fonts.googleapis.com' />
<link rel='preconnect' href='https://fonts.gstatic.com' crossorigin />
<link href='https://fonts.googleapis.com/css2?family=Source+Serif+4:wght@400;500&family=Source+Sans+3:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap' rel='stylesheet' />
<style>
:root {
  --parchment: #f5f4ed;
  --ivory: #faf9f5;
  --warm-sand: #e8e6dc;
  --brand: #1B365D;
  --brand-light: #2D5A8A;
  --near-black: #141413;
  --dark-warm: #3d3d3a;
  --olive: #504e49;
  --stone: #6b6a64;
  --border: #e8e6dc;
  --border-soft: #e5e3d8;
  --tag-08: #EEF2F7;
  --tag-14: #E4ECF5;
  --tag-22: #D0DCE9;
  --tag-30: #D6E1EE;
  --serif: 'Source Serif 4', Charter, Georgia, Palatino, 'Times New Roman', serif;
  --sans: 'Source Sans 3', -apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif;
  --mono: 'JetBrains Mono', 'SF Mono', 'Fira Code', Consolas, Monaco, monospace;
  --whisper: 0 4px 24px rgba(0, 0, 0, 0.05);
}
* { box-sizing: border-box; margin: 0; padding: 0; }
html, body { background: var(--parchment); color: var(--near-black); }
body {
  font-family: var(--serif);
  font-weight: 400;
  font-size: 14px;
  line-height: 1.55;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-feature-settings: 'liga', 'calt';
}
strong { font-weight: 500; }
img { max-width: 100%; display: block; }
a { color: var(--brand); text-decoration: none; border-bottom: 1px solid currentColor; }
a:hover { color: var(--brand-light); }

.shell { max-width: 1120px; margin: 0 auto; padding: 88px 64px 120px; position: relative; }

/* eyebrow strip */
.eyebrow-row {
  display: flex; justify-content: space-between; align-items: center;
  font-family: var(--sans); font-size: 12px; font-weight: 500;
  letter-spacing: 1.2px; text-transform: uppercase; color: var(--stone);
  border-bottom: 1px solid var(--border); padding-bottom: 18px; margin-bottom: 88px;
}
.eyebrow-row .lang { display: inline-flex; gap: 16px; }
.eyebrow-row .lang a { color: var(--stone); border-bottom: none; }
.eyebrow-row .lang a.active { color: var(--brand); }
.eyebrow-row .meta { display: inline-flex; gap: 22px; font-variant-numeric: tabular-nums; }
.eyebrow-row .meta b { color: var(--near-black); font-weight: 500; }

/* hero */
.hero {
  display: grid; grid-template-columns: 1.45fr 0.55fr; gap: 64px;
  align-items: end; margin-bottom: 96px;
}
.hero-copy h1 {
  font-family: var(--serif); font-weight: 500;
  font-size: clamp(60px, 7.4vw, 106px); line-height: 1.05;
  letter-spacing: -1.2px; color: var(--near-black); margin-bottom: 28px;
}
.hero-copy h1 .ink { color: var(--brand); }
.hero-copy .tagline {
  font-family: var(--serif); font-weight: 500;
  font-size: 21px; line-height: 1.45; color: var(--olive); max-width: 48ch;
}
.hero-tokens {
  display: flex; flex-direction: column; gap: 14px; align-items: flex-end;
  font-family: var(--mono); font-size: 11px; letter-spacing: 0.4px;
  color: var(--stone); text-transform: uppercase;
}
.hero-tokens .row {
  display: inline-flex; align-items: center; gap: 10px;
  background: var(--ivory); border: 1px solid var(--border);
  border-radius: 6px; padding: 10px 14px; font-variant-numeric: tabular-nums;
}
.hero-tokens .row b {
  color: var(--brand); font-weight: 500;
  font-family: var(--serif); font-size: 14px;
}
.hero-tokens .row span { font-family: var(--sans); }

/* manifesto */
.manifesto {
  border-top: 1px solid var(--border); border-bottom: 1px solid var(--border);
  padding: 44px 0 40px; margin-bottom: 88px;
  display: grid; grid-template-columns: 1fr 1.85fr; gap: 56px; align-items: start;
}
.manifesto .label {
  font-family: var(--sans); font-size: 12px; font-weight: 500;
  letter-spacing: 1.2px; text-transform: uppercase; color: var(--stone);
}
.manifesto .body {
  font-family: var(--serif); font-weight: 400;
  font-size: 20px; line-height: 1.65; letter-spacing: 0.05em;
  color: var(--olive); border-left: 2px solid var(--brand);
  padding: 4px 0 4px 24px;
}
.manifesto .body strong { color: var(--near-black); font-weight: 500; }
.manifesto .signature {
  margin-top: 22px; font-family: var(--serif); font-weight: 500;
  font-size: 14px; color: var(--dark-warm);
  display: inline-flex; gap: 10px; align-items: baseline;
}
.manifesto .signature span { color: var(--stone); }

/* metrics — 5 columns */
.metrics {
  display: grid; grid-template-columns: repeat(5, 1fr); gap: 0;
  border-top: 1px solid var(--border); border-bottom: 1px solid var(--border);
  margin-bottom: 88px;
}
.metric {
  padding: 28px 22px 26px; display: flex; flex-direction: column;
  gap: 6px; border-right: 1px solid var(--border-soft);
}
.metric:last-child { border-right: 0; }
.metric .value {
  font-family: var(--serif); font-weight: 500; font-size: 36px; line-height: 1;
  color: var(--brand); font-variant-numeric: tabular-nums; letter-spacing: -0.5px;
}
.metric .label {
  font-family: var(--serif); font-weight: 500; font-size: 13px;
  color: var(--near-black); margin-top: 8px;
}
.metric .sub {
  font-family: var(--serif); font-weight: 400; font-size: 12px;
  color: var(--olive); line-height: 1.5; max-width: 28ch;
}

/* chapters */
.chapters { display: grid; grid-template-columns: 1fr; gap: 72px; margin-bottom: 96px; }
.chapter {
  display: grid; grid-template-columns: 1fr 2.6fr; gap: 56px; align-items: start;
}
.chapter .head { display: flex; flex-direction: column; gap: 14px; }
.chapter .num {
  font-family: var(--serif); font-weight: 500; font-size: 14px;
  color: var(--brand); letter-spacing: 0.4px; font-variant-numeric: tabular-nums;
}
.chapter .title {
  font-family: var(--serif); font-weight: 500; font-size: 28px;
  line-height: 1.2; color: var(--near-black); letter-spacing: 0.4px;
}
.chapter .lede {
  font-family: var(--serif); font-weight: 500; font-size: 14px;
  color: var(--olive); line-height: 1.5; max-width: 30ch;
}
.chapter .body { display: flex; flex-direction: column; gap: 16px; }
.chapter .body p {
  font-family: var(--serif); font-weight: 400; font-size: 14px;
  color: var(--dark-warm); line-height: 1.55; max-width: 62ch;
}
.chapter .body p strong { color: var(--near-black); font-weight: 500; }
.chapter .body code {
  font-family: var(--mono); font-size: 12.5px; color: var(--brand);
  background: var(--tag-08); padding: 1px 6px; border-radius: 3px;
}
.chapter ul.dash {
  list-style: none; padding: 0; margin: 4px 0 0;
  display: flex; flex-direction: column; gap: 8px;
}
.chapter ul.dash li {
  position: relative; padding-left: 16px;
  font-family: var(--serif); font-weight: 400; font-size: 14px;
  color: var(--dark-warm); line-height: 1.55;
}
.chapter ul.dash li::before {
  content: '\2013'; position: absolute; left: 0; color: var(--brand);
}
.chapter .prompts {
  display: grid; grid-template-columns: 1fr 1fr; gap: 8px 28px;
  margin-top: 4px;
}
.chapter .prompts li {
  list-style: none; position: relative; padding-left: 16px;
  font-family: var(--serif); font-weight: 400; font-size: 14px;
  color: var(--dark-warm); line-height: 1.55;
}
.chapter .prompts li::before { content: '\2013'; position: absolute; left: 0; color: var(--brand); }
.chapter .prompts li q {
  font-style: normal; quotes: '\201C' '\201D';
  color: var(--near-black);
}

/* chapter aside (code block) */
.chapter-aside {
  background: var(--ivory); border: 1px solid var(--border);
  border-radius: 8px; padding: 22px 22px 20px; margin-top: 20px;
  display: flex; flex-direction: column; gap: 14px;
  transition: box-shadow 0.2s;
}
.chapter-aside:hover { box-shadow: var(--whisper); }
.chapter-aside .a-label {
  font-family: var(--sans); font-size: 12px; font-weight: 500;
  letter-spacing: 0.4px; text-transform: uppercase; color: var(--stone);
}
.chapter-aside .a-body {
  font-family: var(--mono); font-size: 12px; color: var(--near-black);
  white-space: pre; overflow-x: auto;
  background: var(--parchment); border: 1px solid var(--border-soft);
  border-radius: 6px; padding: 12px 14px; line-height: 1.65;
}
.chapter-aside .a-body .k { color: var(--brand); }
.chapter-aside .a-body .c { color: var(--stone); }

/* tags */
.tag {
  display: inline-block; font-family: var(--sans);
  font-size: 12px; font-weight: 500;
  padding: 2px 7px; border-radius: 2px;
  color: var(--brand); background: var(--tag-08); letter-spacing: 0.4px;
}
.tag.standard { background: var(--tag-14); padding: 2px 8px; border-radius: 4px; }
.tag.brush { background: linear-gradient(to right, #D6E1EE, #E4ECF5 70%, #EEF2F7); }
.tag-row { display: inline-flex; gap: 8px; flex-wrap: wrap; align-items: center; }

/* footer */
.footer {
  border-top: 1px solid var(--border); padding-top: 56px;
  display: grid; grid-template-columns: 1.4fr 0.85fr 0.85fr 0.9fr;
  gap: 48px; align-items: start;
}
.footer .kicker {
  font-family: var(--serif); font-weight: 500;
  font-size: 56px; line-height: 1.05; letter-spacing: -0.6px;
  color: var(--near-black); margin-bottom: 14px;
}
.footer .kicker .ink { color: var(--brand); }
.footer .colophon {
  font-family: var(--serif); font-weight: 500; font-size: 13px;
  color: var(--olive); max-width: 32ch; line-height: 1.55;
}
.footer .col h4 {
  font-family: var(--sans); font-size: 12px; font-weight: 500;
  letter-spacing: 1.2px; text-transform: uppercase;
  color: var(--stone); margin-bottom: 16px;
}
.footer .col ul { list-style: none; padding: 0; margin: 0; }
.footer .col li {
  font-family: var(--serif); font-weight: 500; font-size: 13px;
  color: var(--dark-warm); margin-bottom: 8px;
}
.footer .col li a { color: var(--dark-warm); border-bottom: none; }
.footer .col li a:hover { color: var(--brand); }
.footer .col li small {
  display: block; font-family: var(--sans); font-size: 11px;
  font-weight: 500; color: var(--stone); letter-spacing: 0.4px; margin-top: 1px;
}

.legal {
  margin-top: 56px; border-top: 1px solid var(--border-soft); padding-top: 22px;
  display: flex; justify-content: space-between; align-items: center;
  font-family: var(--mono); font-size: 11px; color: var(--stone);
  letter-spacing: 0.4px; font-variant-numeric: tabular-nums;
}
.legal b { color: var(--near-black); font-weight: 500; }

/* responsive */
@media (max-width: 1080px) {
  .shell { padding: 64px 48px 96px; }
  .hero { grid-template-columns: 1fr; gap: 36px; align-items: start; }
  .hero-tokens { flex-direction: row; flex-wrap: wrap; align-items: flex-start; }
  .manifesto { grid-template-columns: 1fr; gap: 22px; }
  .metrics { grid-template-columns: repeat(3, 1fr); }
  .metric:nth-child(3n) { border-right: 0; }
  .metric:nth-child(-n+3) { border-bottom: 1px solid var(--border-soft); }
  .chapter { grid-template-columns: 1fr; gap: 22px; }
  .chapter .prompts { grid-template-columns: 1fr; }
  .footer { grid-template-columns: 1fr 1fr; gap: 36px; }
  .footer .kicker { font-size: 44px; }
}
@media (max-width: 640px) {
  .shell { padding: 48px 24px 72px; }
  .hero-copy h1 { font-size: 46px; line-height: 1.08; letter-spacing: -0.6px; }
  .hero-copy .tagline { font-size: 17px; }
  .metrics { grid-template-columns: 1fr; }
  .metric { border-right: 0; border-bottom: 1px solid var(--border-soft); }
  .metric:last-child { border-bottom: 0; }
  .footer { grid-template-columns: 1fr; gap: 28px; }
  .footer .kicker { font-size: 36px; }
  .chapter .body p { font-size: 13.5px; }
  .legal { flex-direction: column; gap: 10px; align-items: flex-start; }
}
</style>
</head>
<body>

<main class='shell'>

  <!-- ============ EYEBROW ROW ============ -->
  <div class='eyebrow-row' data-od-id='eyebrow-row'>
    <div class='lang'>
      <a href='#' class='active'>EN</a>
    </div>
    <div class='meta'>
      <span><b>Agent Skill</b> · v1</span>
      <span>Edition <b>MMXXVI</b></span>
      <span>MIT</span>
    </div>
  </div>

  <!-- ============ HERO ============ -->
  <section class='hero' data-od-id='hero'>
    <div class='hero-copy'>
      <h1>Design systems for your <span class='ink'>agent</span>.</h1>
      <p class='tagline'>150+ brand systems, 110+ rendering templates, 130+ functional skills, 11 craft references — composed at agent runtime, no daemon required.</p>
    </div>
    <div class='hero-tokens'>
      <span class='row'><b>3</b> <span>Claude Code · Codex · Cursor</span></span>
      <span class='row'><b>16+</b> <span>Node</span></span>
      <span class='row'><b>npx</b> <span>skills add</span></span>
    </div>
  </section>

  <!-- ============ MANIFESTO ============ -->
  <section class='manifesto' data-od-id='manifesto'>
    <span class='label'>Manifesto · Nº 01</span>
    <div>
      <p class='body'>
        Design systems should live where the work happens — inside the agent writing your decks, your landing pages, your dashboards. Open Design Skill brings the full Open Design catalogue into any agent session. <strong>Pick a brand. Pick a template. Layer the universal craft rules.</strong> The same prompt composition Open Design's daemon does — without needing the daemon to run.
      </p>
      <div class='signature'>
        — Maintained by sugarforever
        <span>2026 · MIT · github.com/sugarforever</span>
      </div>
    </div>
  </section>

  <!-- ============ METRICS ============ -->
  <section class='metrics' data-od-id='metrics'>
    <div class='metric'>
      <div class='value'>150+</div>
      <div class='label'>Design systems</div>
      <div class='sub'>One DESIGN.md per brand — Airbnb, Apple, BMW, Stripe, Figma, Claude…</div>
    </div>
    <div class='metric'>
      <div class='value'>110+</div>
      <div class='label'>Rendering templates</div>
      <div class='sub'>Decks, dashboards, landing pages, posters, e-guides, mobile screens.</div>
    </div>
    <div class='metric'>
      <div class='value'>130+</div>
      <div class='label'>Functional skills</div>
      <div class='sub'>Briefs, audits, critique, copywriting, 5-dim reviews.</div>
    </div>
    <div class='metric'>
      <div class='value'>11</div>
      <div class='label'>Craft references</div>
      <div class='sub'>Typography, color, anti-AI-slop, accessibility, motion, RTL.</div>
    </div>
    <div class='metric'>
      <div class='value'>0</div>
      <div class='label'>Daemons required</div>
      <div class='sub'>Plain Node, plain git. Composes at agent runtime, no daemon.</div>
    </div>
  </section>

  <!-- ============ CHAPTERS ============ -->
  <section class='chapters' data-od-id='chapters'>

    <article class='chapter' data-od-id='chapter-01'>
      <div class='head'>
        <p class='num'>01</p>
        <h2 class='title'>The pitch</h2>
        <p class='lede'>A composer for design context, not another framework.</p>
      </div>
      <div class='body'>
        <p>
          Open Design Skill is a <strong>thin wrapper</strong> around the Open Design catalogue. It lets your agent pick a brand spec plus a template once per project, layer in the craft rules that template opts into, and follow the resulting composed workflow — the same prompt composition Open Design's daemon does, without needing the daemon to be running.
        </p>
        <ul class='dash'>
          <li>Triggered by phrases like <code>make me a deck</code>, <code>apply BMW brand</code>, <code>dashboard with Stripe</code>, <code>set up a design system</code>.</li>
          <li>Works across Claude Code, Codex, Cursor Agent, OpenCode, and Gemini CLI — anything that honors the <code>SKILL.md</code> convention.</li>
          <li>Writes real files into your project (HTML, JSX, markdown), not into a prompt buffer.</li>
        </ul>
        <p>
          If you have an agent and an idea, this is the missing layer.
        </p>
      </div>
    </article>

    <article class='chapter' data-od-id='chapter-02'>
      <div class='head'>
        <p class='num'>02</p>
        <h2 class='title'>How it composes</h2>
        <p class='lede'>Three phases. Setup, bind, compose. Then your agent ships.</p>
      </div>
      <div class='body'>
        <p>
          <strong>Phase 1 — Setup.</strong> Check that <code>$OPEN_DESIGN_ROOT</code> points at a clone of the Open Design content repo (or default to <code>~/.open-design-skill/repo</code>). Offer to clone on first run.
        </p>
        <p>
          <strong>Phase 2 — Bind.</strong> Look for <code>.open-design.json</code> in the project root. If absent, walk the user through picking a design system and a template. Save the choice; subsequent turns skip the pick flow.
        </p>
        <p>
          <strong>Phase 3 — Compose.</strong> Read the chosen <code>DESIGN.md</code>, the craft references the template opts into, and the template's <code>SKILL.md</code>. Authority order on conflict: brand tokens win, craft fills the gaps, the template body drives the workflow.
        </p>
        <div class='chapter-aside'>
          <div class='a-label'>.open-design.json · per-project binding</div>
          <pre class='a-body'><span class="c">// Committed to git → design choices persist across collaborators</span>
{
  <span class="k">"version"</span>: 1,
  <span class="k">"designSystem"</span>: { <span class="k">"slug"</span>: <span class="k">"bmw"</span>, <span class="k">"path"</span>: <span class="k">"design-systems/bmw"</span> },
  <span class="k">"skill"</span>: {
    <span class="k">"slug"</span>: <span class="k">"html-ppt-pitch-deck"</span>,
    <span class="k">"path"</span>: <span class="k">"design-templates/html-ppt-pitch-deck"</span>,
    <span class="k">"kind"</span>: <span class="k">"design-template"</span>,
    <span class="k">"mode"</span>: <span class="k">"deck"</span>
  }
}</pre>
        </div>
        <p>
          It is a runbook, not a runtime.
        </p>
      </div>
    </article>

    <article class='chapter' data-od-id='chapter-03'>
      <div class='head'>
        <p class='num'>03</p>
        <h2 class='title'>Install in one line</h2>
        <p class='lede'>Add the skill, clone the content once, never think about it again.</p>
      </div>
      <div class='body'>
        <p>
          The recommended path uses Vercel's <code>skills</code> CLI. It auto-discovers the <code>SKILL.md</code> and copies or symlinks it into your agent's skills directory.
        </p>
        <div class='chapter-aside'>
          <div class='a-label'>Install · 30 seconds</div>
          <pre class='a-body'><span class="c"># 1. Install the skill into your agent</span>
npx skills add <span class="k">sugarforever/open-design-skill</span>

<span class="c"># 2. Clone the Open Design content repo (a few hundred MB)</span>
git clone <span class="k">https://github.com/nexu-io/open-design</span> \
  ~/.open-design-skill/repo

<span class="c"># Or, if you already have a checkout:</span>
export OPEN_DESIGN_ROOT=/path/to/your/open-design</pre>
        </div>
        <p>
          Works with Claude Code, Codex, Cursor Agent, OpenCode, Gemini CLI. Requires Node 16+ and <code>git</code> on PATH. No bash dependency — every helper script is a portable <code>.mjs</code>.
        </p>
      </div>
    </article>

    <article class='chapter' data-od-id='chapter-04'>
      <div class='head'>
        <p class='num'>04</p>
        <h2 class='title'>Use it without thinking</h2>
        <p class='lede'>Five prompts that route through the skill, plus the one config file that remembers your choice.</p>
      </div>
      <div class='body'>
        <p>
          Talk to your agent the way you already do. The skill triggers implicitly on design-task phrasing.
        </p>
        <ul class='prompts'>
          <li><q>Make me a pitch deck for my seed round.</q></li>
          <li><q>Build a SaaS landing page with the Stripe design system.</q></li>
          <li><q>Apply the BMW brand to my homepage.</q></li>
          <li><q>Set up a design system for my project.</q></li>
          <li><q>Use Open Design to build a dashboard.</q></li>
        </ul>
        <p>
          Commit <code>.open-design.json</code> to share design choices across collaborators. Delete it to re-pick. The skill never copies template bodies into your repo — they stay in <code>$OPEN_DESIGN_ROOT</code>, so a <code>git pull</code> in the clone takes effect on the next agent turn.
        </p>
      </div>
    </article>

    <article class='chapter' data-od-id='chapter-05'>
      <div class='head'>
        <p class='num'>05</p>
        <h2 class='title'>Where the line is drawn</h2>
        <p class='lede'>Explicit non-goals. What's the skill, and what's the daemon.</p>
      </div>
      <div class='body'>
        <p>
          Open Design Skill is Stage 1: <strong>content</strong>. It proxies design systems, templates, and craft references into your agent's session. It does not include:
        </p>
        <ul class='dash'>
          <li>The in-browser iframe preview surface.</li>
          <li>Comment-mode surgical edits on the rendered artifact.</li>
          <li>Slider parameters that re-prompt the agent on change.</li>
          <li><code>od.mode</code> routing into different render surfaces.</li>
        </ul>
        <p>
          Those features live in Open Design's local daemon (Stage 2: <strong>surface</strong>). For preview and debugging while iterating on the artifact, use your agent's standard tools — start a dev server, drive the page with the <code>chrome-devtools</code> MCP or Playwright. <span class='tag brush'>Stage 1 → Stage 2</span> is the shape of the upgrade path.
        </p>
      </div>
    </article>

  </section>

  <!-- ============ FOOTER ============ -->
  <footer class='footer' data-od-id='footer'>
    <div>
      <h2 class='kicker'>kami<span class='ink'>.</span></h2>
      <p class='colophon'>
        This page is itself a kami one-pager — generated by the Open Design Skill from <code>design-templates/kami-landing</code>. MIT-licensed for any use.
      </p>
    </div>
    <div class='col'>
      <h4>Project</h4>
      <ul>
        <li><a href='https://github.com/sugarforever/open-design-skill'>README<small>quick start</small></a></li>
        <li><a href='https://github.com/sugarforever/open-design-skill/blob/main/SKILL.md'>SKILL.md<small>workflow runbook</small></a></li>
        <li><a href='https://github.com/sugarforever/open-design-skill/issues'>Issues<small>file a bug</small></a></li>
      </ul>
    </div>
    <div class='col'>
      <h4>Open Design</h4>
      <ul>
        <li><a href='https://github.com/nexu-io/open-design'>Upstream repo<small>nexu-io/open-design</small></a></li>
        <li><a href='https://github.com/nexu-io/open-design/tree/main/design-systems'>DESIGN.md catalog<small>150+ brands</small></a></li>
        <li><a href='https://github.com/nexu-io/open-design/tree/main/design-templates'>Templates<small>110+ surfaces</small></a></li>
      </ul>
    </div>
    <div class='col'>
      <h4>Tools</h4>
      <ul>
        <li><a href='https://www.npmjs.com/package/skills'>skills CLI<small>npx skills add</small></a></li>
        <li><a href='https://www.claude.com/product/claude-code'>Claude Code<small>Anthropic CLI</small></a></li>
        <li><a href='https://github.com/openai/codex'>Codex<small>OpenAI CLI</small></a></li>
      </ul>
    </div>
  </footer>

  <div class='legal'>
    <span><b>Open Design Skill</b> · MIT · MMXXVI</span>
    <span>Composed in kami · 紙 · 纸 · paper-first</span>
  </div>

</main>
</body>
</html>
```

- [ ] **Step 2: Confirm file exists and is non-empty**

Run: `wc -l index.html`
Expected: a count between 400 and 700 lines.

---

### Task 3: Kami invariant self-check

**Files:**
- Modify: none (audit-only)

The kami template SKILL.md has a self-check list. Run it as grep checks against `index.html`. Each check should produce zero matches (or the expected pattern).

- [ ] **Step 1: Forbidden colors — must produce no matches**

Run:
```bash
grep -nE '#fff(fff)?\b|#ffffff|#000000|#000\b' index.html
```
Expected: no output (exit 1 is fine — grep returns nonzero when no match).

- [ ] **Step 2: Cool-gray surfaces — must produce no matches**

Run:
```bash
grep -nE '#f8f9fa|#f3f4f6|slate-' index.html
```
Expected: no output.

- [ ] **Step 3: No italic**

Run:
```bash
grep -nE 'font-style:\s*italic|<em\b|<i\b' index.html
```
Expected: no output. (The CSS sets `.chapter .prompts li q { font-style: normal }` defensively — that's a `normal` declaration, not `italic`, so grep won't match.)

- [ ] **Step 4: No synthetic bold on serif**

Run:
```bash
grep -nE 'font-weight:\s*(600|700|800|900)' index.html
```
Expected: no output. (`Source Sans 3` is loaded with weight 600 in the Google Fonts URL, but no element uses it — the `:600` part appears only in the `<link>` URL which is fine.)

Note: the Google Fonts URL contains `wght@400;500;600` for Source Sans 3. The grep above looks for `font-weight:` declarations specifically, which excludes the URL. If the grep matches, find and fix any `font-weight: 600+` declarations.

- [ ] **Step 5: rgba in tag fills — must produce no matches**

Run:
```bash
grep -nE 'background[^;]*rgba\(' index.html
```
Expected: no output. (Shadow uses `rgba(0,0,0,0.05)` but that's the shadow, not a background.)

- [ ] **Step 6: Headline word count ≤ 6**

Run:
```bash
grep -A0 "<h1>" index.html | sed -n 's/.*<h1>\(.*\)<\/h1>.*/\1/p' | sed 's/<[^>]*>//g' | awk '{print NF}'
```
Expected: a number ≤ 6. (Our headline "Design systems for your agent." = 5 words.)

- [ ] **Step 7: tabular-nums used on metric and legal**

Run:
```bash
grep -nE 'font-variant-numeric:\s*tabular-nums' index.html
```
Expected: at least 4 matches (eyebrow meta, hero-tokens row, metric value, legal).

- [ ] **Step 8: data-od-id present on all editable sections**

Run:
```bash
grep -nE 'data-od-id=' index.html | wc -l
```
Expected: ≥ 9 (eyebrow + hero + manifesto + metrics + chapters wrapper + 5 chapters + footer).

If any check fails, fix `index.html` and re-run the failing check before proceeding.

---

### Task 4: Visual verification in browser

**Files:**
- Modify: none (visual sweep)

- [ ] **Step 1: Open `index.html` in the user's default browser**

Run from a separate shell or with `open`:
```bash
open /Users/wyang14/github/open-design-skill-web/index.html
```

- [ ] **Step 2: Sanity-check at desktop width (≥ 1120px)**

Eyeball check:
- Page background is warm parchment, not white.
- Hero headline reads "Design systems for your **agent**." with "agent" in ink-blue.
- 5 metric tiles sit in one row with vertical dividers between them.
- Five chapters render top-to-bottom with `01`–`05` numbering in ink-blue.
- Footer kicker word is `kami.` with the period in ink-blue.
- No horizontal scroll.

- [ ] **Step 3: Resize to 1024px and 640px to confirm responsive collapse**

Open Chrome DevTools (Cmd+Opt+I), toggle device toolbar, test at 1024px and 640px:
- At 1024px: hero collapses to single column, metrics go to 3 columns (3+2 layout), footer goes 2-column.
- At 640px: metrics stack to 1 column, footer stacks, hero headline scales down to 46px.
- No horizontal scrollbar at any width.

- [ ] **Step 4: Confirm fonts loaded (not fallback)**

In DevTools → Network panel, filter "Font", reload. Expect requests to `fonts.gstatic.com` resolving 200 for Source Serif 4, Source Sans 3, JetBrains Mono.

If any visual issue appears, edit `index.html` and re-run Tasks 3 + 4 before proceeding.

---

### Task 5: Commit

**Files:**
- Stage: `.open-design.json`, `index.html`

- [ ] **Step 1: Verify both files are present and the working tree is otherwise clean**

Run: `git status`
Expected: `.open-design.json` and `index.html` listed as new files (plus the existing untracked `.agents/`, `.claude/`, `skills-lock.json` which we leave alone for now).

- [ ] **Step 2: Stage and commit**

Run:
```bash
git add .open-design.json index.html
git commit -m "$(cat <<'EOF'
Ship landing page via kami-landing template

Single-page marketing landing for open-design-skill at index.html.
Built from the kami-landing template (design-templates/kami-landing)
with no separate design system — kami ships its own baked-in style.

Content: hero, manifesto, 5 metrics (catalog scale), 5 numbered
chapters (pitch / composition / install / usage / non-goals), footer.

Per-project binding written to .open-design.json so subsequent agent
turns short-circuit the pick flow.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

- [ ] **Step 3: Verify commit landed**

Run: `git log --oneline -n 3`
Expected: top entry mentions "kami-landing template".

---

## Self-Review

**1. Spec coverage:**
- Spec §2 (template choice + no design system) → Task 1 writes `designSystem: null` binding ✓
- Spec §3 (six-band page structure) → Task 2 inline HTML matches structure ✓
- Spec §4.1–4.6 (content brief) → Task 2 inline HTML carries every field ✓
- Spec §5 (kami invariants) → Task 3 grep checks enforce them ✓
- Spec §6 (output: self-contained, no JS, Google Fonts via link) → Task 2 file ✓
- Spec §7 (verification) → Tasks 3 + 4 together ✓
- Spec §8 (out of scope) — nothing to implement, no task needed ✓

**2. Placeholder scan:**
- No `TBD`, no `TODO`, no "implement later".
- Every code/HTML step contains the actual content.
- Verification commands have expected outputs.

**3. Type / name consistency:**
- `.open-design.json` schema matches the kami `skill.kind: "design-template"` per OD skill spec.
- HTML class names (`shell`, `hero`, `metric`, `chapter`, etc.) match kami example exactly except for the new `.chapter .prompts` block (added for Chapter 04's example-prompts list) which is defined inline.
- `data-od-id` slugs are unique per section.

No issues found.

---

## Execution Handoff

Plan complete and saved to `docs/superpowers/plans/2026-05-20-open-design-skill-website.md`. Two execution options:

1. **Subagent-Driven (recommended)** — dispatch a fresh subagent per task, review between tasks, fast iteration.
2. **Inline Execution** — execute tasks in this session using executing-plans, batch execution with checkpoints.

Which approach?
