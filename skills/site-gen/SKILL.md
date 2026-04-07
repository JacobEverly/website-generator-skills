---
name: site-gen
description: Generate a single production-grade page from DESIGN.md. Bakes in all lessons learned — no dynamic Tailwind, hydration fixes, accessibility, mobile-first, anti-slop guardrails.
user-invokable: true
args:
  - name: page
    description: What page to generate (e.g. "landing page", "pricing", "about"). Loose descriptions get enhanced.
    required: true
  - name: design-md
    description: Path to DESIGN.md (defaults to ./DESIGN.md)
    required: false
---

Generate a complete, production-grade single page following the project's DESIGN.md and Boundless base theme.

## Step 1: Load Design Context

1. Read the project's `DESIGN.md` (from `design-md` arg or `./DESIGN.md`)
2. Read the base theme: `~/.claude/design-library/boundless-base.md`
3. Read the tech UI reference: `~/.claude/design-library/references/techui.md`
4. Read the animation stack: `~/.claude/design-library/references/animation-stack.md`
5. Read the agent-friendly reference: `~/.claude/design-library/references/agent-friendly.md`
6. Read the Pretext reference: `~/.claude/design-library/references/pretext.md`
7. Read any project CLAUDE.md for additional design context

If no DESIGN.md exists, STOP and tell the user to run `/design-brief` first.

## Step 2: Enhance the Prompt

The user's page description is a starting point. Enrich it before generating:

### Prompt Enhancement Process
1. **Parse intent**: What type of page? What sections implied?
2. **Map to DESIGN.md sections**: Which sections from the section plan apply?
3. **Add UI specifics**: For each section, specify:
   - Layout pattern (asymmetric split, centered, full-bleed, etc.)
   - Component variants from DESIGN.md
   - Typography tokens (hero size, section heading size, body size)
   - Spacing values from base theme
4. **Add copy direction**: Use the Copy Voice section to set tone
5. **Add accessibility requirements**: Semantic HTML, aria attributes, focus states

### Enhanced Prompt Template
```
Generate a {page type} page with these sections:

1. {Section Name}
   - Layout: {specific layout from DESIGN.md}
   - Typography: {specific sizes/weights}
   - Components: {specific component variants}
   - Copy tone: {from DESIGN.md Copy Voice}

2. {Next Section}
   ...

Design tokens:
- Font: {from DESIGN.md}
- Colors: {base + accent from DESIGN.md}
- Spacing: {from base theme}
- Motion: {from DESIGN.md Motion Style}

Hard rules: {from DESIGN.md Hard Rules}
```

## Step 3: Generate the Page

Generate a complete Next.js + Tailwind page component with polished animations.

### Dependencies to Install
Always include the Tier 1 animation + text measurement stack:
```bash
npm install motion lenis @chenglou/pretext
```
Add Tier 2 for marketing/landing pages with scroll-driven sequences:
```bash
npm install gsap @gsap/react
```
Add for OG image generation (replaces browser screenshot approach):
```bash
npm install @napi-rs/canvas
```

### Architecture Rules

**File structure:**
```
app/
  page.tsx          # or the specific route
  layout.tsx        # if not already present
  robots.ts         # AI-friendly robots.txt (auto-generate)
  sitemap.ts        # sitemap with lastModified (auto-generate)
  llms.txt/
    route.ts        # LLM-readable site index (auto-generate)
components/
  sections/
    {SectionName}.tsx   # one file per major section
    FAQ.tsx             # FAQ section (every page gets one)
  ui/
    {Component}.tsx     # reusable UI components
    StructuredData.tsx  # JSON-LD schema (Article + FAQPage + BreadcrumbList)
  providers/
    SmoothScroll.tsx    # Lenis smooth scroll provider
hooks/
  useTextLayout.ts      # Pretext-based text measurement for CLS-free animations
  useAdaptiveHeadline.ts # Binary search for largest font size that fits target line count
scripts/
  generate-og.ts        # Canvas + Pretext OG image generator
```

**Keep sections as separate components** — one file per section, imported into the page.

### Code Rules (Lessons Learned)

#### Tailwind — No Dynamic Classes
```typescript
// NEVER — fails silently, class not in build output
className={`text-${color}-500`}
className={`bg-${size}`}

// ALWAYS — literal strings, use object lookup if dynamic
const colorMap = {
  blue: 'text-blue-500',
  red: 'text-red-500',
} as const;
className={colorMap[color]}
```

