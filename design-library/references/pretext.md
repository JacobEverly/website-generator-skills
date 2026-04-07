# Pretext — Text Measurement & Layout (No DOM)

Pure JS/TS library that measures and lays out multiline text without touching the DOM. Uses canvas APIs for measurement, then pure arithmetic for layout — no reflow, no `getBoundingClientRect()`.

**Install:** `npm install @chenglou/pretext`

**Source:** [github.com/chenglou/pretext](https://github.com/chenglou/pretext)

---

## When to Use Pretext

| Situation | Why Pretext |
|-----------|-------------|
| OG image generation | Canvas text needs exact line breaks — Pretext measures without a browser |
| Hero animation sizing | Pre-calculate line count per breakpoint → set exact container heights → zero CLS |
| Virtualized lists with text | Know row heights before render → no layout thrash |
| Creative text layouts | Variable-width containers, tapered text, text flowing around shapes |
| Canvas/SVG text rendering | `layoutWithLines()` gives you each line's text + width for precise `fillText()` |
| Responsive text decisions | Check if headline fits on one line at a given width before choosing font size |

---

## Core APIs

### Simple Height Measurement

```ts
import { prepare, layout } from '@chenglou/pretext'

// One-time text analysis (caches font metrics)
const prepared = prepare('Your headline text here', 'bold 72px Inter')

// Pure arithmetic — call repeatedly on resize, no DOM cost
const desktop = layout(prepared, 600, 80)  // { height, lineCount }
const mobile = layout(prepared, 340, 80)   // { height, lineCount }
```

`prepare()` is the expensive call (measures character widths). `layout()` is pure math over cached data — call it for every breakpoint, every resize, every animation frame.

### Line-by-Line Layout

```ts
import { prepareWithSegments, layoutWithLines } from '@chenglou/pretext'

const prepared = prepareWithSegments(text, '18px "Helvetica Neue"')
const { lines } = layoutWithLines(prepared, 320, 26)

for (let i = 0; i < lines.length; i++) {
  ctx.fillText(lines[i].text, 0, i * 26)  // Each line has .text and .width
}
```

### Variable-Width Lines (Creative Layouts)

```ts
import { prepareWithSegments, layoutNextLine } from '@chenglou/pretext'

const prepared = prepareWithSegments(text, '16px Inter')
let state = { offset: 0 }

// Each line can have a different width — text flows around shapes
const widths = [400, 380, 360, 340, 360, 380, 400]  // diamond shape
const lines = []
for (const width of widths) {
  const result = layoutNextLine(prepared, width, 24, state)
  if (!result) break
  lines.push(result)
  state = result.state
}
```

### Rich-Text Inline (Multi-Font)

```ts
import { prepareRichInline, layoutNextRichInlineLineRange, materializeRichInlineLineRange } from '@chenglou/pretext/rich-inline'

const prepared = prepareRichInline([
  { text: 'Bold headline ', font: 'bold 48px Inter' },
  { text: 'with lighter subtitle', font: '300 32px Inter' },
])
```

### Utility Functions

```ts
import { measureNaturalWidth, measureLineStats, clearCache, setLocale } from '@chenglou/pretext'

// Find the widest forced line (no wrapping)
const maxWidth = measureNaturalWidth(prepared)

// Get line count + max width without building strings
const stats = measureLineStats(prepared, containerWidth, lineHeight)

// Release font caches when done
clearCache()
```

---

## Options

```ts
prepare(text, font, {
  whiteSpace: 'normal' | 'pre-wrap',    // 'pre-wrap' preserves whitespace (textarea-like)
  wordBreak: 'normal' | 'keep-all',     // 'keep-all' for CJK text
})
```

Font string uses canvas shorthand: `'bold 16px Inter'`, `'italic 14px "DM Sans"'`, `'700 48px "Plus Jakarta Sans"'`.

---

## Pattern: OG Image Generation (Canvas + Pretext)

Replace browser-based screenshot pipelines with pure Canvas rendering. Works headlessly, in CI, anywhere Node runs.

```ts
// scripts/generate-og.ts
import { createCanvas, registerFont } from '@napi-rs/canvas'
import { prepareWithSegments, layoutWithLines } from '@chenglou/pretext'

// Register project fonts (must match DESIGN.md)
registerFont('./public/fonts/Inter-Bold.ttf', { family: 'Inter', weight: '700' })
registerFont('./public/fonts/Inter-Regular.ttf', { family: 'Inter', weight: '400' })

const WIDTH = 1200
const HEIGHT = 630
const PADDING = 60

function generateOgImage(headline: string, subtext: string, metrics: string[]) {
  const canvas = createCanvas(WIDTH, HEIGHT)
  const ctx = canvas.getContext('2d')

  // Background (match DESIGN.md palette)
  ctx.fillStyle = '#0D0D0D'
  ctx.fillRect(0, 0, WIDTH, HEIGHT)

  // Headline — Pretext measures exact line breaks
  const headlinePrepared = prepareWithSegments(headline, 'bold 48px Inter')
  const { lines: headlineLines } = layoutWithLines(headlinePrepared, WIDTH - PADDING * 2, 58)

  ctx.fillStyle = '#FAFAFA'
  ctx.font = 'bold 48px Inter'
  for (let i = 0; i < headlineLines.length; i++) {
    ctx.fillText(headlineLines[i].text, PADDING, 120 + i * 58)
  }

  const headlineBottom = 120 + headlineLines.length * 58

  // Subtext
  const subtextPrepared = prepareWithSegments(subtext, '20px Inter')
  const { lines: subtextLines } = layoutWithLines(subtextPrepared, WIDTH - PADDING * 2, 30)

  ctx.fillStyle = '#6E6E6E'
  ctx.font = '20px Inter'
  for (let i = 0; i < subtextLines.length; i++) {
    ctx.fillText(subtextLines[i].text, PADDING, headlineBottom + 24 + i * 30)
  }

  // Metrics row (accent color, mono font)
  ctx.fillStyle = '#ABABAB'
  ctx.font = '700 28px monospace'
  const metricGap = (WIDTH - PADDING * 2) / metrics.length
  metrics.forEach((m, i) => {
    ctx.fillText(m, PADDING + i * metricGap, HEIGHT - 80)
  })

  // Brand mark
  ctx.fillStyle = '#6E6E6E'
  ctx.font = '14px Inter'
  ctx.fillText('Powered by Boundless', WIDTH - PADDING - 180, HEIGHT - 30)

  return canvas.toBuffer('image/png')
}
```

**Deps:** `npm install @chenglou/pretext @napi-rs/canvas`

**Advantages over Brave headless screenshot:**
- No browser dependency — works in CI, Docker, serverless
- Deterministic output — same input always produces same image
- ~50ms vs ~2s per image
- Font control — register exact TTF/OTF files, no system font surprises

---

## Pattern: Hero Animation Pre-Calculation

Know exact line counts before first paint → set container heights → eliminate CLS during animation.

```tsx
// hooks/useTextLayout.ts
'use client'
import { useLayoutEffect, useState, useCallback } from 'react'
import { prepare, layout } from '@chenglou/pretext'

interface TextLayout {
  height: number
  lineCount: number
}

export function useTextLayout(
  text: string,
  font: string,
  lineHeight: number,
  containerRef: React.RefObject<HTMLElement | null>
): TextLayout | null {
  const [result, setResult] = useState<TextLayout | null>(null)

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
// Usage in HeroSection.tsx
const containerRef = useRef<HTMLDivElement>(null)
const headlineLayout = useTextLayout(
  'Your headline text here',
  'bold 72px Inter',
  80,
  containerRef
)

// Set exact height before animation starts
<div ref={containerRef} style={{ minHeight: headlineLayout?.height }}>
  <motion.h1 initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }}>
    {headlineText}
  </motion.h1>
</div>

// Dynamic stagger timing based on actual line count
const stagger = headlineLayout ? 0.4 / headlineLayout.lineCount : 0.08
```

---

## Pattern: Creative Text Shapes (Variable-Width Layout)

Use `layoutNextLine()` to flow text into non-rectangular shapes — tapered paragraphs, diamond shapes, text wrapping around images. Produces distinctive layouts that can't be done in CSS.

```ts
// Tapered paragraph — each line slightly narrower
const maxWidth = 600
const taper = 15 // pixels narrower per line
const prepared = prepareWithSegments(text, '16px Inter')
let state = { offset: 0 }
const lines = []

for (let i = 0; i < 20; i++) {
  const width = Math.max(300, maxWidth - i * taper)
  const result = layoutNextLine(prepared, width, 24, state)
  if (!result) break
  lines.push({ ...result, x: (maxWidth - width) / 2 }) // center each line
  state = result.state
}
```

This creates a centered, tapered paragraph that narrows toward the bottom — visually distinctive and impossible with standard CSS text layout.

---

## Pattern: Smooth Accordion/FAQ Animations (No Layout Reflow)

From Pretext's accordion demo. Every site has a FAQ section — this makes expand/collapse buttery smooth by pre-measuring content height with Pretext instead of relying on DOM measurement or `auto` height transitions.

```tsx
// components/sections/FAQ.tsx
'use client'
import { useRef, useState, useEffect, useCallback } from 'react'
import { prepare, layout } from '@chenglou/pretext'

interface FAQItem { question: string; answer: string }

function usePanelHeight(text: string, font: string, lineHeight: number, containerWidth: number) {
  const [height, setHeight] = useState(0)

  useEffect(() => {
    if (!containerWidth) return
    const prepared = prepare(text, font)
    const { height: textHeight } = layout(prepared, containerWidth, lineHeight)
    setHeight(Math.ceil(textHeight) + 32) // + padding
  }, [text, font, lineHeight, containerWidth])

  return height
}

export function FAQ({ items }: { items: FAQItem[] }) {
  const [openIndex, setOpenIndex] = useState<number | null>(null)
  const containerRef = useRef<HTMLDListElement>(null)
  const [containerWidth, setContainerWidth] = useState(0)

  useEffect(() => {
    if (!containerRef.current) return
    const ro = new ResizeObserver(([entry]) => setContainerWidth(entry.contentRect.width - 48)) // minus padding
    ro.observe(containerRef.current)
    return () => ro.disconnect()
  }, [])

  return (
    <dl ref={containerRef}>
      {items.map((item, i) => (
        <FAQEntry
          key={i}
          item={item}
          isOpen={openIndex === i}
          onToggle={() => setOpenIndex(openIndex === i ? null : i)}
          containerWidth={containerWidth}
        />
      ))}
    </dl>
  )
}

function FAQEntry({ item, isOpen, onToggle, containerWidth }: {
  item: FAQItem; isOpen: boolean; onToggle: () => void; containerWidth: number
}) {
  const targetHeight = usePanelHeight(item.answer, '16px Inter', 26, containerWidth)

  return (
    <div>
      <dt>
        <button onClick={onToggle} aria-expanded={isOpen}>
          {item.question}
        </button>
      </dt>
      <dd
        style={{
          height: isOpen ? `${targetHeight}px` : '0px',
          overflow: 'hidden',
          transition: 'height 400ms cubic-bezier(0.16, 1, 0.3, 1)',
        }}
      >
        <p style={{ padding: '16px 24px' }}>{item.answer}</p>
      </dd>
    </div>
  )
}
```

**Why this matters:** CSS `height: auto` can't be transitioned. The common workaround is `max-height` with an arbitrarily large value, which makes the animation timing wrong (it animates through invisible space). Pretext gives the exact target height, so the animation duration is honest.

---

## Pattern: Adaptive Headline Sizing (Binary Search)

From Pretext's editorial engine demo. Find the largest font size that fits a headline on a target number of lines — without any DOM measurement. Produces perfectly fitted hero typography that adapts to content length.

```tsx
// hooks/useAdaptiveHeadline.ts
'use client'
import { useLayoutEffect, useState, useCallback, type RefObject } from 'react'
import { prepare, layout } from '@chenglou/pretext'

interface AdaptiveResult {
  fontSize: number
  lineHeight: number
  height: number
  lineCount: number
}

export function useAdaptiveHeadline(
  text: string,
  fontFamily: string,
  fontWeight: string,
  targetLines: number,        // max lines before we shrink
  minSize: number,            // smallest acceptable px
  maxSize: number,            // largest px to try
  containerRef: RefObject<HTMLElement | null>
): AdaptiveResult | null {
  const [result, setResult] = useState<AdaptiveResult | null>(null)

  const fit = useCallback(() => {
    if (!containerRef.current) return
    const width = containerRef.current.offsetWidth

    let lo = minSize, hi = maxSize
    // Binary search for largest font that fits within targetLines
    while (hi - lo > 1) {
      const mid = Math.floor((lo + hi) / 2)
      const lh = Math.ceil(mid * 1.1) // line-height 1.1x
      const prepared = prepare(text, `${fontWeight} ${mid}px ${fontFamily}`)
      const { lineCount } = layout(prepared, width, lh)
      if (lineCount <= targetLines) {
        lo = mid
      } else {
        hi = mid
      }
    }

    const fontSize = lo
    const lineHeight = Math.ceil(fontSize * 1.1)
    const prepared = prepare(text, `${fontWeight} ${fontSize}px ${fontFamily}`)
    const measured = layout(prepared, width, lineHeight)
    setResult({ fontSize, lineHeight, height: measured.height, lineCount: measured.lineCount })
  }, [text, fontFamily, fontWeight, targetLines, minSize, maxSize, containerRef])

  useLayoutEffect(() => {
    fit()
    const ro = new ResizeObserver(fit)
    if (containerRef.current) ro.observe(containerRef.current)
    return () => ro.disconnect()
  }, [fit])

  return result
}
```

```tsx
// Usage in HeroSection.tsx — headline auto-sizes to fill its container
const containerRef = useRef<HTMLDivElement>(null)
const headline = useAdaptiveHeadline(
  'Verifiable Computation for Every Application',
  'Inter', 'bold',
  2,      // max 2 lines
  36,     // min 36px
  80,     // max 80px
  containerRef
)

<div ref={containerRef}>
  {headline && (
    <motion.h1
      style={{
        fontSize: `${headline.fontSize}px`,
        lineHeight: `${headline.lineHeight}px`,
        minHeight: headline.height,
      }}
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
    >
      {headlineText}
    </motion.h1>
  )}
</div>
```

**Why this matters:** Long headlines that wrap to 3-4 lines at a fixed size look broken. Short headlines at the same size waste space. Binary search converges in ~6 iterations of pure arithmetic — no DOM thrash, responsive to resize.

---

## Pattern: Text Wrapping Around Shapes

From Pretext's dynamic-layout and wrap-geometry modules. Flow text around arbitrary polygons, images, or geometric shapes. Uses band-based obstacle detection — for each line's vertical band, compute which horizontal intervals are blocked, then lay text into the remaining space.

```ts
// Simplified: text flowing around a circle
import { prepareWithSegments, layoutNextLine } from '@chenglou/pretext'

function getAvailableWidth(lineY: number, lineHeight: number, circle: { cx: number; cy: number; r: number }, maxWidth: number) {
  const bandTop = lineY
  const bandBottom = lineY + lineHeight
  // Check if this band intersects the circle
  const closestY = Math.max(bandTop, Math.min(circle.cy, bandBottom))
  const dy = closestY - circle.cy
  if (Math.abs(dy) >= circle.r) return { offset: 0, width: maxWidth } // no intersection

  const dx = Math.sqrt(circle.r * circle.r - dy * dy)
  const circleLeft = circle.cx - dx
  const circleRight = circle.cx + dx

  // Text goes to the left of the circle, or right, whichever is wider
  const leftSlot = circleLeft
  const rightSlot = maxWidth - circleRight
  return leftSlot > rightSlot
    ? { offset: 0, width: leftSlot }
    : { offset: circleRight, width: rightSlot }
}

const prepared = prepareWithSegments(articleText, '16px Inter')
let state = { offset: 0 }
const lines = []
const lineHeight = 24

for (let y = 0; y < 800; y += lineHeight) {
  const { offset, width } = getAvailableWidth(y, lineHeight, circle, 600)
  const result = layoutNextLine(prepared, width, lineHeight, state)
  if (!result) break
  lines.push({ ...result, x: offset, y })
  state = result.state
}
```

Combine with the animation stack for scroll-triggered shape reveals — text reflows around shapes that animate into view.

---

## Pattern: Virtualized Text Content

From Pretext's markdown-chat demo. For data-heavy pages with potentially hundreds of text rows (dashboards, feeds, logs), pre-measure all row heights to enable proper virtualization.

```tsx
// hooks/useVirtualizedText.ts
import { prepare, layout } from '@chenglou/pretext'

interface VirtualRow {
  text: string
  height: number
  offsetY: number
}

export function measureRows(
  items: string[],
  font: string,
  lineHeight: number,
  containerWidth: number,
  padding: number = 16
): VirtualRow[] {
  const rows: VirtualRow[] = []
  let offsetY = 0

  for (const text of items) {
    const prepared = prepare(text, font)
    const { height } = layout(prepared, containerWidth - padding * 2, lineHeight)
    const totalHeight = Math.ceil(height) + padding * 2
    rows.push({ text, height: totalHeight, offsetY })
    offsetY += totalHeight
  }

  return rows
}

// Then render only visible rows based on scroll position
function getVisibleRange(rows: VirtualRow[], scrollTop: number, viewportHeight: number) {
  let start = 0, end = rows.length
  for (let i = 0; i < rows.length; i++) {
    if (rows[i].offsetY + rows[i].height > scrollTop) { start = i; break }
  }
  for (let i = start; i < rows.length; i++) {
    if (rows[i].offsetY > scrollTop + viewportHeight) { end = i; break }
  }
  return { start, end }
}
```

**Why this matters:** Standard virtualization libraries (react-virtual, tanstack-virtual) either guess row heights or measure after rendering (causing flicker). Pretext gives exact heights before any DOM exists.

---

## Caveats

- Use named fonts (`'Inter'`, `'DM Sans'`), not `system-ui` — macOS canvas returns inconsistent widths for `system-ui`
- Measurements reflect canvas character widths, not glyph positions — sufficient for layout, not for cursor placement in mixed-direction text
- Call `clearCache()` if you measure with many different fonts to avoid memory growth
- In Node.js (OG images), fonts must be registered with `@napi-rs/canvas` before Pretext can measure them
