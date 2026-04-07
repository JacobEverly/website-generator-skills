# DESIGN.md — Bold Marketing

> Extends [Boundless Base Theme](../boundless-base.md)
> Example: High-impact product launch page

## Project Identity

**Name**: Prism Analytics
**Tagline**: See what your data is actually telling you
**Audience**: Growth-stage startup founders and heads of product who are frustrated with complex BI tools
**Goal**: Book a demo — the product requires onboarding, so self-serve isn't the path

## Personality Layer

### Display Font
**DM Sans** (700/800 weight for display) — rounded and confident, feels approachable but not childish. The roundness contrasts with the sharp precision of the data product.
- Where to use: Hero heading (80px), section titles (28px), CTA headings
- Where NOT to use: Body, nav, metadata (use Inter)

### Accent Palette
```css
--accent-primary: #7C3AED;    /* Violet — used in the hero image gradient, data visualization accents in product screenshots, and the demo frame background. Never on buttons or UI chrome. */
--accent-secondary: #F97316;  /* Orange — used as a contrast pop in data viz highlights and the "new" pill on announcements. */
```

### Motion Style
**Moderate** — this is a marketing page, motion creates excitement and anticipation.
- Page load: Elements slide up with 30px translate, 500ms cubic-bezier(0.16, 1, 0.3, 1), staggered 100ms
- Hover effects: Cards lift slightly (translateY -4px), 200ms. Buttons darken.
- Scroll: Sections fade in on scroll intersection (IntersectionObserver, threshold 0.2). Product screenshots have subtle parallax (10px range).
- Timing: Medium — 200ms for interactions, 500ms for entrances

### Unique Components
- **Product demo frame**: Violet-to-blue gradient background with a product screenshot floating inside, rounded-2xl, subtle shadow for depth. The screenshot is the star — the gradient provides context/energy.
- **Metric callout**: Large number (48px, DM Sans 800) in near-black with a small label below (13px, Inter 400, muted). Not a card — just typography on white. Asymmetric placement (left-aligned, not centered).
- **Announcement pill**: Small pill at top of hero with orange accent dot + text. Links to blog/changelog.

## Section Plan
1. **Nav** — Logo + 3 links + "Book a demo" primary CTA
2. **Hero** — Announcement pill above, left-aligned heading (80px) with subtext, "Book a demo" button + "Watch the 2-min overview" text link. Right side: product demo frame with screenshot. Asymmetric 40/55.
3. **Metrics bar** — 3 metric callouts in a row (e.g., "10x faster", "50% less churn", "$2M saved"). Left-aligned, not centered. Separated by generous space, not cards.
4. **Product walkthrough** — 3 sections, alternating asymmetric layouts (text left / screenshot right, then flipped). Each shows a key feature with a heading, 2-sentence description, and product screenshot.
5. **Integrations** — Logo bar, grayscale, no label. 8-10 logos.
6. **Social proof** — One large pull quote (28px, DM Sans 600, near-black) with attribution below. Not a testimonial card — editorial typography on white.
7. **CTA** — "See Prism in action" heading + "Book a demo" button + "No credit card required" subtext. Generous spacing (120px top/bottom).
8. **Footer** — 3-column links + copyright

## Image Direction
- Hero: Product screenshot in a demo frame with violet gradient surround
- Section images: Product screenshots showing real UI, each highlighting a different feature
- Icons: None — typography and screenshots carry the page
- OG: Violet gradient background, "Prism Analytics" in DM Sans white, product screenshot peek

## Copy Voice
- Headlines: Bold claims. "See what your data is actually telling you." Direct, slightly provocative.
- Body: Conversational but smart. Acknowledge the pain ("You've tried the dashboards. They don't help.") then present the solution.
- CTAs: Personal and low-friction. "Book a demo" (not "Request a demo"). "Watch the 2-min overview" (not "Learn more").

## Hard Rules
All base theme "Never Do" rules, plus:
- Product screenshots must look realistic — never mockups or wireframes
- Maximum 3 metric callouts (more dilutes impact)
- Only ONE testimonial quote — curation signals confidence
- Demo frame gradient uses accent palette only, not arbitrary colors
- No feature comparison tables — this is a marketing page, not a spec sheet