#### Theme Provider Setup
```tsx
// layout.tsx — suppressHydrationWarning is REQUIRED
<html lang="en" suppressHydrationWarning>
  <body>
    <ThemeProvider attribute="class" defaultTheme="system" enableSystem>
      {children}
    </ThemeProvider>
  </body>
</html>
```

#### Dark Mode CSS
```css
/* globals.css — use :is(.dark) selector with @apply */
:is(.dark) body {
  @apply bg-neutral-950 text-neutral-100;
}
```

#### Font Scaling
```tsx
// Use font-size on <html> to scale all rem-based utilities
// Import fonts via next/font/google
import { Inter, DM_Sans } from 'next/font/google';
```

#### Typography
- Line-height: 1.6 for body, 1.05 for hero, 1.3 for section headings
- Letter-spacing: -0.02em for hero, 0.05em for uppercase labels
- Use curly quotes (" ") not straight quotes (" ")
- Metadata pattern: `<strong>Category</strong> · Date`

#### Responsive
- Always mobile-first: `grid-cols-1 sm:grid-cols-2 lg:grid-cols-3`
- Test mental model: single column on phone, expand at breakpoints
- Never hide critical content on mobile — adapt, don't remove

#### Accessibility
- Semantic HTML: `<nav>`, `<main>`, `<article>`, `<section>`, `<footer>`
- `aria-expanded` on accordions/toggles
- `target="_blank" rel="noopener noreferrer"` on external links
- `focus-visible` states on all interactive elements
- Alt text on all images
- Skip-to-content link

#### Animation (from animation-stack.md)

**Smooth scroll setup (every project):**
```tsx
// components/providers/SmoothScroll.tsx
'use client'
import { useEffect } from 'react'
import Lenis from 'lenis'

export function SmoothScroll({ children }: { children: React.ReactNode }) {
  useEffect(() => {
    const lenis = new Lenis({ duration: 1.2, easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)) })
    function raf(time: number) { lenis.raf(time); requestAnimationFrame(raf) }
    requestAnimationFrame(raf)
    return () => lenis.destroy()
  }, [])
  return <>{children}</>
}
```

**Section entrance animations (Motion):**
```tsx
import { motion } from 'motion/react'

// Fade-up entrance — use on every section
<motion.section
  initial={{ opacity: 0, y: 30 }}
  whileInView={{ opacity: 1, y: 0 }}
  viewport={{ once: true, margin: '-100px' }}
  transition={{ duration: 0.6, ease: [0.16, 1, 0.3, 1] }}
>
```

**Staggered children:**
```tsx
const container = { hidden: {}, show: { transition: { staggerChildren: 0.08 } } }
const item = { hidden: { opacity: 0, y: 20 }, show: { opacity: 1, y: 0 } }

<motion.div variants={container} initial="hidden" whileInView="show" viewport={{ once: true }}>
  {items.map(i => <motion.div key={i} variants={item} />)}
</motion.div>
```

**GSAP scroll-triggered (for marketing pages):**
```tsx
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import { useGSAP } from '@gsap/react'

gsap.registerPlugin(ScrollTrigger)

useGSAP(() => {
  gsap.from('.reveal', {
    opacity: 0, y: 60, duration: 1, ease: 'power3.out',
    stagger: 0.1,
    scrollTrigger: { trigger: '.reveal', start: 'top 80%' }
  })
})
```

**Easing — always use exponential out (premium feel):**
- Motion: `ease: [0.16, 1, 0.3, 1]` (expo-out)
- GSAP: `ease: 'power3.out'`
- Never: `linear`, `ease-in`, `bounce`, `elastic`

**Timing:** Interactions 100-150ms, entrances 400-600ms, hero 600-800ms, stagger gap 50-100ms.

#### Hero Animation Pattern (CRITICAL — this is the first impression)

The hero is the most important 3 seconds of the site. The rule: **content first, animation second**. The value prop must be readable within 500ms. Animation reveals the content — it never gates it.

**The pattern: staggered parallel reveal**
- Text elements (headline, subtext, CTA) fade up with tight stagger — the full text sequence completes in ~500ms
- Supporting visual (product screenshot, 3D element, illustration) animates in simultaneously from a different axis (scale, slide-in from right, fade)
- Optional ambient background (gradient, particles, grid dots) is already rendering or fades in even faster

