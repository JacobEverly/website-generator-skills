# Animation Stack Reference

The standard animation libraries for all website projects. Organized by tier — always install Tier 1, add others as needed.

## Tier 1 — Every Project

### Motion (formerly Framer Motion)
The baseline React animation layer. Page transitions, entrance animations, gestures, scroll-linked transforms, layout animations.

```bash
npm install motion
```

```tsx
import { motion, useScroll, useTransform, AnimatePresence } from 'motion/react'
```

**Key APIs:**
- `motion.div` — animated wrapper for any element
- `AnimatePresence` — entrance/exit animations
- `useScroll` + `useTransform` — scroll-linked values
- `layout` prop — automatic layout animations
- `whileHover`, `whileTap` — gesture animations
- `variants` — orchestrated staggered children

**Common patterns:**
```tsx
// Fade-in on mount
<motion.div initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.5, ease: [0.16, 1, 0.3, 1] }}>

// Staggered children
const container = { hidden: {}, show: { transition: { staggerChildren: 0.08 } } }
const item = { hidden: { opacity: 0, y: 20 }, show: { opacity: 1, y: 0 } }

// Scroll-linked opacity
const { scrollYProgress } = useScroll()
const opacity = useTransform(scrollYProgress, [0, 0.5], [1, 0])
```

**30.7k GitHub stars / 3.6M weekly downloads**

---

### Lenis (Smooth Scroll)
Lightweight smooth scroll with physics-based momentum. Syncs with GSAP ScrollTrigger.

```bash
npm install lenis
```

```tsx
import Lenis from 'lenis'

// Initialize in layout
useEffect(() => {
  const lenis = new Lenis({ duration: 1.2, easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)) })
  function raf(time: number) { lenis.raf(time); requestAnimationFrame(raf) }
  requestAnimationFrame(raf)
  return () => lenis.destroy()
}, [])
```

**From Darkroom Engineering (premium web studio)**

---

## Tier 2 — Marketing / Landing Pages

### GSAP + ScrollTrigger
Professional-grade animation engine. Unmatched for scroll-driven sequences, timeline orchestration, text reveals, pinned sections.

```bash
npm install gsap
```

```tsx
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import { useGSAP } from '@gsap/react'

gsap.registerPlugin(ScrollTrigger)
```

**Key APIs:**
- `gsap.to()` / `gsap.from()` / `gsap.fromTo()` — tween animations
- `gsap.timeline()` — sequenced animation chains
- `ScrollTrigger` — scroll-linked triggers (free)
- `SplitText` — character/word-level text animations (Club GSAP, paid)
- `DrawSVG` — SVG path drawing (Club GSAP, paid)

**Common patterns:**
```tsx
// Scroll-triggered fade in
useGSAP(() => {
  gsap.from('.section', {
    opacity: 0, y: 60, duration: 1,
    ease: 'power3.out',
    scrollTrigger: { trigger: '.section', start: 'top 80%' }
  })
})

// Staggered text reveal
gsap.from('.word', {
  opacity: 0, y: 40, duration: 0.8,
  ease: 'power3.out', stagger: 0.05,
  scrollTrigger: { trigger: '.heading', start: 'top 75%' }
})

// Pinned section
ScrollTrigger.create({
  trigger: '.panel', pin: true, start: 'top top',
  end: '+=500', scrub: 1
})
```

**23.6k GitHub stars / 1.47M weekly downloads**

**Licensing:** Core + ScrollTrigger = free. SplitText, DrawSVG, ScrollSmoother = Club GSAP (paid). Use Lenis instead of ScrollSmoother.

---

## Tier 3 — Specific Features

### React Three Fiber + Drei (3D/WebGL)
React renderer for Three.js. Declarative 3D scenes as React components.

```bash
npm install three @react-three/fiber @react-three/drei
npm install @react-three/postprocessing  # optional: shader effects
```

```tsx
import { Canvas } from '@react-three/fiber'
import { OrbitControls, Environment, Float } from '@react-three/drei'
```

**Use when:** 3D hero sections, particle effects, interactive 3D objects, WebGL backgrounds, shader-based transitions.

**~30k GitHub stars / ~700k weekly downloads**

---

### Rive (Interactive State Machines)
Designer-built animations that respond to state (hover, loading, error, success). Replaces Lottie for new interactive work.

```bash
npm install @rive-app/react-canvas
```

```tsx
import { useRive } from '@rive-app/react-canvas'
const { RiveComponent } = useRive({ src: '/animation.riv', stateMachines: 'State Machine 1', autoplay: true })
```

**Use when:** Loading states with multiple phases, interactive icons, hover-responsive illustrations, mascot animations.

**Workflow:** Designer creates `.riv` file in Rive editor → export → load in React runtime.

---

### Auto-Animate (Zero-Config Layout Transitions)
One-line animation for list items appearing/disappearing, accordion open/close, conditional content.

```bash
npm install @formkit/auto-animate
```

```tsx
import { useAutoAnimate } from '@formkit/auto-animate/react'
const [parent] = useAutoAnimate()
return <ul ref={parent}>{items.map(item => <li key={item.id}>{item.name}</li>)}</ul>
```

**13k GitHub stars. Use when you need "things just animate" without writing animation code.**

---

## Tier 4 — Situational

### dotLottie (After Effects Animations)
```bash
npm install @lottiefiles/dotlottie-react
```
Use when sourcing from LottieFiles library or when designers export from After Effects.

### Anime.js v4 (Lightweight SVG/CSS Timelines)
```bash
npm install animejs
```
Lightweight alternative to GSAP for targeted SVG path/CSS animations. 65.7k GitHub stars.

