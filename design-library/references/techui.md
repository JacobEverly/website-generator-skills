---
name: tech-ui
description: Build interfaces in the OpenAI/modern-tech design language — pure white, generous space, rounded shapes, no chrome, color through imagery only. Use when building product UIs, dashboards, landing pages, or any interface that should feel premium, clean, and modern.
---

This skill applies the visual design language of OpenAI's product and marketing interfaces: extreme restraint, editorial whitespace, softly rounded shapes, and color delivered entirely through photography and gradients — never through UI chrome.

## Core Philosophy

The design says nothing with decoration. It lets the content speak inside a frame of radical emptiness. Every element earns its place. When in doubt, add more space, not more elements.

**The single guiding principle**: If you can feel the UI, it has too much.

---

## Color

**Backgrounds**: `#FFFFFF` or `#FAFAFA`. Sections with lower emphasis use `#F4F4F4`.
**Text primary**: `#0D0D0D` (near-black, not pure black).
**Text secondary / metadata**: `#6E6E6E` (cool gray, never warm).
**Borders**: `#E5E5E5` — one pixel, no more.
**Interactive surfaces**: `#F4F4F4` on hover, `#EBEBEB` pressed.

**Color through imagery only.** Buttons, headers, cards, nav — all neutral. Vibrancy lives in abstract gradient photography (soft peach/orange, purple/blue, teal/yellow blurs), not UI elements.

**Exception**: Product/model identity cards use gradient fills (pink/purple, lavender/blue, etc.) because the gradient IS the product's visual identity — not decoration.

**Never**: cyan-on-dark, neon accents, gradient text, colored buttons, dark mode with glowing elements.

```css
:root {
  --bg: #FFFFFF;
  --bg-subtle: #FAFAFA;
  --bg-muted: #F4F4F4;
  --fg: #0D0D0D;
  --fg-muted: #6E6E6E;
  --fg-faint: #ABABAB;
  --border: #E5E5E5;
  --surface: #F4F4F4;
  --surface-hover: #EBEBEB;
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --radius-pill: 999px;
}
```

---

## Typography

### The typeface: OpenAI Sans

OpenAI's brand typeface is **OpenAI Sans** — a custom geometric humanist sans that blends precision with rounded, approachable character. Key traits:
- Smoother curves and bespoke letterforms than standard geometric sans
- Friendly, circular appearance — not cold or technical
- Designed for both digital and print; includes ligatures, tabular figures, case-sensitive punctuation
- Five weights: **Light, Regular, Medium, Semibold, Bold** — each with a matching italic

OpenAI Sans is not publicly available. Use these substitutes ranked by closeness:

1. **Söhne** (Klim Type Foundry) — closest match, premium license
2. **Inter** — free, very close; slightly less rounded but excellent
3. **DM Sans** — free, more rounded and approachable, good for product UI
4. **Plus Jakarta Sans** — free, similar geometric-humanist balance

```css
/* Production font stack */
font-family: 'OpenAI Sans', 'Söhne', 'Inter', system-ui, -apple-system, sans-serif;

/* Free fallback stack */
font-family: 'Inter', 'DM Sans', system-ui, -apple-system, sans-serif;
```

**Never**: Roboto, Arial, Open Sans, or any monospace font for UI text.

### Weight usage

| Weight | CSS value | Use for |
|---|---|---|
| Light (300) | `font-weight: 300` | Large display text where delicacy is intentional |
| Regular (400) | `font-weight: 400` | Body, card headlines, nav links, mega-menu primary links, sub-nav |
| Medium (500) | `font-weight: 500` | Active nav items, button labels, segmented tab labels |
| Semibold (600) | `font-weight: 600` | Section headings (left-aligned), sub-nav category headers |
| Bold (700) | `font-weight: 700` | Hero/page headings, metadata category labels |

**Key insight**: mega-menu primary links are 400 (Regular) at ~44px. Size does the work, not weight. Increasing weight would make them feel aggressive, not premium.

### Type scale

```
Hero heading:             56–80px / 700 / tracking -0.02em / line-height 1.05
Large centered heading:   36–48px / 700 / tracking -0.02em / line-height 1.1
Section heading:          20–28px / 600 / tracking 0 / line-height 1.3
Mega-menu primary links:  40–48px / 400 / tracking 0 / line-height 1.15
Sub-nav category label:   13px    / 700 / color: var(--fg-muted)
Sub-nav / body links:     14–15px / 400
Card headline:            16–18px / 400 / line-height 1.4
Body / subtext:           16–17px / 400 / line-height 1.6 / color: var(--fg-muted)
Metadata label:           12–13px / 700 / color: var(--fg)
Metadata detail:          12–13px / 400 / color: var(--fg-muted)
Eyebrow / breadcrumb:     14px    / 400 / centered / color: var(--fg-muted)
Image caption:            13px    / 400 / color: var(--fg-muted)
```

