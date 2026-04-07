# DESIGN.md — Dark Developer Tool

> Extends [Boundless Base Theme](../boundless-base.md)
> Example: Dark mode technical product (API, SDK, infrastructure)

## Project Identity

**Name**: Forge Runtime
**Tagline**: The execution layer your code deserves
**Audience**: Senior backend engineers and platform teams evaluating infrastructure. They read RFCs for fun and will dismiss anything that looks like marketing.
**Goal**: `npm install forge-runtime` — direct adoption, no sales call

## Personality Layer

### Dark Mode Override
This project uses dark mode as the default. Base theme colors invert:
```css
:root {
  --bg:           #0A0A0A;
  --bg-subtle:    #111111;
  --bg-muted:     #1A1A1A;
  --fg:           #FAFAFA;
  --fg-muted:     #888888;
  --fg-faint:     #555555;
  --border:       #2A2A2A;
  --surface:      #1A1A1A;
  --surface-hover: #252525;
}
```

### Display Font
**Inter** (700 weight for display) — in dark mode, Inter's geometric precision reads as technical authority. No display font needed — the restraint IS the personality.
- Where to use: Hero heading (64px, not 80px — dark mode headings feel larger), section titles
- Where NOT to use: N/A — Inter everywhere, consistency is the point

### Accent Palette
```css
--accent-primary: #3B82F6;    /* Blue-500 — used ONLY in code syntax highlighting, inline code backticks, and documentation links. Never on buttons, never as background, never on chrome. */
```
One accent color. That's it. The restraint is the identity.

### Motion Style
**Minimal** — this audience actively dislikes unnecessary animation.
- Page load: Instant. No entrance animations. Content is there when the page loads.
- Hover effects: Background color shift (`var(--surface)` → `var(--surface-hover)`), 100ms. Nothing else.
- Scroll: None. No parallax, no reveal, no fade-in.
- Timing: Fast — 100ms for everything

### Unique Components
- **Code block**: Dark bg (#0D1117, GitHub dark), syntax-highlighted, with a copy button (ghost style) top-right. Language label top-left in faint text. Rounded-lg. This is the primary visual element of the site.
- **Benchmark table**: Minimal table with hairline borders (`var(--border)`), monospace numbers right-aligned. Highlight row for Forge Runtime in slightly lighter surface color. No zebra striping.
- **Version badge**: Small pill showing current version `v2.4.1` in monospace, hairline border, next to the install command.

## Section Plan
1. **Nav** — Logo (wordmark only) + Docs / GitHub / Changelog links + install command as CTA (`npm install forge-runtime` in a code pill)
2. **Hero** — Left-aligned, 64px heading, 2-line subtext explaining what it does technically, install command code block below. No image. Asymmetric: text takes 55%, right 40% is empty (intentional negative space).
3. **Code example** — Full-width code block showing a real usage example (20+ lines). This IS the pitch.
4. **Benchmarks** — Heading + benchmark table comparing Forge vs alternatives. Numbers speak. No editorializing.
5. **Architecture** — Asymmetric 55/40 split. Left: text explaining the execution model (3 short paragraphs). Right: simple ASCII or SVG diagram (not a fancy illustration).
6. **Getting started** — 3 code blocks: install, configure, run. Step numbers in muted text. Minimal prose.
7. **Footer** — Single row: Logo + GitHub + Docs + License (MIT) + copyright

## Image Direction
- Hero: No image. Negative space is the design.
- Section images: ASCII diagrams or minimal SVG. Never illustrations, photos, or abstract graphics.
- Icons: None.
- OG: Black background (#0A0A0A), project name in Inter 700 white, version badge, one-line description in muted text.

## Copy Voice
- Headlines: Technical and precise. "Predictable latency at any scale." No marketing superlatives.
- Body: RFC-style clarity. Short sentences. Technical terms used correctly (not simplified). Assume the reader knows what a runtime is.
- CTAs: Commands, not marketing language. The CTA is literally `npm install forge-runtime`, not "Get started" or "Try for free".

## Hard Rules
All base theme "Never Do" rules, plus:
- No illustrations, abstract graphics, or photography — code and negative space only
- No testimonials — benchmarks are the social proof
- No pricing section — it's open source
- Accent color (blue) only in code syntax — nowhere else on the page
- Body text max 60ch line length for readability
- All code examples must be syntactically correct and runnable
- No "developer experience" or "DX" in copy — show, don't tell
