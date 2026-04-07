---
name: boundless-base-theme
description: Shared Boundless aesthetic for all sites — tech-forward developer audience. Two-layer system: this is the base, project DESIGN.md extends it.
---

# Boundless Base Theme

Shared aesthetic foundation for all Boundless-built websites. Every project extends this with its own personality layer via a project DESIGN.md.

## Audience

Developers, protocol engineers, crypto-native builders. They value clarity over flash, substance over decoration. They're skeptical of marketing sites and respect ones that respect their intelligence.

## Core Palette

```css
:root {
  /* Backgrounds */
  --bg:           #FFFFFF;
  --bg-subtle:    #FAFAFA;
  --bg-muted:     #F4F4F4;

  /* Foreground */
  --fg:           #0D0D0D;
  --fg-muted:     #6E6E6E;
  --fg-faint:     #ABABAB;

  /* Borders */
  --border:       #E5E5E5;

  /* Surfaces */
  --surface:      #F4F4F4;
  --surface-hover: #EBEBEB;

  /* Radii */
  --radius-sm:    8px;
  --radius-md:    12px;
  --radius-lg:    16px;
  --radius-pill:  999px;
}
```

Dark mode inverts the scale. `--bg` becomes `#0D0D0D`, `--fg` becomes `#FAFAFA`. Borders go to `#2A2A2A`. Surfaces go to `#1A1A1A`.

## Typography

**Font stack**: `'Inter', 'DM Sans', 'Plus Jakarta Sans', system-ui, -apple-system, sans-serif`

Max 2 fonts per site. Display font may vary per project; body stays Inter.

**Never use**: Roboto, Arial, Open Sans, monospace for UI text.

### Type Scale
```
Hero heading:       56–80px / 700 / tracking -0.02em / line-height 1.05
Section heading:    20–28px / 600 / line-height 1.3
Body:               16–17px / 400 / line-height 1.6 / color: var(--fg-muted)
Metadata:           12–13px / 700 category + 400 date / color: var(--fg-muted)
Nav links:          14px / 400
Button labels:      14px / 500
```

**Metadata pattern**: `<strong>Category</strong> · Date` — bold category, muted separator, muted date.

## Spacing

Double what feels right.

```
Section gap:          80–120px
Card grid gap:        24–32px
Section header mb:    32–40px
Nav height:           60px
Input padding:        16–20px h, 14–16px v
Button padding:       10–14px h, 8–12px v
Logo bar padding:     48–64px v
```

## Components

### Buttons
- **Primary**: pill-shaped (999px radius), solid near-black (`var(--fg)`), white text
- **Secondary**: pill-shaped, outline with `var(--fg)` border
- **Ghost**: pill-shaped, hairline `var(--border)` border
- **Text link**: underline only, no color change
- Never two solid buttons side by side
- `white-space: nowrap` mandatory on every button

### Navigation
- Sticky, 60px height, hairline border on scroll
- Links at 14px / 400 weight
- Active link at 600 weight

### Cards
- **No borders. No shadows.** Depth from image contrast against white.
- Images min 12px border-radius, 16px for hero/demo

### Inputs
- Rounded 16px, hairline border, circular send button

### Segmented Control
- Gray outer pill, white active tab, no sliding animation

### Toggle
- Near-black on-state. Never brand color.

### Logo Bar
- Grayscale, 0.7 opacity, no "Trusted by" label

## Layout Principles

- **Asymmetric always**: 40/55 or 35/65 column splits. Never 50/50.
- **Max 10 sections per page** — scroll fatigue kills after that
- **Mobile-first**: Start single column, add columns at breakpoints
- **Max content width**: 1080–1100px

## Color Philosophy

Color through imagery only. UI chrome (buttons, nav, cards, borders) stays neutral. Vibrancy lives in photography, abstract gradients, or product-identity elements — never in chrome.

**Exception**: Product-identity gradient cards where the gradient IS the identity.

## Animation Defaults

See full reference: `~/.claude/design-library/references/animation-stack.md`

**Every project installs:**
- `motion` — React animation layer (page transitions, entrances, gestures)
- `lenis` — smooth scroll with physics-based momentum
- `@chenglou/pretext` — DOM-free text measurement (CLS-free hero animations, OG image generation)

**Marketing/landing pages also install:**
- `gsap` + `@gsap/react` — scroll-triggered sequences, text reveals, pinned sections

**Default motion style:**
- Easing: exponential out (`[0.16, 1, 0.3, 1]` in Motion, `power3.out` in GSAP)
- Section entrances: fade-up, 400-600ms, triggered on viewport intersection
- Stagger gap: 50-100ms between child elements
- Hover: background shift only, 100-150ms
- Smooth scroll duration: 1200ms

**Never:** bounce easing, elastic easing, linear for UI, scroll-jacking, parallax that moves content (subtle background parallax is fine).

## What Makes It Boundless

Projects extend this base with:
- A display font that gives personality (DM Sans for approachable, Plus Jakarta for modern, etc.)
- Accent color(s) drawn from imagery/brand, used sparingly (never on chrome)
- Motion style (subtle or bold, defined per project)
- Unique component variants that break from the base where it serves the story

## Never Do (immutable across all projects)

- Card borders or card shadows
- Colored primary buttons — solid near-black only
- Gradient text or gradient headings
- Icons next to every heading
- Dark backgrounds for UI chrome
- Equal 50/50 columns
- Animated looping gradient backgrounds
- "Trusted by" labels above logo bars
- Cyan-on-dark, neon accents, glassmorphism
- Identical card grids (vary sizes, break the grid)
- Generic centered hero with gradient bg + "Start Building" CTA
- Roboto, Arial, Open Sans
- Bounce or elastic easing
- Monospace as lazy "developer" shorthand