### Typography rules

**Metadata pattern**: `<strong>Category</strong> · Date · N min read` — bold category, muted separator (·), muted date. One line.

**Inline links in body text**: underline only, no color change. The underline is the only affordance needed.

**Italics**: Used sparingly for editorial emphasis, never for UI labels or headings.

**DO**: Large, bold, confident headings. Let size and weight do the work — never color.
**DON'T**: Colored headings, underlines on headings, gradient text, decorative all-caps, letter-spacing on body text.

---

## Spacing

Generous is never enough. Double what feels right.

```
Section vertical gap:         80–120px
Card grid gap:                24–32px
Card internal padding:        0 (images edge-to-edge; text below with no container)
Section header margin-bottom: 32–40px
Input padding:                16–20px horizontal, 14–16px vertical
Pill / button padding:        10–14px horizontal, 8–12px vertical
Nav height:                   ~60px
Logo bar vertical padding:    48–64px
```

**Rule**: If content looks a little sparse, it's probably right. If it looks airy and editorial, it's definitely right.

---

## Shapes and Borders

Rounded everywhere. Never sharp. Never circular for content images.

| Element | Border-radius |
|---|---|
| Content images (cards, thumbnails) | 12px |
| Hero images | 16px |
| Product demo frames | 16px |
| Input fields | 16px |
| CTA buttons (pill) | 999px |
| Floating label chips (on images) | 12–14px |
| Suggestion/filter pills | 999px |
| Segmented control (outer) | 999px |
| Segmented control (active tab) | 999px |
| Send/submit icon buttons | 50% (circle) |
| Nav CTA buttons | 999px |

No box-shadows on cards. Depth comes from image contrast against white, or from white-on-gray (segmented controls).

---

## Core Components

### Navigation bar

```html
<nav class="nav">
  <a class="nav-logo" href="/">OpenAI</a>
  <div class="nav-links">
    <a href="#">Research</a>
    <a href="#" class="active">Products</a>
    <a href="#">Business</a>
    <a href="#">Developers</a>
    <a href="#">Company</a>
    <a href="#">Foundation</a>
    <button class="nav-search" aria-label="Search">
      <svg><!-- search icon --></svg>
    </button>
  </div>
  <div class="nav-auth">
    <button class="btn-ghost">Log in <svg><!-- caret --></svg></button>
    <button class="btn-primary">Try ChatGPT ↗</button>
  </div>
</nav>
```

```css
.nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 32px;
  height: 60px;
  background: var(--bg);
  position: sticky;
  top: 0;
  z-index: 100;
  border-bottom: 1px solid transparent; /* becomes var(--border) on scroll */
}

.nav-logo {
  font-size: 17px;
  font-weight: 700;
  color: var(--fg);
  text-decoration: none;
}

.nav-links {
  display: flex;
  align-items: center;
  gap: 28px;
}

.nav-links a {
  font-size: 14px;
  font-weight: 400;
  color: var(--fg);
  text-decoration: none;
}

.nav-links a.active { font-weight: 600; }
.nav-links a:hover { color: var(--fg-muted); }

.nav-auth { display: flex; align-items: center; gap: 8px; }
```

### CTA buttons

Three variants — always pill-shaped:

```html
<!-- Primary: solid black -->
<button class="btn-primary">Contact sales</button>

<!-- Secondary: outline -->
<button class="btn-outline">Start building ∨</button>

<!-- Tertiary: text + chevron -->
<a class="btn-text" href="#">Our Charter <span>›</span></a>
```

```css
.btn-primary {
  background: var(--fg);
  color: #FFFFFF;
  border: none;
  border-radius: 999px;
  padding: 10px 20px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  white-space: nowrap;
}

.btn-primary:hover { background: #1a1a1a; }

.btn-outline {
  background: transparent;
  color: var(--fg);
  border: 1px solid var(--fg);
  border-radius: 999px;
  padding: 10px 20px;
  font-size: 14px;
  font-weight: 400;
  cursor: pointer;
}

.btn-outline:hover { background: var(--surface); }

/* Ghost — used in nav for "Log in" */
.btn-ghost {
  background: transparent;
  color: var(--fg);
  border: 1px solid var(--border);
  border-radius: 999px;
  padding: 8px 16px;
  font-size: 14px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 4px;
}

.btn-text {
  font-size: 14px;
  color: var(--fg);
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  gap: 4px;
}
```

