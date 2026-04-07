---
name: vercel-homepage
source: https://vercel.com
tags: [developer-tool, dark, minimal, performance, infrastructure, saas, ai-cloud, geist, monochrome]
captured: 2026-03-19
---

## Visual Analysis

### Color

- **Page background (light):** `rgb(250, 250, 250)` / `#fafafa` -- not pure white, a warm off-white that reduces eye strain
- **Page background (dark):** `rgb(0, 0, 0)` / `#000000` -- true black, which is rare and bold; most dark themes use near-black
- **Primary text:** `rgb(23, 23, 23)` / `#171717` -- near-black, softer than pure `#000`
- **Secondary text:** `rgb(77, 77, 77)` / `#4d4d4d` -- muted gray for descriptions, labels, and subheadings
- **Tertiary text:** `rgb(102, 102, 102)` / `#666666` -- footer links and low-emphasis content
- **Dark mode text:** `rgb(237, 237, 237)` / `#ededed` -- off-white, avoids harsh pure white on black
- **Link blue:** `rgb(0, 114, 245)` / `#0072f5` -- vibrant, accessible blue used sparingly for text links only
- **Card backgrounds:** `rgb(255, 255, 255)` / `#ffffff` -- pure white cards on the off-white page create subtle depth
- **Borders:** `rgb(235, 235, 235)` / `#ebebeb` -- extremely subtle, barely-there 1px solid borders between sections
- **Card shadows:** `rgba(0, 0, 0, 0.04) 0px 2px 2px 0px` -- nearly invisible shadow, just enough to lift cards
- **Hero gradient (dark mode):** Spectacular multi-color gradient behind the Vercel triangle -- warm reds, oranges, pinks, greens, and teals bleeding into pure black. Resembles a prismatic light refraction
- **Hero gradient (light mode):** Same prismatic gradient but softer, with watercolor-like transparency against the light background
- **Template card tints:** Each framework card has a subtle pastel tint in the header region (light blue for React, light red/orange for Svelte, light green for Nuxt, warm peach for Next.js) -- brand-appropriate but extremely muted
- **Button primary:** `rgb(23, 23, 23)` / `#171717` with white text -- black, not a brand color
- **Button secondary:** `rgb(255, 255, 255)` / `#ffffff` with dark text -- white/outlined
- **Grid lines:** Extremely faint grid pattern in globe/infrastructure section, hairline strokes
- **Design system tokens:** `--ds-gray-100` through `--ds-gray-1000`, `--ds-gray-alpha-*`, `--ds-blue-*`, `--ds-green-*`, `--ds-amber-*`, `--ds-red-*`, `--ds-teal-*`, `--ds-purple-*`, `--ds-pink-*`, `--ds-background-100/200`

### Typography

- **Font family (sans):** `Geist, Arial, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol"` -- Vercel's custom typeface, designed for developer tools. Clean, geometric, highly legible
- **Font family (mono):** `"Geist Mono", ui-monospace, SFMono-Regular, "Roboto Mono", Menlo, Monaco, "Liberation Mono"` -- used for footer category headings (uppercase), code blocks, and technical labels
- **H1 hero (48px):** weight 600, line-height 48px (1:1), letter-spacing -2.4px (-5%). Extremely tight tracking gives a premium, compressed feel
- **H2 large (40px):** weight 600, line-height 48px, letter-spacing -2.4px (-6%). Used for "Deploy your first app in seconds."
- **H2 inline (24px):** weight 600, line-height 32px, letter-spacing -0.96px (-4%). Used for "Scale your... without compromising..."
- **H3 section title (32px):** weight 600, line-height 40px, letter-spacing -1.28px (-4%). Used for "Your product, delivered."
- **H3 card headings (24px):** weight 600, line-height 32px, letter-spacing -0.96px (-4%)
- **H3 massive CTA (48px):** weight 600, line-height 56px, letter-spacing -2.88px (-6%). Used for "Start Deploying" link
- **Section eyebrow labels (14px):** weight 500, line-height 20px, letter-spacing -0.28px, color `#4d4d4d`. Small category labels above real headings (e.g., "Fluid Compute", "AI Gateway")
- **Body text (16px):** weight 400, base font size
- **Button text (14-16px):** weight 500 (medium)
- **Footer category headings (12px):** weight 500, Geist Mono, uppercase, line-height 12px -- creates strong technical identity
- **Nav links (14px):** weight 500
- **Social proof stats:** Large display text with company logos inline in their brand fonts -- "runway build times went from 7m to 40s" where company names are bold
- **Type scale classes:** `text-heading-72/64/56/48/40/32/24/20/16/14`, `text-copy-24/20/18/16/14/13`, `text-label-20/18/16/14/13/12`, `text-button-16/14/12`
- **Key signature:** Aggressive negative letter-spacing (-4% to -6%) on every heading level. This is Vercel's most distinctive typographic choice.

