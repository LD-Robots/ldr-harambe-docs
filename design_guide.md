# Field Notes — Technical Document Design System

A dark-warm, editorial-technical aesthetic for engineering field notes, research summaries, tuning guides, and similar technical documentation. Use this file as a reference when asked to produce HTML documents in this style, or when extending the system to new components.

---

## When to use this system

**Good fit**

- Engineering / research field notes
- Motor identification reports, tuning guides, procedure documents
- Technical documentation with math, code, and data visualization
- Internal reference material that lives alongside a project

**Bad fit**

- Marketing or consumer-facing pages
- Dashboards with heavy real-time data
- Anything that needs iconography or illustration to carry meaning
- Bright / casual / playful contexts

---

## Six principles

1. **Warmth over sterility.** Warm near-black background, warm off-white ink. Never pure black + pure white.
2. **One accent, used decisively.** Amber is the single signature color. Cyan, rose, moss are semantic only — used sparingly.
3. **Metadata is part of the design.** Labels, section numbers, units, captions, eyebrow text. Visible structure is the point.
4. **Three fonts, distinct roles.** Fraunces (editorial), IBM Plex Sans (body), JetBrains Mono (code/numbers/labels). Roles do not overlap.
5. **Density with air.** Tight line-height inside blocks, generous space between sections (~96px). Dense content, breathing room.
6. **Decoration earns its place.** Grain overlay, dashed rules, labels punching through borders — each carries meaning or softens the flat digital surface. Nothing decorative for its own sake.

---

## Color tokens

All colors are defined as CSS custom properties under `:root`. Reference by variable, not by hex.

| Token | Hex | Role |
|---|---|---|
| `--bg` | `#0e0c09` | page background — warm near-black |
| `--bg-panel` | `#15120d` | panels, cards, chart wrappers |
| `--bg-inset` | `#0a0806` | code blocks, math blocks — pushed inward |
| `--ink` | `#ece2cf` | primary text, strong emphasis |
| `--ink-dim` | `#a79a82` | secondary text, descriptions |
| `--ink-faint` | `#6b6354` | captions, labels, metadata |
| `--rule` | `#2a2419` | solid dividers, borders |
| `--rule-soft` | `#1e1a13` | dashed dividers, grid lines in charts |
| `--amber` | `#e8a33d` | signature accent, emphasis, primary chart series |
| `--amber-deep` | `#b87a1e` | underlines, code-block left borders |
| `--cyan` | `#5fb3b3` | secondary/measured data in charts |
| `--rose` | `#d97757` | warnings, failure modes |
| `--moss` | `#8fa968` | success, positive delta, code strings |

**Ratio rule.** Amber should not exceed ~10% of colored pixels in any given screen. Overusing the accent flattens the hierarchy.

---

## Typography

Three fonts, loaded from Google Fonts. Each has exactly one job.

### Fraunces — display + editorial voice

Used for `h1`, `h2`, `h3`, subtitles, and callouts.

- H1 uses weight 300 **italic** with an `<em>` word flipped to weight 600 upright amber
- H2 uses weight 400 with italic amber `<em>` for emphasis
- H3 uses weight 500 upright
- Callouts use weight 300 italic at 18px

### IBM Plex Sans — body text

All running prose, variable descriptions, card copy.

- Body: weight **300** at 16px (300, not 400 — the thinner body makes 500 emphasis feel like a real shift)
- Lead paragraph: weight 300 at 18.5px, `--ink`
- Emphasis: weight 500, color `--ink`

### JetBrains Mono — code, numbers, labels

Code blocks, inline code, metadata labels, numeric readouts, section numbers.

- Code blocks: weight 400 at 13.5px, line-height 1.7
- Labels (all-caps): weight 500 at 10–12px, letter-spacing 0.2–0.3em
- Numbers in readouts: weight 500, amber

### Type scale