**Button pair pattern**: primary (solid black) on the left, secondary (outline or ghost) on the right. Never two solid buttons side by side.

**`white-space: nowrap` is mandatory on every button.** Button text must never wrap to a second line under any circumstance. If the label is too long, shorten the label — never let the button grow tall.

### Segmented control / tab switcher

Used to switch between content modes (Voice / Video / Image). The container is a muted pill; the active tab is a white pill floating inside.

```html
<div class="seg-control" role="tablist">
  <button class="seg-tab active" role="tab">Voice</button>
  <button class="seg-tab" role="tab">Video</button>
  <button class="seg-tab" role="tab">Image</button>
</div>
```

```css
.seg-control {
  display: inline-flex;
  background: var(--bg-muted); /* #F4F4F4 */
  border-radius: 999px;
  padding: 4px;
  gap: 2px;
}

.seg-tab {
  border: none;
  background: transparent;
  border-radius: 999px;
  padding: 8px 20px;
  font-size: 14px;
  font-weight: 400;
  color: var(--fg-muted);
  cursor: pointer;
  transition: all 120ms ease;
}

.seg-tab.active {
  background: var(--bg); /* white */
  color: var(--fg);
  font-weight: 500;
  /* subtle lift — optional, very faint */
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
}
```

**Rule**: The segmented control is NOT a toggle switch (no sliding animation needed). The active pill is simply white on the gray container. Content below updates to match selection.

### On/off toggle switch

For boolean settings, use a minimal pill toggle — dark when on, muted gray when off:

```html
<label class="toggle">
  <input type="checkbox" role="switch">
  <span class="toggle-track">
    <span class="toggle-thumb"></span>
  </span>
  <span class="toggle-label">Enable feature</span>
</label>
```

```css
.toggle {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
}

.toggle input { position: absolute; opacity: 0; width: 0; height: 0; }

.toggle-track {
  width: 44px;
  height: 24px;
  background: var(--surface-hover); /* off state */
  border-radius: 999px;
  position: relative;
  transition: background 150ms ease;
  flex-shrink: 0;
}

.toggle input:checked + .toggle-track {
  background: var(--fg); /* on = near-black */
}

.toggle-thumb {
  position: absolute;
  top: 3px;
  left: 3px;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: white;
  transition: transform 150ms ease;
  box-shadow: 0 1px 3px rgba(0,0,0,0.15);
}

.toggle input:checked + .toggle-track .toggle-thumb {
  transform: translateX(20px);
}

.toggle-label {
  font-size: 14px;
  color: var(--fg);
}
```

**Toggle rules**:
- Off: gray track (`#EBEBEB`), white thumb
- On: near-black track (`#0D0D0D`), white thumb
- Never use a brand color (blue, green) for the on state — black only
- Size: 44×24px standard; 36×20px compact
- Always pair with a text label — never a standalone toggle

### Mega-menu — simple product list

```html
<div class="mega-menu">
  <a class="mega-link" href="#">ChatGPT <span class="ext">↗</span></a>
  <a class="mega-link" href="#">Sora</a>
  <a class="mega-link" href="#">Atlas <span class="ext">↗</span></a>
  <a class="mega-link" href="#">Codex</a>
  <a class="mega-link" href="#">Prism</a>
</div>
```

```css
.mega-menu {
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 24px 32px 32px;
  background: var(--bg);
}

.mega-link {
  font-size: 44px;
  font-weight: 400;
  color: var(--fg);
  text-decoration: none;
  line-height: 1.15;
  display: flex;
  align-items: center;
  gap: 8px;
}

.mega-link:hover { color: var(--fg-muted); }

.ext {
  font-size: 28px;
  font-weight: 300;
  color: var(--fg-muted);
}
```

**Rule**: Size IS the navigation signal here. No bullets, no borders, no icons. The `↗` arrow is the only indicator that a product is external/launching separately.

### Mega-menu — complex (3-column)