**Reference implementation — HeroSection.tsx:**
```tsx
'use client'
import { motion } from 'motion/react'

// Shared easing — exponential out feels premium
const ease = [0.16, 1, 0.3, 1] as const

// Hero text container — orchestrates staggered children
const heroText = {
  hidden: {},
  show: {
    transition: { staggerChildren: 0.08, delayChildren: 0.1 }
  }
}

// Each text element fades up 20px
const heroItem = {
  hidden: { opacity: 0, y: 20 },
  show: {
    opacity: 1, y: 0,
    transition: { duration: 0.5, ease }
  }
}

// Hero visual enters from a different axis
const heroVisual = {
  hidden: { opacity: 0, scale: 0.96, y: 20 },
  show: {
    opacity: 1, scale: 1, y: 0,
    transition: { duration: 0.8, ease, delay: 0.2 }
  }
}

export function HeroSection() {
  return (
    <section className="relative overflow-hidden">
      {/* Optional: ambient background — renders immediately, no animation delay */}
      <div className="absolute inset-0 pointer-events-none" aria-hidden>
        {/* gradient, grid dots, particles, etc. */}
      </div>

      <div className="relative max-w-[1100px] mx-auto px-6 pt-32 pb-20">
        {/* Asymmetric layout: 45% text, 50% visual */}
        <div className="grid grid-cols-1 lg:grid-cols-[45fr_50fr] gap-16 items-center">

          {/* Text column — staggered fade-up, completes in ~500ms */}
          <motion.div
            variants={heroText}
            initial="hidden"
            animate="show"
          >
            {/* Optional eyebrow / announcement pill */}
            <motion.p variants={heroItem} className="text-sm text-neutral-500 mb-4">
              Announcing v2.0
            </motion.p>

            {/* Headline — the single most important element */}
            <motion.h1
              variants={heroItem}
              className="text-5xl lg:text-7xl font-bold tracking-tight leading-[1.05] text-neutral-900"
            >
              Your headline here
            </motion.h1>

            {/* Subtext */}
            <motion.p
              variants={heroItem}
              className="mt-6 text-lg text-neutral-500 leading-relaxed max-w-md"
            >
              One or two sentences explaining the value proposition clearly.
            </motion.p>

            {/* CTA pair — primary solid + secondary outline/text */}
            <motion.div variants={heroItem} className="mt-8 flex items-center gap-4">
              <a href="#" className="inline-flex items-center px-6 py-3 rounded-full bg-neutral-900 text-white text-sm font-medium whitespace-nowrap hover:bg-neutral-800 transition-colors">
                Get started
              </a>
              <a href="#" className="text-sm text-neutral-600 hover:text-neutral-900 transition-colors">
                View documentation →
              </a>
            </motion.div>
          </motion.div>

          {/* Visual column — enters slightly later, from a different axis */}
          <motion.div
            variants={heroVisual}
            initial="hidden"
            animate="show"
          >
            {/* Product screenshot, 3D element, terminal block, etc. */}
            <div className="rounded-2xl overflow-hidden">
              {/* The hero visual goes here */}
            </div>
          </motion.div>

        </div>
      </div>
    </section>
  )
}
```

**Hero text pre-calculation with Pretext (eliminates CLS):**

Use Pretext to know exact line counts before the animation starts. This eliminates layout shift during hero entrance animations — the container has the correct height from frame 1.

```tsx
// hooks/useTextLayout.ts
'use client'
import { useLayoutEffect, useState, useCallback, type RefObject } from 'react'
import { prepare, layout } from '@chenglou/pretext'

export function useTextLayout(
  text: string,
  font: string,
  lineHeight: number,
  containerRef: RefObject<HTMLElement | null>
) {
  const [result, setResult] = useState<{ height: number; lineCount: number } | null>(null)

  const measure = useCallback(() => {
    if (!containerRef.current) return
    const width = containerRef.current.offsetWidth
    const prepared = prepare(text, font)
    setResult(layout(prepared, width, lineHeight))
  }, [text, font, lineHeight, containerRef])

  useLayoutEffect(() => {
    measure()
    const ro = new ResizeObserver(measure)
    if (containerRef.current) ro.observe(containerRef.current)
    return () => ro.disconnect()
  }, [measure])

  return result
}
```

```tsx
// In HeroSection.tsx — set exact container height, dynamic stagger
const containerRef = useRef<HTMLDivElement>(null)
const headlineLayout = useTextLayout('Your headline', 'bold 72px Inter', 80, containerRef)

// Container won't jump when animation reveals text
<div ref={containerRef} style={{ minHeight: headlineLayout?.height }}>
  <motion.h1 variants={heroItem}>{headlineText}</motion.h1>
</div>

// Tighter stagger when headline wraps to more lines
const stagger = headlineLayout ? Math.min(0.08, 0.4 / headlineLayout.lineCount) : 0.08
```

Always create `hooks/useTextLayout.ts` in every project that uses hero animations.

**Adaptive headline sizing with Pretext (optional — use when headline length varies):**