| Role | Font | Size | Weight | Treatment |
|---|---|---|---|---|
| H1 display | Fraunces italic | `clamp(44px, 7vw, 88px)` | 300 | amber `<em>` for emphasis word |
| H2 section | Fraunces | `clamp(32px, 4.5vw, 52px)` | 400 | italic amber `<em>` |
| H3 subsection | Fraunces | 26px | 500 | — |
| H4 utility | JetBrains Mono | 12px | 500 | uppercase, 0.2em tracking, amber |
| Body | Plex Sans | 16px | 300 | line-height 1.65, max-width 70ch |
| Lead | Plex Sans | 18.5px | 300 | `--ink`, 68ch max |
| Callout | Fraunces italic | 18px | 300 | 2px amber left border, `--ink-dim` |
| Eyebrow | JetBrains Mono | 11–12px | 400–500 | uppercase, 0.3em tracking, amber |
| Code | JetBrains Mono | 13.5px | 400 | line-height 1.7 |
| Label | JetBrains Mono | 10px | 500 | uppercase, 0.22–0.25em tracking |

---

## Layout & spacing

- **Wrap**: `max-width: 1180px`, 32px horizontal padding
- **Prose paragraph**: `max-width: 70ch` — do not stretch body text across the full wrap
- **Lead paragraph**: `max-width: 68ch`
- **Callout**: `max-width: 60ch`
- **Section vertical spacing**: 96px between sections, 24px from H2 to body, 16px from H3 to body
- **Hero**: 56px top padding, 64px bottom padding, border-top + border-bottom `--rule`

One column throughout. Two- and three-column grids (`.two-col`, `.three-col`) appear inside specific sections but never dictate main flow.

---

## Signature patterns

### 1. Eyebrow text

Short all-caps monospace amber label above the hero title or any major section.

```html
<div class="eyebrow">Motor ID · Feedforward · PD tuning</div>
<h1 class="title">Your <em>headline</em> here.</h1>
```

### 2. Section numbers (§)

Every section opens with "§ NN — short descriptor" in amber monospace.

```html
<section id="pd">
  <span class="section-num">§ 03 — PD gains from first principles</span>
  <h2>Why Kp, Kd aren't <em>random</em> knobs.</h2>
</section>
```

### 3. Labeled panels (THE signature move)

A panel with a tiny label that appears to punch through its top border. Implemented via `::before` with background matching the page. This is the most recognizable element of the system — use it liberally.

```html
<div class="panel" data-label="Rules of thumb">
  <p>Panel content here.</p>
</div>
```

```css
.panel {
  background: var(--bg-panel);
  border: 1px solid var(--rule);
  padding: 28px 32px;
  margin: 24px 0;
  position: relative;
}
.panel::before {
  content: attr(data-label);
  position: absolute;
  top: -9px;       /* pulls label up onto the border */
  left: 20px;
  background: var(--bg);  /* matches page — punches through border */
  padding: 0 10px;
  font-family: var(--mono);
  font-size: 10px;
  letter-spacing: 0.25em;
  color: var(--amber);
  text-transform: uppercase;
}
```

The same pattern is used on `.math-block.labeled` (with `.lbl`), code blocks (`pre[data-lang]`), and the hero header's top-left/top-right markers.

---

## Components

### Callout — italic serif pull-quote

```html
<div class="callout">
  A memorable single sentence that summarizes the section's takeaway.
</div>
```

Used for mental models, design-philosophy lines, summary statements. Fraunces italic, 2px amber left border, no background. Max-width 60ch.

### Warning — rose-tinted

```html
<div class="warn">
  <strong>Warning.</strong> The body explains a failure mode or common mistake.
</div>
```

Rose-tinted gradient background, 2px rose left border. The opening word is `<strong>` and also rose. Use sparingly — typically one or two per page.

### Card — metric display

```html
<div class="three-col">
  <div class="card">
    <h5>Metric label</h5>
    <div class="big">0.102</div>
    <p>Short description of what this number means.</p>
  </div>
  <!-- two more cards -->
</div>
```

Mono amber label (uppercase tracked), large Fraunces number, dim-ink description. Typically three across in a `.three-col` grid.

### Readouts — instrument-panel style

```html
<div class="readouts">
  <div class="ro"><span class="k">K_p</span><span class="v">22.95 N·m/rad</span></div>
  <div class="ro"><span class="k">K_d</span><span class="v">2.14 N·m·s/rad</span></div>
</div>
```

