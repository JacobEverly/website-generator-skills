# DESIGN.md — Minimal SaaS

> Extends [Boundless Base Theme](../boundless-base.md)
> Example: Clean developer tool landing page

## Project Identity

**Name**: Acme CLI
**Tagline**: Ship faster from your terminal
**Audience**: Backend developers who live in the terminal and hate bloated GUIs
**Goal**: GitHub sign-up → first deploy in under 5 minutes

## Personality Layer

### Display Font
**Plus Jakarta Sans** (700 weight for display) — geometric but warm, not cold like a pure geometric sans. More personality than Inter but still feels like a developer tool.
- Where to use: Hero heading, section titles
- Where NOT to use: Body text, nav, metadata (use Inter)

### Accent Palette
```css
--accent-primary: #4F46E5;    /* Indigo — used only in code snippets, terminal highlights, and the hero image gradient. Never on buttons or chrome. */
--accent-secondary: #F59E0B;  /* Amber — used sparingly for "new" badges and terminal cursor blink. */
```

### Motion Style
**Subtle** — this audience distrusts gratuitous animation.
- Page load: Fade-in with 20px upward translate, 400ms ease-out, staggered 80ms per section
- Hover effects: Background color shift only (`var(--surface)` → `var(--surface-hover)`), 150ms
- Scroll: No parallax, no scroll-triggered animations
- Timing: Fast — 150ms for interactions, 400ms for entrance

### Unique Components
- **Terminal block**: Dark bg (#1E1E2E), monospace font, with a blinking cursor accent in amber. Rounded-xl. This is the hero element, not a generic code block.
- **Step counter**: Numbered steps (1, 2, 3) in near-black circles with white text, connected by a hairline vertical line. Not a stepper UI — editorial numbering.

## Section Plan
1. **Nav** — Logo + 4 links + GitHub CTA (outline button with GitHub icon)
2. **Hero** — Left-aligned heading + subtext + terminal block showing install command (asymmetric 40/55 split, terminal on right)
3. **How it works** — 3 steps, vertical layout with step counters, one code snippet each
4. **Key features** — 2-column asymmetric grid (35/65), feature name left, explanation + mini terminal right. Vary which side is larger.
5. **Integrations** — Logo bar of supported platforms. Grayscale, no label.
6. **CTA** — Centered, minimal. One heading, one button. Generous top/bottom spacing (120px).
7. **Footer** — Links + copyright. Single row.

## Image Direction
- Hero: No hero image. The terminal block IS the visual.
- Section images: Mini terminal screenshots showing real commands
- Icons: None — text and layout do the work
- OG: Dark background, project name in Plus Jakarta Sans, terminal cursor blinking amber

## Copy Voice
- Headlines: Short, imperative. "Ship faster." "Deploy in seconds." No questions, no cleverness.
- Body: Technical but not jargon-heavy. Assume the reader writes code daily.
- CTAs: Action verbs. "Get started", "View docs", "Install now"

## Hard Rules
All base theme "Never Do" rules, plus:
- No illustrations or abstract graphics — this is a terminal tool, the terminal IS the visual
- No testimonial cards — social proof via GitHub stars count only
- Maximum 2 colors beyond neutrals (indigo + amber)
- No emoji in copy