```html
<div class="mega-complex">
  <!-- Left: large primary links -->
  <nav class="mega-primary">
    <a class="mega-link" href="#">Overview</a>
    <a class="mega-link" href="#">ChatGPT Pricing</a>
    <a class="mega-link" href="#">API Pricing</a>
    <a class="mega-link" href="#">Customer Stories</a>
    <a class="mega-link" href="#">Resources</a>
    <a class="mega-link" href="#">Contact Sales</a>
  </nav>

  <!-- Middle: sub-nav with category header -->
  <nav class="mega-sub">
    <p class="mega-sub-label">Products</p>
    <a href="#">ChatGPT Business ↗</a>
    <a href="#">ChatGPT Enterprise ↗</a>
    <a href="#">Codex</a>
    <a href="#">OpenAI Frontier</a>
    <a href="#">API Platform</a>
  </nav>

  <!-- Right: sub-nav with category header -->
  <nav class="mega-sub">
    <p class="mega-sub-label">Solutions</p>
    <a href="#">Coding</a>
    <a href="#">Agents</a>
    <a href="#">Healthcare</a>
    <a href="#">Financial Services</a>
    <a href="#">All Solutions</a>
  </nav>
</div>
```

```css
.mega-complex {
  display: grid;
  grid-template-columns: 1fr 200px 200px;
  gap: 48px;
  padding: 24px 32px 40px;
  background: var(--bg);
  align-items: start;
}

/* Primary links: same as simple mega-menu above */

.mega-sub-label {
  font-size: 13px;
  font-weight: 700;
  color: var(--fg-muted);
  margin-bottom: 12px;
  text-transform: none;
}

.mega-sub a {
  display: block;
  font-size: 14px;
  font-weight: 400;
  color: var(--fg);
  text-decoration: none;
  padding: 4px 0;
}

.mega-sub a:hover { color: var(--fg-muted); }
```

### Page hero (centered, eyebrow + heading)

```html
<section class="page-hero">
  <p class="eyebrow">Company</p>
  <h1>About</h1>
  <p class="hero-sub">OpenAI is an AI research and deployment company. Our mission is to ensure that artificial general intelligence benefits all of humanity.</p>
</section>
```

```css
.page-hero {
  text-align: center;
  padding: 80px 24px 48px;
  max-width: 640px;
  margin: 0 auto;
}

.eyebrow {
  font-size: 14px;
  color: var(--fg-muted);
  margin-bottom: 16px;
}

.page-hero h1 {
  font-size: 72px;
  font-weight: 700;
  letter-spacing: -0.02em;
  line-height: 1.05;
  color: var(--fg);
  margin-bottom: 24px;
}

.hero-sub {
  font-size: 17px;
  color: var(--fg-muted);
  line-height: 1.6;
  max-width: 520px;
  margin: 0 auto;
}
```

### Asymmetric 2-col content section (text + image)

Left column: ~40% (text, heading, body, CTAs). Right column: ~55% (large rounded image). Not equal halves.

```html
<section class="split-section">
  <div class="split-text">
    <h2>Our vision for the future of AGI</h2>
    <p>Our mission is to ensure that artificial general intelligence — AI systems that are generally smarter than humans — benefits all of humanity.</p>
    <div class="split-ctas">
      <button class="btn-outline">Our plan for AGI</button>
      <a class="btn-text" href="#">Our Charter ›</a>
    </div>
  </div>
  <figure class="split-image">
    <img src="..." alt="Abstract painting">
    <figcaption>Illustration: Justin Jay Wang × DALL·E</figcaption>
  </figure>
</section>
```

```css
.split-section {
  display: grid;
  grid-template-columns: 2fr 3fr;
  gap: 80px;
  align-items: center;
  padding: 80px 0;
  max-width: 1100px;
  margin: 0 auto;
}

.split-text h2 {
  font-size: 28px;
  font-weight: 600;
  color: var(--fg);
  margin-bottom: 16px;
  line-height: 1.3;
}

.split-text p {
  font-size: 16px;
  color: var(--fg-muted);
  line-height: 1.6;
  margin-bottom: 28px;
}

.split-ctas {
  display: flex;
  align-items: center;
  gap: 16px;
}

.split-image img {
  width: 100%;
  border-radius: 16px;
  display: block;
}

.split-image figcaption {
  margin-top: 10px;
  font-size: 13px;
  color: var(--fg-muted);
}
```

### Logo bar (customer logos)

No label. No color. Logos speak for themselves in grayscale.