Horizontal row of labeled numeric values. Used under charts to show computed readings.

### Steps — numbered procedure

```html
<ul class="steps">
  <li>
    <strong>Step title</strong>
    Body text explaining the step.
  </li>
  <li>
    <strong>Next step</strong>
    More body.
  </li>
</ul>
```

Numbered 01, 02, 03 via CSS counter + `decimal-leading-zero`. Dashed dividers between items.

### Bullets — arrow-prefixed

```html
<ul class="bullets">
  <li>A short bulleted point.</li>
  <li>Another point, arrow prefix in amber.</li>
</ul>
```

Uses `::before { content: "→" }` in amber. Use instead of disc bullets throughout.

### Math block — KaTeX-rendered equation

```html
<div class="math-block labeled">
  <span class="lbl">Eq. 1 — equation caption</span>
  $$ K_p = J \omega_n^2 $$
</div>
```

Dark inset background, monospace caption in the top-left corner. Use `$$...$$` for display math (KaTeX auto-render picks it up).

Follow every math block with a variable legend (`dl.vars`) or prose using the symbols:

```html
<dl class="vars">
  <dt>ω_n</dt> <dd>natural frequency, how fast the loop responds (rad/s)</dd>
  <dt>ζ</dt>   <dd>damping ratio — how much it rings</dd>
</dl>
```

### Code block

```html
<pre data-lang="python"><code>
def tune_gains(J, wn, zeta=0.7):
    Kp = J * wn**2
    Kd = 2 * zeta * (Kp * J)**0.5
    return Kp, Kd
</code></pre>
```

Amber-deep left border (2px, marks it instantly at skim). Language label top-right via `data-lang`. Manual tokenization with span classes `.tok-k` (keyword, rose), `.tok-s` (string, moss), `.tok-c` (comment, dim italic), `.tok-n` (number, amber), `.tok-f` (function, cyan).

### Reference table

```html
<table class="ref">
  <thead><tr><th>Parameter</th><th>Value</th><th>Note</th></tr></thead>
  <tbody>
    <tr><td>K_p</td><td>22.95</td><td class="v">stiffness, pick ω_n first</td></tr>
  </tbody>
</table>
```

Monospace throughout except cells with `.v` class, which soften to Plex Sans for prose. Headers amber on amber-deep underline. First column is amber by default.

### Inline equation fragment

```html
...at frequencies above <span class="inline-eq">ω_n / 10</span> the response...
```

Monospace, amber, slightly smaller. For tiny math snippets that don't warrant KaTeX.

---

## Data visualization (Chart.js)

Always wrap charts in `.graph-wrap`. Set these Chart.js defaults once per page:

```javascript
Chart.defaults.font.family = "'JetBrains Mono', monospace";
Chart.defaults.font.size = 11;
Chart.defaults.color = '#a79a82';      // --ink-dim
Chart.defaults.borderColor = '#2a2419'; // --rule

const C = {
  amber: '#e8a33d',   // primary series
  cyan:  '#5fb3b3',   // secondary / measured
  rose:  '#d97757',   // warning / wrong-way
  moss:  '#8fa968',   // success
  dim:   '#6b6354',   // reference lines (use with borderDash: [4,4])
  grid:  '#1e1a13',   // grid lines
  ink:   '#ece2cf',   // tooltip body
};

const grid = {
  grid:  { color: C.grid, drawBorder: false },
  ticks: { color: C.dim,  font: { size: 10, family: "'JetBrains Mono', monospace" } },
  title: { color: C.dim,  font: { size: 11, family: "'JetBrains Mono', monospace" } },
};
```

**Rules**

- One dominant color per chart (amber). Secondary series in cyan. Reference lines in `--ink-faint` with `borderDash: [4, 4]`.
- Grid barely visible — use `--rule-soft`.
- Axis titles always include units in brackets: `time [s]`, `torque [N·m]`.
- Legend at the bottom, mono, `--ink` color.
- Tooltip background `--bg-inset`, title amber, body `--ink`.

---

## Micro-details