When the headline text might vary in length (CMS-driven, A/B tested, localized), use `useAdaptiveHeadline` to auto-size. Binary search finds the largest font that fits within a target line count — no DOM thrash, responsive to resize.

```tsx
// hooks/useAdaptiveHeadline.ts — binary search for optimal font size
const headline = useAdaptiveHeadline(
  headlineText,
  'Inter', 'bold',
  2,       // max 2 lines
  36,      // min 36px
  80,      // max 80px  
  containerRef
)

// Apply computed size
<h1 style={{ fontSize: headline?.fontSize, lineHeight: headline?.lineHeight + 'px' }}>
```

See full implementation in `~/.claude/design-library/references/pretext.md` → "Pattern: Adaptive Headline Sizing".

**Hero variant patterns** (adapt based on DESIGN.md):

| Type | Text | Visual | Best for |
|------|------|--------|----------|
| **Asymmetric split** | Left 45%, fade-up stagger | Right 50%, scale-in + fade | Product with screenshot/demo |
| **Centered minimal** | Center, fade-up stagger, max-w-640 | Below text, fade-up with 200ms delay | Developer tools, APIs |
| **Full-bleed visual** | Overlaid on visual, fade-up | Full-width background, already rendered | Bold marketing, visual products |
| **Terminal hero** | Left, fade-up stagger | Right, terminal block slides in from right | CLI tools, dev infrastructure |

**Hero animation rules:**
1. Headline must be readable within 300ms of page load — use short `delayChildren: 0.1` at most
2. Total text reveal completes in ~500ms (stagger 0.08 x ~5 elements)
3. Visual can take up to 800ms but must start animating within 200ms
4. Background/ambient elements load instantly (no animation delay) or use CSS animations that don't block rendering
5. No sequenced gates — text and visual animate in parallel, not one-after-the-other
6. On mobile: stack vertically (text above visual), same animation timing
7. `animate` not `whileInView` — hero animates on mount, not on scroll intersection
8. Never: typing animation on headline, loading spinner before hero, fade-in that takes >1s, hero content hidden behind a scroll trigger

#### Images
- Min `rounded-xl` (12px) on all images
- `rounded-2xl` (16px) for hero/demo images
- Use `next/image` with proper width/height/alt
- Placeholder gradient backgrounds for missing images

### Anti-Slop Guardrails

Before writing any component, verify it does NOT contain:
- Card borders (`border`) or card shadows (`shadow-*`) on content cards
- Gradient text (`bg-gradient-to-* bg-clip-text text-transparent`)
- Colored primary buttons — must be `bg-neutral-900 text-white` or equivalent
- Equal 50/50 column layouts — always asymmetric
- Identical card grids (3 same-size, same-layout cards in a row)
- Generic centered hero with gradient background
- Cyan/neon accents, glassmorphism effects
- Icons next to every heading
- "Trusted by" labels above logo bars
- Bounce/elastic animations

If any of these appear in the generated code, fix them before outputting.

### Agent-Friendly & GEO (Auto-Generated for Every Site)

See full reference: `~/.claude/design-library/references/agent-friendly.md`

Every site gets these files automatically — they make the site discoverable and usable by AI agents:

1. **`app/llms.txt/route.ts`** — LLM-readable site index. Curated list of pages with descriptions + an Instructions section that steers AI agent behavior.

2. **`app/robots.ts`** — Explicitly allows AI crawlers (OAI-SearchBot, ChatGPT-User, Claude-SearchBot, Claude-User, PerplexityBot, ClaudeBot, GPTBot). Blocks scrapers (AhrefsBot, SemrushBot).

3. **`app/sitemap.ts`** — Auto-generated sitemap with `lastModified` dates for crawl freshness.

4. **`components/ui/StructuredData.tsx`** — JSON-LD triple schema stack (Article + FAQPage + BreadcrumbList). 2x more AI citations than Article alone. Include on every page.

5. **`components/sections/FAQ.tsx`** — Every page ends with a 3-5 question FAQ section. Feeds FAQPage schema. Highest AI citation probability of any content type. **Use Pretext for smooth accordion animations** — pre-measure answer height with `prepare()` + `layout()`, then animate with `height` CSS transition. Never use `max-height` hacks. See full pattern in `~/.claude/design-library/references/pretext.md` → "Pattern: Smooth Accordion/FAQ Animations".

6. **Content structure rules:**
   - First paragraph of every section = self-contained, citable summary (the "extraction zone")
   - No dangling pronouns — always use explicit nouns
   - H2 headings phrased as questions when appropriate
   - Statistics include attribution and dates
   - 60-100 words per paragraph