```html
<div class="logo-bar">
  <img src="lowes.svg" alt="Lowe's">
  <img src="morgan-stanley.svg" alt="Morgan Stanley">
  <img src="booking.svg" alt="Booking.com">
  <img src="amgen.svg" alt="Amgen">
  <img src="mercado-libre.svg" alt="Mercado Libre">
</div>
```

```css
.logo-bar {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 64px;
  padding: 48px 0;
  flex-wrap: wrap;
}

.logo-bar img {
  height: 28px;
  width: auto;
  opacity: 0.7;
  filter: grayscale(100%);
  object-fit: contain;
}

.logo-bar img:hover { opacity: 1; }
```

### Portrait gradient product/model cards

Used for model families (GPT-5.4, mini, nano). The gradient IS the product identity. Text overlaid at bottom-left in white.

```html
<div class="model-grid">
  <div class="model-card" style="--grad: linear-gradient(135deg, #E87FBE 0%, #C4A8F0 40%, #A8C8F0 100%);">
    <div class="model-info">
      <p class="model-name">GPT-5.4</p>
      <p class="model-price">Input: $2.50 per 1M tokens</p>
    </div>
  </div>
  <!-- repeat for mini, nano -->
</div>
```

```css
.model-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}

.model-card {
  background: var(--grad);
  border-radius: 16px;
  aspect-ratio: 3 / 4;
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: flex-end;
}

.model-info {
  padding: 24px;
}

.model-name {
  font-size: 20px;
  font-weight: 400;
  color: white;
  margin-bottom: 4px;
}

.model-price {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.75);
}
```

**Model gradient palette**:
- GPT flagship: `linear-gradient(135deg, #E87FBE, #C4A8F0, #A8D0F8)` — pink to lavender to sky
- Mini variant: `linear-gradient(135deg, #F4A06A, #D4A8F0, #A8B8F0)` — peach to lavender
- Nano variant: `linear-gradient(135deg, #B8A8F0, #A8B8F8, #C0D0F8)` — cool lavender to pale blue

### Product demo frame

Used to showcase a product screenshot inside a soft gradient container — not a browser frame, just a floating card effect.

```html
<div class="demo-frame">
  <img src="product-screenshot.png" alt="Product demo" class="demo-screenshot">
</div>
<p class="demo-caption">Zillow makes home and financing searches easier with voice using the Realtime API.</p>
```

```css
.demo-frame {
  background: linear-gradient(135deg, #B8C8F8 0%, #D0B8F0 40%, #F4D0A0 100%);
  border-radius: 16px;
  padding: 40px 40px 0;
  overflow: hidden;
  margin: 40px auto;
  max-width: 900px;
}

.demo-screenshot {
  width: 100%;
  border-radius: 12px 12px 0 0; /* rounded top, flush bottom into container */
  display: block;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
}

.demo-caption {
  text-align: center;
  font-size: 13px;
  color: var(--fg-muted);
  margin-top: 16px;
}
```

### Large text input (ChatGPT-style)

```html
<div class="input-wrap">
  <textarea placeholder="Ask anything..." rows="1"></textarea>
  <button class="send-btn" aria-label="Send">↑</button>
</div>
```

```css
.input-wrap {
  position: relative;
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 16px 56px 16px 20px;
  background: var(--bg);
  max-width: 740px;
  width: 100%;
}

.input-wrap textarea {
  width: 100%;
  border: none;
  outline: none;
  resize: none;
  font-size: 16px;
  color: var(--fg);
  background: transparent;
}

.send-btn {
  position: absolute;
  right: 12px;
  bottom: 12px;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: var(--fg);
  color: white;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
}
```

### Suggestion pills

```css
.pills { display: flex; flex-wrap: wrap; gap: 8px; justify-content: center; }

.pill {
  border: 1px solid var(--border);
  border-radius: 999px;
  padding: 8px 16px;
  font-size: 14px;
  color: var(--fg);
  background: var(--bg);
  cursor: pointer;
  transition: background 120ms ease;
}

.pill:hover { background: var(--surface); }
```

### Section header with utility link

```css
.section-header {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  margin-bottom: 32px;
}

.section-header h2 { font-size: 22px; font-weight: 600; color: var(--fg); }
.section-header a { font-size: 14px; color: var(--fg-muted); text-decoration: none; }
.section-header a:hover { color: var(--fg); }
```

### News/article list card (thumbnail left, text right)