- **Grain overlay**: fixed, pointer-events:none, SVG feTurbulence noise at ~3.5% opacity sitting over everything. Kills flat-digital feel.
- **Background gradients**: two very subtle radial gradients on the body (`#1a1409` fading to transparent) — one top-right, one upper-left. Suggests light sources without committing to a visible color.
- **Dashed vs solid rules**: solid = structural breaks, dashed = soft breaks within a section.
- **Zero-padded numbers**: use `counter(x, decimal-leading-zero)` — 01, 02, 03 not 1, 2, 3.
- **Responsive type**: `clamp(min, preferred, max)` on all display headings. No media queries needed.
- **Weight 300 body**: the deliberately thin body weight makes 500 emphasis feel like a meaningful shift. On dark warm it reads perfectly; don't change to 400.

---

## What breaks the aesthetic

- **Too much amber.** One amber element per screenful is often enough. When everything is emphasized, nothing is.
- **Extra colors.** Adding a fifth or sixth hue dilutes the palette. Audit before inventing a new semantic role.
- **Pure black text or white text.** Use `--ink` (#ece2cf) for foreground; pure white vibrates against warm dark.
- **Replacing Fraunces.** Its optical-size axis and italic quirks carry the editorial voice. Playfair, Lora, Newsreader all feel generic by comparison.
- **Centered body text.** Everything is left-aligned.
- **Icons.** This system does not use iconography (except inside charts). Icons compete with the typographic emphasis system.
- **Drop shadows, glass effects, glows.** Depth comes from background-lightness differences only.
- **Animations.** Tiny hover color-transitions only. No scroll-linked effects, no fade-ins, no parallax.
- **Decorative images.** The grain and the radial gradients are the only ambient visual texture. Additional imagery breaks the voice.

---

## Starter template

> **For a new doc, copy [`boilerplate.html`](boilerplate.html) at the repo root and rename it.** It contains the full token set, the reactive-palette overrides, the theme toggle, and a hero/section/footer skeleton — already wired up. The skeleton below is the same structure, kept here for reference.

A minimal HTML skeleton that loads all dependencies and sets up the first section. Fill in the content.

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Your document title</title>

  <!-- KaTeX for math -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css">
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js"></script>
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js"
     onload="renderMathInElement(document.body,{
       delimiters:[{left:'$$',right:'$$',display:true},{left:'$',right:'$',display:false}],
       throwOnError:false});"></script>

  <!-- Chart.js (optional, only if charts are used) -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>

  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,400;0,9..144,600;0,9..144,800;1,9..144,400&family=IBM+Plex+Sans:wght@300;400;500;600&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">

  <style>
    /* paste the complete CSS block below here */
  </style>
</head>
<body>

<button class="theme-toggle" type="button" aria-label="Switch palette">
  <span class="dot">●</span><span class="lbl">FIELD NOTES</span>
</button>

<div class="wrap">

  <header class="hero">
    <div class="eyebrow">Your eyebrow text</div>
    <h1 class="title">Your <em>headline</em> here.</h1>
    <p class="subtitle">A brief description of the document.</p>

    <div class="meta">
      <div><span class="k">Key</span><span class="v">Value</span></div>
      <div><span class="k">Key</span><span class="v">Value</span></div>
      <div><span class="k">Key</span><span class="v">Value</span></div>
      <div><span class="k">Key</span><span class="v">Value</span></div>
    </div>

    <nav class="toc">
      <a href="#first"><span class="n">01</span>First section</a>
      <a href="#second"><span class="n">02</span>Second section</a>
    </nav>
  </header>

  <section id="first">
    <span class="section-num">§ 01 — First section</span>
    <h2>Section <em>headline</em>.</h2>
    <p class="lead">The lead paragraph, larger and brighter.</p>
    <p>Body paragraph.</p>

    <div class="panel" data-label="Example label">
      <p>Panel content here.</p>
    </div>
  </section>

</div>