### Layout

- **Max content width:** ~1200px centered
- **Header:** 64px height, transparent background, no backdrop blur, no border. 24px horizontal padding. Completely clean
- **Hero section:** ~720px tall, centered text with prismatic triangle illustration below. Two CTAs side by side
- **Section dividers:** 1px solid `#ebebeb` borders between major sections -- not background color changes
- **Grid system:** 3-column for product cards (328px each), 2-column for template cards, asymmetric 1/3 + 2/3 for feature sections
- **Social proof:** Full-width flowing prose paragraph with inline company logos -- not a logo bar
- **Tab navigation:** Horizontal pill-style tabs (AI Apps, Web Apps, Ecommerce, Marketing, Platforms) with selected state
- **Product grid:** 3-column bento-style grid with consistent sizing, subtle borders, and arrow CTAs
- **Framework section:** Node-graph visualization -- framework icons left, infrastructure icons right, connected by flowing colored lines with Vercel triangle at center
- **Globe section:** Interactive 3D wireframe globe showing edge network with pulsing nodes
- **Compute section:** Timeline visualization with colored horizontal bars showing Active vs Idle periods
- **AI Gateway section:** Code block left, live model rankings table right -- practical, data-driven
- **Deploy section:** Text + feature bullets left, 2x3 template card grid right
- **CTA section:** Massive "Start Deploying" text-as-link with arrow, plus secondary CTAs
- **Footer:** 6-column first row (Get Started, Build, Scale, Secure, Resources, Learn), 5-column second row (Frameworks, SDKs, Use Cases, Company, Community). Status indicator bottom-left, theme switcher bottom-right
- **Responsive breakpoints:** `@media (max-width: 1150px)` hides desktop nav; `(min-width: 1151px)` hides mobile toggle. Container queries (`@container`) for component-level responsiveness

### Components

- **Navigation bar:** 64px, transparent. Vercel logotype left, 5 nav items center-left (Products, Resources, Solutions with dropdowns; Enterprise, Pricing). Right side: "Ask AI" button, "Log In" link, "Sign Up" button
- **"Ask AI" button:** White bg, 6px border-radius, 32px height -- signals Vercel's AI-first positioning
- **Primary CTAs (hero):** Pill shape (`border-radius: 100px`), 48px height, 16px font, 14px horizontal padding. Black bg + white text + icon
- **Secondary CTAs:** Pill shape, 40px height, 14px font, 10px padding
- **Nav buttons:** Squarish (6px radius), 32px height, 14px font
- **Announcement banner:** Full-width above nav, light bg, 64px min-height. "Ship JS is coming to 5 cities" + "Save the date" pill
- **Product cards:** White bg, 1px `#ebebeb` border, `rgba(0,0,0,0.04)` shadow, ~328x171px. Title + description + circular arrow button CTA. Some include inline product badges (pills: "Fluid", "AI SDK", "AI GATEWAY")
- **Arrow link buttons:** 40px circular with right-arrow icon, used as card CTAs
- **Code block:** Geist Mono, syntax highlighted TypeScript, protected by MutationObserver against DOM manipulation
- **Model rankings table:** Framework name, colored dot indicator, percentage share -- live data ("Claude Sonnet 4.6 - 32.2%")
- **Template cards:** Pastel-tinted upper region with framework logo (12px top border-radius), white lower region with name
- **Browser chrome mockups:** Mini illustrations with traffic light dots and URL bars
- **Chat interface mockup:** Input field with blue send button + "Thinking..." status
- **"NEW" badges:** Small bordered pills on footer items for new products
- **Footer theme switcher:** System/light/dark mode icons, bottom-right
- **Status indicator:** Geist Mono, "NO STATUS AVAILABLE.", bottom-left

### Motion