---

## Use Case Quick Reference

| Need | Use |
|------|-----|
| Page transitions / route animations | Motion `AnimatePresence` |
| Entrance/reveal animations | Motion or GSAP |
| Scroll-triggered sequences | GSAP ScrollTrigger |
| Smooth scrolling | Lenis |
| Hover/tap micro-interactions | Motion `whileHover`/`whileTap` |
| List/layout transitions | Auto-Animate or Motion `layout` |
| 3D effects, WebGL | React Three Fiber |
| Interactive stateful animations | Rive |
| SVG path drawing | GSAP DrawSVG or Anime.js |
| Text character reveals | GSAP SplitText or Motion stagger |
| Icon/illustration animations | Rive or dotLottie |

## Hero Animation Pattern

The hero is the first 3 seconds. Rule: **content first, animation second**. Value prop readable within 500ms.

**The pattern: staggered parallel reveal**
- Text (headline → subtext → CTA) fades up with 80ms stagger, completing in ~500ms
- Visual (screenshot, 3D, illustration) animates simultaneously from different axis (scale + fade)
- Ambient background (gradient, particles) renders instantly, no animation delay

```tsx
// Hero text — staggered fade-up, ~500ms total
const heroText = {
  hidden: {},
  show: { transition: { staggerChildren: 0.08, delayChildren: 0.1 } }
}
const heroItem = {
  hidden: { opacity: 0, y: 20 },
  show: { opacity: 1, y: 0, transition: { duration: 0.5, ease: [0.16, 1, 0.3, 1] } }
}

// Hero visual — enters from different axis, slightly later
const heroVisual = {
  hidden: { opacity: 0, scale: 0.96, y: 20 },
  show: { opacity: 1, scale: 1, y: 0, transition: { duration: 0.8, ease: [0.16, 1, 0.3, 1], delay: 0.2 } }
}

// Use `animate` not `whileInView` — hero fires on mount
<motion.div variants={heroText} initial="hidden" animate="show">
  <motion.h1 variants={heroItem}>Headline</motion.h1>
  <motion.p variants={heroItem}>Subtext</motion.p>
  <motion.div variants={heroItem}>{/* CTAs */}</motion.div>
</motion.div>
<motion.div variants={heroVisual} initial="hidden" animate="show">
  {/* Product screenshot / visual */}
</motion.div>
```

**Hero variants by page type:**

| Type | Text | Visual | Best for |
|------|------|--------|----------|
| Asymmetric split | Left 45%, fade-up | Right 50%, scale-in | Product with screenshot |
| Centered minimal | Center, fade-up, max-w-640 | Below, fade-up 200ms delay | Dev tools, APIs |
| Full-bleed visual | Overlaid, fade-up | Full-width bg, instant | Bold marketing |
| Terminal hero | Left, fade-up | Right, terminal slides in | CLI tools |

**Rules:**
1. Headline readable within 300ms — `delayChildren: 0.1` max
2. Total text reveal: ~500ms
3. Visual can take 800ms but starts within 200ms
4. Text and visual animate in parallel, never sequenced
5. Use `animate` (on mount), never `whileInView` for hero
6. Never: typing animation, loading gates, fade-in >1s
7. Use `useTextLayout` hook (Pretext) to pre-calculate headline height — set `minHeight` on the container so the hero never shifts during entrance animation

**Pre-calculating hero dimensions with Pretext:**

The `useTextLayout` hook (see `~/.claude/design-library/references/pretext.md`) eliminates CLS by measuring text before the animation starts. The container has the correct height from the first frame, so the fade-up animation reveals content into a correctly-sized space.

```tsx
const containerRef = useRef<HTMLDivElement>(null)
const headlineLayout = useTextLayout('Your headline', 'bold 72px Inter', 80, containerRef)

// Container height is locked before animation starts
<div ref={containerRef} style={{ minHeight: headlineLayout?.height }}>
  <motion.h1 variants={heroItem}>{headlineText}</motion.h1>
</div>

// Adaptive stagger: tighter when headline wraps to more lines
const stagger = headlineLayout ? Math.min(0.08, 0.4 / headlineLayout.lineCount) : 0.08
```

---

## Easing Reference

```tsx
// Motion — cubic bezier (recommended)
transition={{ ease: [0.16, 1, 0.3, 1] }}     // ease-out-expo (premium feel)
transition={{ ease: [0.33, 1, 0.68, 1] }}     // ease-out-cubic (smooth)
transition={{ ease: [0.22, 1, 0.36, 1] }}     // ease-out-quint (snappy)

// GSAP named easings
ease: 'power3.out'      // smooth deceleration (most used)
ease: 'power4.out'      // stronger deceleration
ease: 'expo.out'        // dramatic deceleration
ease: 'back.out(1.7)'   // slight overshoot (use sparingly)

// Lenis scroll easing
easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t))  // expo decay
```

**Never use:** `ease-in`, `linear` for UI (feels sluggish), `bounce`, `elastic` (dated/tacky).

## Timing Guidelines

| Context | Duration |
|---------|----------|
| Hover/focus states | 100-150ms |
| Button press | 100ms |
| Dropdown/popover | 150-200ms |
| Page element entrance | 400-600ms |
| Hero entrance | 600-800ms |
| Stagger between items | 50-100ms |
| Scroll-triggered reveals | 600-800ms |
| Smooth scroll duration | 1000-1200ms |
| Background ambient loops | 2000-4000ms |

**Rule:** Interactions fast (100-200ms), entrances medium (400-800ms), ambient slow (2000ms+).
