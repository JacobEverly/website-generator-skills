---
name: capture-inspo
description: Capture design inspiration from URLs, screenshots, or descriptions. Analyzes visual patterns and stores structured references in the global design library for future projects.
user-invokable: true
args:
  - name: source
    description: URL to capture, path to a screenshot, or text description of design patterns
    required: true
  - name: tags
    description: Comma-separated tags for categorization (e.g. "saas, dark, minimal")
    required: false
---

Capture and analyze design inspiration from any source, storing structured references in the global design library at `~/.claude/design-library/inspirations/`.

## Step 1: Determine Source Type

The `source` argument can be:
1. **URL** — fetch and analyze the live site
2. **File path** — read and analyze a screenshot image
3. **Text description** — analyze the described design patterns

## Step 2: Capture the Design

### For URLs

Use Playwright MCP to capture the full experience:

1. Navigate to the URL with `browser_navigate`
2. Take a full-page screenshot with `browser_take_screenshot`
3. Take a snapshot of the DOM with `browser_snapshot` to extract structure
4. Analyze the visual design from the screenshot
5. Extract design patterns from the DOM snapshot (colors, fonts, spacing, components)

If Playwright is unavailable, fall back to `WebFetch` to get the HTML and analyze the markup + inline styles.

### For Screenshots

Read the image file directly. Analyze the visual design patterns visible in the screenshot.

### For Text Descriptions

Parse the description and structure it into the standard format below.

## Step 3: Analyze Design Patterns

Extract these design dimensions:

### Color
- Background colors (primary, secondary, accent backgrounds)
- Text colors (primary, secondary, muted)
- Accent/brand colors and how they're used
- Border and divider colors
- Whether color is used on chrome or only in imagery

### Typography
- Font families (identify by visual characteristics if not extractable)
- Size scale (hero, heading, body, metadata)
- Weight usage patterns
- Letter-spacing and line-height choices
- How hierarchy is created (size vs weight vs color)

### Layout
- Max content width
- Column structure and ratios (note if asymmetric)
- Section spacing
- Grid patterns
- Responsive approach visible

### Components
- Button styles (shape, color, states)
- Card treatments (borders? shadows? image handling?)
- Navigation pattern
- Input styles
- Any unique/distinctive components

### Motion (if observable)
- Page load animations
- Hover effects
- Scroll-triggered animations
- Transition timing and easing

### What Makes It Work
- The 2-3 design decisions that make this site distinctive
- What would be lost if you made it "normal"
- Why a designer would respect this site

## Step 4: Store the Inspiration

Create a markdown file at `~/.claude/design-library/inspirations/{slug}.md` with this format:

```markdown
---
name: {descriptive-slug}
source: {url, file path, or "description"}
tags: [{comma-separated tags}]
captured: {YYYY-MM-DD}
---

## Visual Analysis

### Color
- {color observations}

### Typography
- {typography observations}

### Layout
- {layout observations}

### Components
- {component observations}

### Motion
- {motion observations, if any}

## What Makes It Work
- {key insight 1}
- {key insight 2}
- {key insight 3}

## Reusable Patterns
- {any specific patterns worth extracting for future use}
```

### Naming Convention
- Use the site/brand name + specific page: `stripe-pricing.md`, `linear-homepage.md`, `vercel-dashboard.md`
- For screenshots without clear attribution: `dark-saas-dashboard.md`, `minimal-landing-page.md`

### Tagging
Apply relevant tags from these categories:
- **Type**: landing, dashboard, docs, pricing, blog, portfolio, saas, marketing
- **Aesthetic**: minimal, bold, dark, light, editorial, playful, premium, brutalist
- **Features**: animation-heavy, typography-led, image-heavy, data-dense, interactive
- **Color**: monochrome, vibrant, muted, gradient, high-contrast

If the user provided tags, use those. Otherwise, generate 3-6 tags from the analysis.

## Step 5: Update the Library Index

Add an entry to `~/.claude/design-library/LIBRARY.md` under the `## Inspirations` section:

```markdown
- [{name}](inspirations/{slug}.md) — {one-line summary} `{tags}`
```

## Step 6: Check for Pattern Extraction

After storing, count how many inspirations share a common tag (e.g., "pricing", "hero", "dashboard"). If 3+ inspirations share a category tag, suggest extracting a common pattern to `~/.claude/design-library/patterns/{category}.md`.

## Output

Confirm what was captured with:
- The file path where it was stored
- The key design insights (2-3 bullets)
- Tags applied
- Whether any pattern extraction is recommended