- **Hero prismatic gradient:** Subtle, slow-shifting color spectrum animation -- light refracting through the triangle shape
- **Globe visualization:** Interactive 3D globe with pulsing nodes indicating real-time edge network activity. WebGL/canvas rendered
- **Scroll-triggered reveals:** Sections fade/slide in progressively as user scrolls
- **Tab transitions:** Smooth content crossfades when switching use case tabs
- **Compute timeline:** Animated bars showing active/idle billing concept
- **Framework diagram lines:** Colored connecting lines may animate on scroll
- **Hover states:** Subtle background color shifts and opacity transitions on buttons and links
- **Overall philosophy:** Motion is restrained and purposeful. Nothing bounces, jiggles, or demands attention. Animations explain concepts (globe = global network, timeline = billing) rather than decorate. Functional over performative.

## What Makes It Work

- **The discipline of near-monochrome.** The entire page operates on black, white, and two grays (`#4d4d4d`, `#666666`). The only color comes from the hero gradient, link blue, and delicate pastel tints on template cards. This restraint makes every splash of color feel intentional and premium. Most SaaS sites use 3-5 brand colors; Vercel uses essentially zero.

- **Tight letter-spacing as brand identity.** The -4% to -6% tracking on all headings is the single most recognizable design choice. Combined with Geist's geometric letterforms and semibold weight, it creates a typographic voice that is calm, confident, and technical without being cold. It says "we care about details at the pixel level."

- **Content-as-component design.** Instead of decorative illustrations, Vercel uses functional UI mockups -- browser windows, chat interfaces, code blocks, data tables, and network diagrams -- as visual content. The AI Gateway section shows real code and live model rankings. Marketing IS the product. This earns credibility with developer audiences.

- **The prismatic hero as the one extravagance.** Against the strict monochrome of the rest of the page, the rainbow-through-a-prism gradient is genuinely striking. In dark mode, warm colors bleeding into pure black is beautiful. It works because of the restraint everywhere else.

- **Border-line minimalism.** Sections separated by 1px `#ebebeb` borders, not background color bands. Cards have 1px borders with nearly invisible `0.04` opacity shadows. The page feels like one continuous surface with the faintest structural lines, creating visual coherence most marketing pages lack.

- **Social proof as inline narrative.** Company stats woven into a flowing sentence with brand-font logos inline, not a static logo bar. "runway build times went from 7m to 40s" is more engaging and credible than a grid of logos.

## Reusable Patterns

- **Monochrome + one gradient accent:** Constrain the entire palette to black/white/gray, then use a single dramatic gradient as the hero moment. The contrast between the restrained page and the expressive hero creates visual tension.

- **Tight-tracked Geist headings:** -4% to -6% letter-spacing on semibold sans-serif headings is immediately recognizable as premium developer-tool design. Geist (or similar geometric sans) with aggressive negative tracking signals technical precision.

- **Eyebrow/headline/description stack:** Small (14px), muted label above the heading, then body text below. "Fluid Compute" label above "A compute model for all workloads." The label categorizes; the headline sells. Used consistently throughout.

- **Pill CTAs with icons (100px radius, 48px height):** Full pill shape instantly distinguishes primary CTAs from nav buttons (6px radius, 32px height). Include a small icon (deploy arrow, sparkle) for visual weight.

- **Social proof as prose:** Weave customer metrics into flowing sentences with brand logos inline instead of a static logo bar. More engaging and credible for technical audiences.

- **Functional UI as illustration:** Replace decorative illustrations with actual product screenshots, code blocks, data tables, and interface mockups. Earns trust with developers by showing, not telling.

- **1px border section dividers:** `1px solid #ebebeb` between sections instead of alternating background colors or large whitespace. Clean structure without visual heaviness.

- **Geist Mono uppercase structural labels:** Monospace, uppercase, 12px for navigation/footer category headings. Technical, organized feel that differentiates structural elements from content.

- **Card shadow formula:** `rgba(0, 0, 0, 0.04) 0px 2px 2px` -- barely visible depth. Combined with 1px `#ebebeb` border. No floating or 3D illusion.

- **Brand-tinted template cards:** Pastel tint derived from framework brand color in upper region, white below. Adds personality without overwhelming the monochrome system.

- **Asymmetric 1/3 + 2/3 feature sections:** Text (label + heading + body + CTA) in left third, visualization/diagram in right two-thirds. Compact, scannable text side; visual side tells the deeper story.

- **True black dark mode (`#000`):** Not an afterthought -- the hero gradient looks more dramatic against pure black. Both modes feel intentionally designed, not theme-swapped. Dark mode is arguably the superior version.

- **64px transparent header:** No background, no blur, no border, just the logo and nav floating on the page. Maximum content breathing room.