```css
.news-card { display: flex; gap: 24px; align-items: flex-start; }

.news-thumb {
  width: 160px; height: 160px;
  border-radius: 12px;
  flex-shrink: 0; overflow: hidden;
  background: var(--surface);
}

.news-thumb img { width: 100%; height: 100%; object-fit: cover; }

.news-body h3 { font-size: 17px; font-weight: 400; color: var(--fg); line-height: 1.4; margin-bottom: 8px; }

.meta { font-size: 13px; color: var(--fg-muted); }
.meta strong { color: var(--fg); font-weight: 700; }
```

### Hero card with floating frosted label (blog hero)

```css
.hero-img { position: relative; border-radius: 16px; overflow: hidden; aspect-ratio: 16/9; }
.hero-img img { width: 100%; height: 100%; object-fit: cover; }

.hero-label {
  position: absolute; top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  background: rgba(255,255,255,0.88);
  backdrop-filter: blur(12px);
  border-radius: 14px;
  padding: 12px 24px;
  display: flex; align-items: baseline; gap: 8px;
  white-space: nowrap;
}

.label-title { font-size: 28px; font-weight: 700; color: var(--fg); }
.label-sub { font-size: 28px; font-weight: 400; color: var(--fg-muted); }
```

---

## Abstract Gradient Thumbnails

When real photography isn't available, use CSS abstract gradients — they should feel like out-of-focus macro photography, not geometric patterns.

```css
.grad-warm    { background: radial-gradient(ellipse at 30% 40%, #F9A86A 0%, #F4709A 50%, #C4A8F0 100%); }
.grad-cool    { background: radial-gradient(ellipse at 60% 30%, #6E8FF0 0%, #7C5CCC 50%, #3B5B99 100%); }
.grad-nature  { background: radial-gradient(ellipse at 40% 60%, #B8E04A 0%, #44C9AA 50%, #5B9BE8 100%); }
.grad-warm2   { background: radial-gradient(ellipse at 50% 30%, #F4D06A 0%, #F09060 40%, #E06080 100%); }
```

---

## Layout Templates

### Centered hero (ChatGPT / product homepage)
- `max-width: 680px`, centered, `padding: 140px 24px 80px`
- Heading centered, large (56–80px), bold
- Input below, full width of container
- Pills below input, centered, `margin-top: 16px`

### Segmented tab hero
- Centered heading above
- Segmented control (inline-flex, centered) below heading
- Body text below control, centered, max-width 560px
- Demo frame below body text, full container width

### Editorial blog index
- Left column `flex: 1`: large hero card with floating label
- Right column `280px` fixed: stacked smaller blog cards

### News list grid
- `grid-template-columns: 1fr 1fr`, gap `48px`
- Each cell: thumb-left + text-right

### Story / editorial grid
- `grid-template-columns: repeat(3, 1fr)`, gap `24px`
- Large images, short captions only

### Product page with model cards
- Centered heading + subtext
- `grid-template-columns: repeat(3, 1fr)` portrait-ratio gradient cards

### Platform / API page
- Full-width centered hero, `padding-top: 120px`
- CTA pair centered below heading
- Logo bar below CTAs
- Then model cards grid

---

## What to Never Do

- Card borders (1px outline boxes on cards)
- Drop shadows on cards (not even subtle ones)
- Colored primary buttons — black only
- Blue/green on-state for toggles — black only
- Gradient text or gradient headings
- Icons next to every heading
- Dark backgrounds (except deliberate modal overlays or product gradient cards)
- Circular images for editorial content
- Colored badges, status dots, or accent pills
- Animated looping gradient backgrounds
- Equal 50/50 columns — almost always asymmetric (40/55 or 35/65)
- "Trusted by" label above logo bars — logos speak alone

---

## Checklist before shipping

- [ ] Backgrounds: white, `#FAFAFA`, or `#F4F4F4` only
- [ ] No card shadows
- [ ] Images: `border-radius: 12px` minimum
- [ ] Section headers: bold left, muted link right, same baseline
- [ ] Metadata: bold category + muted date pattern
- [ ] Input: rounded 16px, hairline border, circular send button
- [ ] Toggle on-state: near-black track, never brand color
- [ ] Segmented control: gray outer pill, white active tab
- [ ] CTA buttons: solid black primary + outline secondary — never two solids
- [ ] Spacing feels almost too generous — good
- [ ] No color in UI chrome — color only in imagery or product-identity gradient cards
- [ ] Type hierarchy: 700 hero / 600 section / 400 card headline / 700+400 meta
- [ ] Mega-menu primary links: oversized regular weight (~44px), not bold