7. **Meta tags** — `dateModified`, standalone `description`, OG image, Twitter card on every page.

8. **OG Image (auto-generated for every site — Canvas + Pretext):**
   Generate a 1200x630 OG image using `@napi-rs/canvas` + `@chenglou/pretext`. No browser dependency, works in CI, deterministic output.

   Create `scripts/generate-og.ts`:
   ```ts
   import { createCanvas, registerFont } from '@napi-rs/canvas'
   import { prepareWithSegments, layoutWithLines } from '@chenglou/pretext'
   import { writeFileSync } from 'fs'

   // Register project fonts (must match DESIGN.md fonts)
   registerFont('./public/fonts/Inter-Bold.ttf', { family: 'Inter', weight: '700' })
   registerFont('./public/fonts/Inter-Regular.ttf', { family: 'Inter', weight: '400' })

   const W = 1200, H = 630, PAD = 60

   function generate(headline: string, subtext: string, metrics: string[]) {
     const canvas = createCanvas(W, H)
     const ctx = canvas.getContext('2d')

     // Background — match DESIGN.md palette
     ctx.fillStyle = '#0D0D0D'
     ctx.fillRect(0, 0, W, H)

     // Headline — Pretext calculates exact line breaks
     const hp = prepareWithSegments(headline, 'bold 48px Inter')
     const { lines: hl } = layoutWithLines(hp, W - PAD * 2, 58)
     ctx.fillStyle = '#FAFAFA'
     ctx.font = 'bold 48px Inter'
     hl.forEach((line, i) => ctx.fillText(line.text, PAD, 140 + i * 58))

     // Subtext
     const sp = prepareWithSegments(subtext, '20px Inter')
     const { lines: sl } = layoutWithLines(sp, W - PAD * 2, 30)
     ctx.fillStyle = '#6E6E6E'
     ctx.font = '20px Inter'
     const subY = 140 + hl.length * 58 + 24
     sl.forEach((line, i) => ctx.fillText(line.text, PAD, subY + i * 30))

     // Metrics row
     ctx.fillStyle = '#ABABAB'
     ctx.font = '700 28px monospace'
     const gap = (W - PAD * 2) / metrics.length
     metrics.forEach((m, i) => ctx.fillText(m, PAD + i * gap, H - 80))

     // Brand mark
     ctx.fillStyle = '#6E6E6E'
     ctx.font = '14px Inter'
     ctx.fillText('Powered by Boundless', W - PAD - 180, H - 30)

     writeFileSync('public/og-image.png', canvas.toBuffer('image/png'))
   }

   // Call with your site's content
   generate('Your Headline Here', 'One line describing the product.', ['Metric 1', 'Metric 2', 'Metric 3'])
   ```

   Run with: `npx tsx scripts/generate-og.ts`

   Then add to metadata: `openGraph.images` + `twitter.images` pointing to `/og-image.png`
   Use `twitter.card: "summary_large_image"` for rich X previews.

   **OG image design rules:**
   - Match the site's color palette (bg, text, accent from DESIGN.md)
   - Include: headline, 1-line subtext, 3-4 key metrics in accent color
   - Use font-mono for metric numbers
   - Optional: ghosted background texture matching the hero (price tickers, grid, etc.)
   - "Powered by Boundless" or equivalent brand mark bottom-right
   - No logos or images — typography does the work
   - Pretext handles line wrapping so headlines never clip or overflow

9. **Static export sites:** For `output: "export"` sites, GEO files go in `public/` as static files (not API routes):
   - `public/llms.txt`, `public/robots.txt`, `public/sitemap.xml`
   - JSON-LD goes in layout.tsx `<head>` via `dangerouslySetInnerHTML`

## Step 4: Validate

Run these checks after generation:

1. **TypeScript**: `npx tsc --noEmit` — must pass with zero errors
2. **Anti-slop scan**: Grep the generated files for banned patterns:
   - `shadow-` on card-like elements
   - `border` on card-like elements (borders on inputs/nav are fine)
   - `bg-gradient` combined with `bg-clip-text`
   - `grid-cols-2` without asymmetry (should be custom grid-template-columns)
   - `animate-bounce`, `animate-pulse` used decoratively
3. **DESIGN.md conformance**: Verify colors, fonts, spacing match the design system
4. **Mobile check**: Verify every section has responsive classes

## Step 5: Output

Present the generated page with:
- File paths created/modified
- A brief description of each section
- Any deviations from DESIGN.md (with justification)
- Reminder that `/site-refine` can be run for autonomous quality improvement