<script>
(function(){
  const KEY  = 'harambe-doc-theme';
  const root = document.documentElement;
  const btn  = document.querySelector('.theme-toggle');
  const lbl  = btn.querySelector('.lbl');
  function apply(mode){
    if (mode === 'reactive') root.setAttribute('data-theme','reactive');
    else root.removeAttribute('data-theme');
    lbl.textContent = mode === 'reactive' ? 'REACTIVE' : 'FIELD NOTES';
    btn.dataset.mode = mode;
  }
  apply(localStorage.getItem(KEY) || 'field-notes');
  btn.addEventListener('click', () => {
    const next = btn.dataset.mode === 'reactive' ? 'field-notes' : 'reactive';
    localStorage.setItem(KEY, next);
    apply(next);
  });
})();
</script>
</body>
</html>
```

---

## Complete CSS block (copy-paste ready)

Paste everything below into the `<style>` tag of the starter template.

```css
:root{
  --bg:#0e0c09; --bg-panel:#15120d; --bg-inset:#0a0806;
  --ink:#ece2cf; --ink-dim:#a79a82; --ink-faint:#6b6354;
  --rule:#2a2419; --rule-soft:#1e1a13;
  --amber:#e8a33d; --amber-deep:#b87a1e;
  --cyan:#5fb3b3; --rose:#d97757; --moss:#8fa968;
  --mono:'JetBrains Mono',ui-monospace,Menlo,monospace;
  --sans:'IBM Plex Sans',system-ui,sans-serif;
  --serif:'Fraunces',Georgia,serif;
}
*{box-sizing:border-box}
html,body{background:var(--bg);color:var(--ink);margin:0;padding:0}
body{
  font-family:var(--sans);font-weight:300;font-size:16px;line-height:1.65;
  -webkit-font-smoothing:antialiased;
  background:
    radial-gradient(1200px 600px at 85% -10%, #1a1409 0%, transparent 60%),
    radial-gradient(900px 500px at -10% 20%, #120f08 0%, transparent 55%),
    var(--bg);
  background-attachment:fixed;
}
body::before{
  content:"";position:fixed;inset:0;pointer-events:none;z-index:1;opacity:.035;
  background-image:url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='160' height='160'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2' stitchTiles='stitch'/></filter><rect width='100%' height='100%' filter='url(%23n)'/></svg>");
}
.wrap{max-width:1180px;margin:0 auto;padding:64px 32px 120px;position:relative;z-index:2}

header.hero{border-top:1px solid var(--rule);border-bottom:1px solid var(--rule);padding:56px 0 64px;margin-bottom:72px;position:relative}
header.hero::before{content:"DOC/01";position:absolute;top:-11px;left:0;background:var(--bg);padding:0 12px 0 0;font-family:var(--mono);font-size:11px;letter-spacing:.3em;color:var(--amber)}
header.hero::after{content:"field notes";position:absolute;top:-11px;right:0;background:var(--bg);padding:0 0 0 12px;font-family:var(--mono);font-size:11px;letter-spacing:.2em;color:var(--ink-faint)}
.eyebrow{font-family:var(--mono);font-size:12px;letter-spacing:.3em;text-transform:uppercase;color:var(--amber);margin-bottom:28px}
h1.title{font-family:var(--serif);font-weight:300;font-style:italic;font-size:clamp(44px,7vw,88px);line-height:.98;letter-spacing:-.02em;margin:0 0 8px;color:var(--ink)}
h1.title em{font-style:normal;font-weight:600;color:var(--amber)}
.subtitle{font-family:var(--serif);font-weight:300;font-size:clamp(20px,2.4vw,28px);line-height:1.35;color:var(--ink-dim);max-width:720px;margin:24px 0 0}
.meta{display:grid;grid-template-columns:repeat(4,1fr);gap:24px;margin-top:48px;padding-top:24px;border-top:1px dashed var(--rule);font-family:var(--mono);font-size:11px;letter-spacing:.08em}
.meta .k{color:var(--ink-faint);text-transform:uppercase;display:block;margin-bottom:6px;font-size:10px;letter-spacing:.2em}
.meta .v{color:var(--ink);font-size:13px;font-weight:500}
.meta .v em{color:var(--amber);font-style:normal}

section{margin:96px 0 0;position:relative}
.section-num{font-family:var(--mono);font-size:11px;letter-spacing:.3em;color:var(--amber);margin-bottom:18px;display:block}
h2{font-family:var(--serif);font-weight:400;font-size:clamp(32px,4.5vw,52px);line-height:1.05;letter-spacing:-.015em;margin:0 0 24px;color:var(--ink);max-width:860px}
h2 em{color:var(--amber);font-style:italic;font-weight:300}
h3{font-family:var(--serif);font-weight:500;font-size:26px;line-height:1.2;letter-spacing:-.01em;margin:48px 0 16px;color:var(--ink)}
h4{font-family:var(--mono);font-weight:500;font-size:12px;letter-spacing:.2em;text-transform:uppercase;color:var(--amber);margin:32px 0 12px}
p{margin:0 0 18px;max-width:70ch}
p.lead{font-size:18.5px;color:var(--ink);max-width:68ch;line-height:1.6}
strong{color:var(--ink);font-weight:500}
em{color:var(--amber);font-style:italic}
a{color:var(--amber);text-decoration:none;border-bottom:1px dotted var(--amber-deep)}
hr.divider{border:none;border-top:1px solid var(--rule);margin:72px 0}
hr.divider-soft{border:none;border-top:1px dashed var(--rule-soft);margin:32px 0}

.panel{background:var(--bg-panel);border:1px solid var(--rule);padding:28px 32px;margin:24px 0;position:relative}
.panel::before{content:attr(data-label);position:absolute;top:-9px;left:20px;background:var(--bg);padding:0 10px;font-family:var(--mono);font-size:10px;letter-spacing:.25em;color:var(--amber);text-transform:uppercase}
.panel.inset{background:var(--bg-inset)}

.callout{border-left:2px solid var(--amber);padding:4px 0 4px 24px;margin:24px 0;font-family:var(--serif);font-style:italic;font-size:18px;color:var(--ink-dim);line-height:1.5;max-width:60ch}

.warn{background:linear-gradient(180deg, rgba(217,119,87,.08), rgba(217,119,87,.03));border-left:2px solid var(--rose);padding:18px 22px;margin:28px 0;font-size:15.5px}
.warn strong{color:var(--rose)}

.math-block{background:var(--bg-inset);border:1px solid var(--rule);padding:24px 28px;margin:20px 0;overflow-x:auto;position:relative}
.math-block.labeled{padding-top:34px}
.math-block .lbl{position:absolute;top:10px;left:16px;font-family:var(--mono);font-size:10px;letter-spacing:.22em;color:var(--ink-faint);text-transform:uppercase}
.math-block .katex{color:var(--ink);font-size:1.05em}
.math-block .katex-display{margin:.4em 0}

dl.vars{font-family:var(--mono);font-size:13px;line-height:1.9;margin:16px 0 24px;display:grid;grid-template-columns:minmax(80px,auto) 1fr;gap:4px 28px}
dl.vars dt{color:var(--amber);font-weight:500}
dl.vars dd{color:var(--ink-dim);margin:0;font-family:var(--sans);font-size:14px}

pre{background:var(--bg-inset);border:1px solid var(--rule);border-left:2px solid var(--amber-deep);padding:22px 26px;margin:24px 0;overflow-x:auto;font-family:var(--mono);font-size:13.5px;line-height:1.7;color:var(--ink);position:relative}
pre::before{content:attr(data-lang);position:absolute;top:8px;right:16px;font-size:10px;letter-spacing:.25em;color:var(--ink-faint);text-transform:uppercase}
code{font-family:var(--mono);font-size:.92em;color:var(--amber);background:rgba(232,163,61,.08);padding:1px 6px;border-radius:2px}
pre code{color:var(--ink);background:none;padding:0}
.tok-k{color:var(--rose)} .tok-s{color:var(--moss)} .tok-c{color:var(--ink-faint);font-style:italic} .tok-n{color:var(--amber)} .tok-f{color:var(--cyan)}

table.ref{width:100%;border-collapse:collapse;margin:24px 0;font-family:var(--mono);font-size:13.5px}
table.ref th,table.ref td{text-align:left;padding:12px 16px;border-bottom:1px solid var(--rule);vertical-align:top}
table.ref th{color:var(--amber);font-weight:500;font-size:11px;letter-spacing:.18em;text-transform:uppercase;border-bottom:1px solid var(--amber-deep)}
table.ref td:first-child{color:var(--amber);font-weight:500}
table.ref td.v{color:var(--ink-dim);font-family:var(--sans);font-size:14px;font-weight:300}

ul.steps{list-style:none;padding:0;margin:24px 0;counter-reset:st}
ul.steps > li{counter-increment:st;padding:16px 0 16px 56px;border-bottom:1px dashed var(--rule-soft);position:relative;max-width:72ch}
ul.steps > li::before{content:counter(st,decimal-leading-zero);position:absolute;left:0;top:18px;font-family:var(--mono);font-size:11px;letter-spacing:.15em;color:var(--amber)}
ul.steps > li:last-child{border-bottom:none}
ul.steps > li strong{display:block;margin-bottom:4px;color:var(--ink);font-weight:500}

ul.bullets{list-style:none;padding:0;margin:16px 0;max-width:68ch}
ul.bullets li{padding:6px 0 6px 24px;position:relative}
ul.bullets li::before{content:"→";position:absolute;left:0;top:6px;color:var(--amber);font-family:var(--mono)}

nav.toc{margin:32px 0 0;padding:24px 0 0;border-top:1px solid var(--rule);display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:24px 48px}
nav.toc a{display:block;color:var(--ink-dim);border:none;padding:8px 0;border-bottom:1px dashed var(--rule-soft);font-family:var(--sans);font-size:14px;font-weight:300;transition:color .2s,padding-left .2s}
nav.toc a:hover{color:var(--amber);padding-left:8px}
nav.toc a .n{font-family:var(--mono);font-size:10px;letter-spacing:.2em;color:var(--ink-faint);margin-right:14px}

.two-col{display:grid;grid-template-columns:1fr 1fr;gap:32px;margin:24px 0}
@media (max-width:800px){.two-col{grid-template-columns:1fr}}
.three-col{display:grid;grid-template-columns:repeat(3,1fr);gap:24px;margin:24px 0}
@media (max-width:900px){.three-col{grid-template-columns:1fr}}

.card{background:var(--bg-panel);border:1px solid var(--rule);padding:22px 24px}
.card h5{font-family:var(--mono);font-size:10px;letter-spacing:.25em;text-transform:uppercase;color:var(--amber);margin:0 0 10px}
.card p{font-size:14.5px;color:var(--ink-dim);margin:0}
.card .big{font-family:var(--serif);font-size:36px;font-weight:400;color:var(--ink);line-height:1;margin:4px 0 8px;letter-spacing:-.01em}

.graph-wrap{background:var(--bg-panel);border:1px solid var(--rule);margin:28px 0;padding:24px 28px 28px;position:relative}
.graph-wrap .g-head{display:flex;justify-content:space-between;align-items:baseline;margin-bottom:4px;flex-wrap:wrap;gap:12px}
.graph-wrap .g-title{font-family:var(--serif);font-weight:500;font-size:20px;color:var(--ink)}
.graph-wrap .g-sub{font-family:var(--mono);font-size:11px;letter-spacing:.15em;color:var(--ink-faint);text-transform:uppercase}
.graph-wrap .g-desc{color:var(--ink-dim);font-size:14.5px;max-width:65ch;margin:4px 0 18px}
.graph-canvas{position:relative;height:340px;width:100%}

.readouts{display:flex;gap:24px;flex-wrap:wrap;margin-top:14px;padding-top:14px;border-top:1px dashed var(--rule);font-family:var(--mono);font-size:12px}
.readouts .ro{display:flex;flex-direction:column;gap:2px}
.readouts .ro .k{font-size:10px;letter-spacing:.2em;color:var(--ink-faint);text-transform:uppercase}
.readouts .ro .v{color:var(--amber);font-size:14px;font-weight:500}

footer{margin-top:120px;padding-top:32px;border-top:1px solid var(--rule);font-family:var(--mono);font-size:11px;letter-spacing:.15em;color:var(--ink-faint);display:flex;justify-content:space-between;flex-wrap:wrap;gap:12px}

.inline-eq{font-family:var(--mono);color:var(--amber);font-size:.94em}

/* ─────── Reactive palette (toggle) ─────────────────────
   When data-theme="reactive" is set on <html>, tokens
   resolve to VSCode webview CSS variables, falling back
   to VSCode Dark/Light Modern hexes when those aren't
   injected (i.e. doc opened in a regular browser).
   prefers-color-scheme handles light vs dark fallback. */
:root[data-theme="reactive"]{
  --bg:var(--vscode-editor-background,#1f1f1f);
  --bg-panel:var(--vscode-sideBar-background,#181818);
  --bg-inset:var(--vscode-textCodeBlock-background,#1a1a1a);
  --ink:var(--vscode-editor-foreground,#cccccc);
  --ink-dim:var(--vscode-descriptionForeground,#9d9d9d);
  --ink-faint:var(--vscode-disabledForeground,#6f6f6f);
  --rule:var(--vscode-panel-border,#2b2b2b);
  --rule-soft:var(--vscode-editorWidget-border,#252525);
  --amber:var(--vscode-textLink-foreground,#4ec9b0);
  --amber-deep:var(--vscode-textLink-activeForeground,#569cd6);
  --cyan:var(--vscode-symbolIcon-functionForeground,#dcdcaa);
  --rose:var(--vscode-errorForeground,#f48771);
  --moss:var(--vscode-debugTokenExpression-string,#ce9178);
}
@media (prefers-color-scheme: light){
  :root[data-theme="reactive"]{
    --bg:var(--vscode-editor-background,#ffffff);
    --bg-panel:var(--vscode-sideBar-background,#f8f8f8);
    --bg-inset:var(--vscode-textCodeBlock-background,#f3f3f3);
    --ink:var(--vscode-editor-foreground,#3b3b3b);
    --ink-dim:var(--vscode-descriptionForeground,#616161);
    --ink-faint:var(--vscode-disabledForeground,#8c8c8c);
    --rule:var(--vscode-panel-border,#e5e5e5);
    --rule-soft:var(--vscode-editorWidget-border,#ececec);
    --amber:var(--vscode-textLink-foreground,#0451a5);
    --amber-deep:var(--vscode-textLink-activeForeground,#0e639c);
  }
}
:root[data-theme="reactive"] body{background:var(--bg)}
@media (prefers-color-scheme: light){
  :root[data-theme="reactive"] body::before{opacity:.025}
}

.theme-toggle{
  position:fixed;top:16px;right:16px;z-index:10;
  background:var(--bg-panel);color:var(--ink-faint);
  border:1px solid var(--rule);padding:8px 12px;
  font-family:var(--mono);font-size:10px;font-weight:500;
  letter-spacing:.22em;text-transform:uppercase;
  cursor:pointer;transition:color .2s,border-color .2s;
}
.theme-toggle:hover{color:var(--amber);border-color:var(--amber-deep)}
.theme-toggle .dot{color:var(--amber);margin-right:8px}
```

---

## Class cheatsheet

| Class / selector | Use for |
|---|---|
| `.wrap` | page container (max-width 1180, centered) |
| `.hero` | top-of-page header block |
| `.eyebrow` | mono tracked text above a title |
| `h1.title` | Fraunces italic display headline |
| `.subtitle` | Fraunces light subtitle under title |
| `.meta` | 4-column metadata grid at end of hero |
| `.section-num` | "§ 01 — ..." section marker |
| `p.lead` | first paragraph of a section, larger |
| `.panel[data-label]` | labeled content panel |
| `.callout` | italic serif pull-quote |
| `.warn` | rose-tinted warning block |
| `.math-block.labeled` | display math with `.lbl` caption |
| `dl.vars` | variable-definition list after math |
| `pre[data-lang]` | code block with language tag |
| `table.ref` | reference table (use `td.v` for prose cells) |
| `ul.steps` | numbered step list (01, 02, 03) |
| `ul.bullets` | arrow-bulleted list |
| `.card` | metric card (label / big number / desc) |
| `.graph-wrap` | chart container with title + canvas |
| `.readouts .ro` | instrument-panel value display |
| `.two-col` / `.three-col` | responsive inline grids |
| `.inline-eq` | inline mono-amber equation fragment |
| `hr.divider` / `hr.divider-soft` | solid / dashed section rule |

---

*End of design system reference. When extending, start from this file's structure and tokens — don't invent parallel systems.*
