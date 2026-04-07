---
name: raycast-homepage
source: https://raycast.com
tags: [productivity, dark, polished, mac-native, developer-tool, launcher, keyboard-first, glassmorphism, spring-physics, bento-grid]
captured: 2026-03-19
---

## Visual Analysis

### Color

- **Background**: Near-black `#07080a` (CSS var `--grey-900`) -- not pure black, carries a subtle cool undertone that prevents harshness and reads as a premium digital surface
- **Background tier system** via CSS custom properties (critical for dark-mode depth):
  - `--color-bg` / `--grey-900`: `#07080a` (page base)
  - `--color-bg-100`: `rgb(16,17,17)` (first surface elevation)
  - `--color-bg-200`: `rgb(24,25,26)` (card/panel backgrounds)
  - `--color-bg-300`: `rgb(49,49,51)` (interactive elements)
  - `--color-bg-400`: `rgb(73,75,77)` (hover/active states)
- **Full grey scale** with 10 named stops: `--grey-50: #e6e6e6` / `--grey-100: #cdcece` / `--grey-200: #9c9c9d` / `--grey-300: #6a6b6c` / `--grey-400: #434345` / `--grey-500: #2f3031` / `--grey-600: #1b1c1e` / `--grey-700: #111214` / `--grey-800: #0c0d0f` / `--grey-900: #07080a`
- **Text color hierarchy** (4 tiers, white only, varied by opacity/shade):
  - Primary: `#ffffff` (headings, hero text, strong emphasis)
  - Secondary: `rgb(156,156,157)` / `--grey-200` (nav links, paragraph descriptions)
  - Tertiary: `rgb(106,107,108)` / `--grey-300` (labels like "Favorite Feature:", sub-descriptions, section subtitles)
  - Quaternary: `rgb(94,99,102)` / `--color-fg-400` (metadata, timestamps, lowest priority)
- **Semantic accent colors** (functional, not decorative):
  - Blue: `hsl(202,100%,67%)` / `--blue-dark: #56c2ff` + `hsla(202,100%,67%,0.15)` transparent
  - Red: `hsl(0,100%,69%)` / `--red-dark: rgba(255,99,99,1)` + 15% transparent
  - Yellow: `hsl(43,100%,60%)` + 15% transparent
  - Green: `hsl(151,59%,59%)` + 15% transparent
- **Primary CTA color**: `rgb(230,230,230)` (off-white, not pure #fff) with text `rgb(47,48,49)` -- the inverse of the page palette, making it the brightest single element
- **Border color**: `hsl(195,5%,15%)` / `--color-border` -- barely visible, structural separation only
- **Card borders via inset shadow**: `rgba(255,255,255,0.06) 0px 0px 0px 1px inset` -- softer than actual CSS borders
- **Divider elements**: `rgba(255,255,255,0.1)` at 12px height -- thin, understated section breaks
- **Extension card strategy**: Each card pulls its integrated app's brand color (Spotify green `#0D6E30`, Slack purple `#4a154b`, Linear blue `#2b5eb4`) into radial/linear gradients at 0.04-0.42 opacity -- creating color variety without palette chaos
- **Red brand accent**: Used sparingly -- logo, AI section badge, keyboard glow. Never as a background or large surface

### Typography

- **Primary font**: Inter (loaded via Next.js `var(--font-inter)`), fallback: `"Inter Fallback", sans-serif`
- **Monospace font**: `var(--font-jetbrains-mono), Menlo, Monaco, Courier, monospace`. GeistMono also appears for homebrew install text: `GeistMono, ui-monospace, SFMono-Regular, "Roboto Mono", Menlo, Monaco, "Liberation Mono", "DejaVu Sans Mono", "Courier New", monospace`
- **Font feature settings**: `"liga", "calt", "kern", "ss03"` -- ligatures, contextual alternates, kerning, and stylistic set 03 (alternate characters) all enabled
- **Rendering**: `-webkit-font-smoothing: antialiased` (subpixel smoothing disabled for cleaner rendering on dark backgrounds)
- **Base line-height**: `1.15` on html root
- **Exact type scale** (extracted from computed styles):
  - **H1 hero**: 64px / weight 600 / line-height 70.4px (1.1x) / letter-spacing normal / text-align center / max-width 540px
  - **H2 section headings**: 20px / weight 500 / line-height normal / letter-spacing 0.2px
  - **Body large**: 18px / weight 400 / line-height normal / letter-spacing 0.2px
  - **Body standard**: 16px / weight 500 / line-height 25.6px (1.6x) / letter-spacing 0.2px or normal
  - **Body small**: 14px / weight 500 / line-height 24px or 22.4px / letter-spacing 0.2px
  - **Caption/meta**: 13px / weight 400 / line-height 20px / letter-spacing 0.1px
  - **Code/technical**: 12px / weight 400 / GeistMono
- **Key insight**: Section headings (h2) are only 20px -- smaller than many sites' body text. They rely on weight differentiation (500 vs 400) and massive whitespace to create hierarchy. The hero headline is the only truly large typography. This keeps the entire page calm and confident.
- **Consistent letter-spacing**: 0.2px across most text sizes (body, headings, labels) -- adds micro-legibility without visible looseness. Labels use 0.3px.

### Layout

- **Container system** (CSS custom properties):
  - `--container-xs-width`: 746px (narrow content, reading width)
  - `--container-sm-width`: 1064px (medium content)
  - `--container-width`: 1204px (default, most sections)
  - `--container-lg-width`: 1280px (wide content)
  - `--grid-gap`: 32px
- **Viewport**: Optimized for 1440px width
- **Navbar dimensions**: Fixed position, `z-index: 2`, total height 92px (76px inner bar + 16px top padding from `--navbar-container-padding-top: 16px`). `--navbar-height: 58px` for content height, `--navbar-total-spacing` calculated as `calc(58px + 2 * 16px)`
- **Spacing scale** (8px base unit, 16 steps):
  - `--spacing-0-5`: 4px | `--spacing-1`: 8px | `--spacing-1-5`: 12px | `--spacing-2`: 16px
  - `--spacing-2-5`: 20px | `--spacing-3`: 24px | `--spacing-4`: 32px | `--spacing-5`: 40px
  - `--spacing-6`: 48px | `--spacing-7`: 56px | `--spacing-8`: 64px | `--spacing-9`: 80px
  - `--spacing-10`: 96px | `--spacing-11`: 112px | `--spacing-12`: 168px | `--spacing-13`: 224px
  - Notable: exponential jump at the top end (112 -> 168 -> 224) for massive section spacing
- **Border-radius scale**:
  - `--rounding-none`: 0px | `--rounding-xs`: 4px | `--rounding-sm`: 6px | `--rounding-normal`: 8px
  - `--rounding-md`: 12px | `--rounding-lg`: 16px | `--rounding-xl`: 20px | `--rounding-xxl`: 24px | `--rounding-full`: 100%
- **Section rhythm**: Enormous vertical spacing between sections (estimated 120-224px). Each section occupies its own "room." The page is ~15,500px tall but never feels cramped
- **Hero composition**: Headline centered vertically in viewport with max-width 540px. Sub-headline below at reduced opacity. Download buttons pushed to bottom of viewport. Deliberate dead space above and below creates gravitational pull to the headline
- **Grid patterns**: Primarily flexbox with 8px gap for component layouts. Feature sections use left-text / right-visual asymmetric splits. Extension cards in horizontal grid with category tabs above
- **Footer**: 6-column layout (Product, Core Features, Top Extensions, Company, Community, By Raycast), 24px horizontal padding

### Components

- **Floating glass navbar**:
  - Outer wrapper: fixed, `padding: 16px 16px 0px`, z-index 2
  - Inner bar (`.Navbar_navbar__XlgWY`): `backdrop-filter: blur(5px)`, `border: 1px solid rgba(255,255,255,0.06)`, `border-radius: 16px`, `padding: 16px 32px`, height 76px
  - Box-shadow: `rgba(255,255,255,0.15) 0px 1px 1px 0px inset` -- single inset top-edge light catch simulating glass refraction
  - Nav links: 14px, weight 500, `rgb(156,156,157)`, no text-decoration
  - Logo: Red gradient icon + "Raycast" wordmark in white
  - Right side: "Log in" text link + Download CTA pill
- **Download button (primary CTA)** -- the standout component:
  - Background: `rgb(230,230,230)` (warm off-white)
  - Text: `rgb(47,48,49)` (dark grey, not pure black)
  - Border-radius: 8px (`--rounding-normal`)
  - Padding: 8px 12px
  - Font: 14px / weight 500
  - **4-layer box-shadow**: `rgba(0,0,0,0.5) 0px 0px 0px 2px` (dark outline ring), `rgba(255,255,255,0.19) 0px 0px 14px 0px` (white outer glow), `rgba(0,0,0,0.2) 0px -1px 0.4px 0px inset` (bottom inner shadow), `rgb(255,255,255) 0px 1px 0.4px 0px inset` (top inner highlight) -- creates a glowing, beveled, physically press-able button. The 14px white glow makes this the absolute brightest element on the entire page
  - Apple icon precedes "Download" text
- **Extension cards**:
  - Border-radius: 20px (`--rounding-xl`)
  - Multi-layer box-shadow: `rgba(255,255,255,0.1) 0px 1px 0px 0px inset` (top highlight), `rgba(7,13,79,0.05) 0px 0px 20px 3px` (ambient glow), `rgba(7,13,79,0.05) 0px 0px 40px 20px` (deep shadow), `rgba(255,255,255,0.06) 0px 0px 0px 1px inset` (border simulation)
  - Background: per-card brand-color radial/linear gradients (opacity 0.04-0.42)
  - Content: 128px icon, title, description, background imagery
  - Category tabs above: "Productivity", "Engineering", "Design", "Writing" in pill selector
- **Value proposition cards** (Fast / Ergonomic / Native / Reliable):
  - Small cards with SF Symbol-style icon + bold label + short description
  - Alongside a large keyboard illustration
  - Text pattern: **"Fast."** Think in milliseconds. / **"Native."** Pure performance. / **"Reliable."** 99.8% crash-free rate.
  - Thin `rgba(255,255,255,0.1)` dividers between items
- **AI section**:
  - Red "AI" eyebrow label above heading
  - Full-width dark mockup of AI chat interface with red/maroon accent glow
  - Three columns below: "Ask Anything, Anytime, Anywhere" / "Always On ChatGPT" / "Your Automation Assistant"
  - Bold lead-in sentence + lighter description per column
- **Feature grid** (Snippets, Quicklinks, Hotkeys, AI Commands):
  - 2x2 bento grid of dark elevated cards
  - Each contains a live-looking UI mockup or product screenshot
  - Elevated backgrounds with subtle card treatment
  - Per-card label + description below
- **Testimonial section**:
  - Row of three featured users with circular avatars + name + title (MKBHD, Guillermo Rauch/CEO Vercel, etc.)
  - Detailed quote card below with "Favorite Feature:" and "Top Extension:" metadata labels
  - Labels: 14px, weight 500, `rgb(106,107,108)`, letter-spacing 0.3px
  - Quote with key phrases in **bold white** against `--grey-300` base text
- **Community cards** (Slack / X/Twitter):
  - Side-by-side cards with monospace member/follower counts ("32k members", "80k followers")
  - Arrow links: "Join ->" / "Follow ->" with right arrow
  - Bordered with subtle 1px stroke: `1px solid rgba(255,255,255,0.1)`
  - Border-radius: 6px
  - Below: horizontal scroll of YouTube video thumbnails with rounded corners
- **Developer section** ("Build the perfect tools"):
  - Large display heading on left, 3D isometric illustrations on right
  - Illustrations use Raycast blue (`#56c2ff` range) on dark background
  - Monospace labels (`F16_01`, `F16_03`) annotating illustrations -- technical/schematic aesthetic
  - Sub-sections: "React to macOS", "Built-in UI", "Batteries included", "Publish to the Store"
- **Newsletter input**:
  - Background: `rgba(255,255,255,0.05)` -- nearly invisible field
  - Border: `1px solid rgba(255,255,255,0.05)`
  - Border-radius: 8px
  - Font: 14px, placeholder "chris@swift-lang.com" (subtle personality)
  - Submit button: `rgba(255,255,255,0.9)` bg, black text, 4px border-radius -- compact, matches primary CTA style
- **Keyboard visualization** (closing CTA):
  - Photorealistic keyboard rendering with warm red/amber glow emanating from command key and spacebar
  - Creates a visceral "feel" of keyboard-first interaction as final brand impression
  - Full QWERTY layout including function row and modifier keys

### Motion

- **Scroll-triggered entrance animations**: `page_fadeInUp__yDeSr` and `page_fadeInUpStagger__UbVUU` -- elements fade in (opacity 0 to 1) while translating upward. Staggered variant creates cascading reveals across sibling elements (~100ms delay between items)
- **Spring physics easing**: Custom `--spring-1` CSS variable defines a 100-point `linear()` easing function -- a physically modeled spring curve with slight overshoot at ~40% (peaks at 1.043) and natural settle. This is advanced: most sites use cubic-bezier, Raycast bakes real spring dynamics into CSS. Used for interactive transitions throughout
- **3D WebGL hero element**: Animated red/maroon refractive cube (Three.js) with glass material properties (IOR: 1.5, transmission: 1.0, chromatic aberration). Provides constant ambient movement in the hero without demanding attention
- **Opacity-linked scroll reveals**: Sub-hero text observed at near-zero opacity (`0.003`), indicating scroll-position-linked progressive reveal -- text materializes as user scrolls
- **Keyboard key animation**: `AnimatedCmdSpaceKeyboard` component demonstrates the Cmd+Space activation gesture with key depression animation
- **Hover states**: Implied by the multi-layer shadow system on cards (shadow intensification on hover). Nav links and buttons have smooth transition states
- **Motion philosophy**: Deeply restrained. No bouncing, sliding, or attention-demanding animations. Motion serves to pace the reading experience (scroll reveals) and provide physicality (spring easing). The 3D cube is the single expressive motion element. Everything else is triggered by user scroll, never auto-playing

## What Makes It Work

- **Chromatic discipline on a dark canvas**: The near-black `#07080a` is not pure black -- it has just enough warmth to prevent the "floating in void" feeling. All color is earned: white text, muted grey links, and per-brand extension card accents are the only color sources. The red brand accent appears in only three places (logo, AI badge, keyboard glow). This restraint makes every color moment feel intentional and premium.

- **Inverted brightness hierarchy for CTA**: On a dark page, the primary action (Download) is the brightest element -- an off-white pill with a 4-layer box-shadow that literally glows with a 14px white radial bloom. This is the opposite of typical light-mode CTAs and creates an unmissable focal point without using color. The button's inner beveled shadows (top highlight + bottom shadow at sub-pixel offsets) create a tactile, physically press-able quality that most flat designs lack entirely.

- **Massive whitespace as confidence signal**: Section headings are only 20px -- often smaller than body text on other sites. The page compensates with enormous vertical spacing (168-224px between sections from the exponential spacing scale). This says "we don't need to shout" and creates a reading rhythm where each section gets its own contemplative moment. The hero is the boldest example: a 64px headline centered in a nearly empty viewport with max-width 540px.

- **The floating glass navbar as brand statement**: `backdrop-filter: blur(5px)` + `border: 1px solid rgba(255,255,255,0.06)` + `inset box-shadow: rgba(255,255,255,0.15) 0px 1px 1px` creates a frosted glass panel that communicates "Mac-quality product" before reading a word. The 16px border-radius makes it feel like an OS element, not a website component.

- **Spring physics embedded in CSS**: The `--spring-1` custom property with 100-point `linear()` easing is a level of motion craft most sites never reach. It means every transition -- hover states, scroll reveals, interactive elements -- has physically correct overshoot and settle rather than arbitrary cubic-bezier curves. This contributes to an unconscious feeling of "this feels right" throughout the experience.

- **Dark mode done right through inset shadow elevation**: Four distinct background tiers prevent the "everything is black" problem. Cards float above surfaces via inset top-edge highlights (`rgba(255,255,255,0.1) 0px 1px 0px inset`) that simulate light catching a raised edge -- this is how physical objects work under overhead light. Border simulation via inset box-shadows (`rgba(255,255,255,0.06) 0px 0px 0px 1px inset`) is softer and more natural than actual CSS borders on dark surfaces.

- **Declarative copywriting with typographic authority**: "Your shortcut to everything." -- period, not exclamation. "It's not about saving time." Section headings read like claims, not labels. Combined with Inter at weight 600/500 and the `ss03` stylistic set, the typography carries quiet confidence. The conversational tone paired with precise typographic treatment creates voice.

- **Individual personality in a uniform grid**: Each extension card gets its own brand-color gradient rather than identical styling. The structural consistency (same size, same 20px radius, same 4-layer shadow) holds the grid together while the color variation (Spotify green, Slack purple, GitHub grey) makes it feel alive and ecosystem-rich.

## Reusable Patterns

- **Glass floating navbar**: Fixed wrapper with 16px padding, inner bar with `backdrop-filter: blur(5px)`, `border: 1px solid rgba(255,255,255,0.06)`, `border-radius: 16px`, `box-shadow: rgba(255,255,255,0.15) 0px 1px 1px 0px inset`. Instantly premium, easy to implement.

- **4-layer CTA button glow**: (1) `rgba(0,0,0,0.5) 0px 0px 0px 2px` dark outline, (2) `rgba(255,255,255,0.19) 0px 0px 14px` outer glow, (3) `rgba(0,0,0,0.2) 0px -1px 0.4px inset` bottom inner shadow, (4) `rgb(255,255,255) 0px 1px 0.4px inset` top highlight. Creates physical button depth on any dark background.

- **Card elevation via inset shadows**: Replace drop shadows on dark backgrounds with `inset 0px 1px 0px rgba(255,255,255,0.1)` (top-edge light catch) + `inset 0px 0px 0px 1px rgba(255,255,255,0.06)` (border simulation). More natural than box-shadow underneath.

- **8px spacing scale with exponential ceiling**: 4/8/12/16/20/24/32/40/48/56/64/80/96/112/168/224px. The dramatic jump at 112->168->224 is the secret to Raycast's section-level breathing room. Adopt the full scale as CSS custom properties.

- **10-stop grey scale**: `#e6e6e6` / `#cdcece` / `#9c9c9d` / `#6a6b6c` / `#434345` / `#2f3031` / `#1b1c1e` / `#111214` / `#0c0d0f` / `#07080a`. Fine-grained enough for 4 text tiers + 4 background tiers + borders.

- **Border-radius scale**: 0/4/6/8/12/16/20/24/100% as named tokens. Buttons get 8px, cards get 20px, navbar gets 16px, pills get 100%. Consistency without monotony.

- **Spring easing via CSS `linear()`**: 100-point spring curve stored as `--spring-1` custom property. Provides physically-correct overshoot and settle for all transitions. Drop-in replacement for cubic-bezier.

- **Section heading pattern**: 20px / weight 500 heading in white + same-size subheading in `--grey-300`. No large headings needed -- let 168-224px spacing do the hierarchy work.

- **Bold-lead description pattern**: "**Fast.** Think in milliseconds." / "**Native.** Pure performance." Single bolded keyword followed by brief supporting phrase. Scannable, punchy, eliminates filler.

- **Brand-color gradient cards**: Each card in a grid applies its subject's brand color as a radial gradient at 0.04-0.42 opacity, with matching tinted shadows. Creates visual variety while maintaining structural uniformity.

- **Near-invisible input fields**: Background `rgba(255,255,255,0.05)`, border `rgba(255,255,255,0.05)`, border-radius 8px. The field barely exists until focused. Pairs with a high-contrast submit button to direct action.

- **Structured testimonial cards**: "Favorite Feature:" label in tertiary grey + feature badge, "Top Extension:" label + extension badge, pull-quote with bold key phrases, avatar + name + title. More browseable and credible than simple blockquotes.

- **Monospace annotations on illustrations**: Small `F16_01`-style labels in monospace font placed on isometric 3D illustrations. Creates a technical/schematic feel that reinforces developer credibility.

- **Version info as quiet confidence**: "v1.104.10 | macOS 13+ | Install via homebrew" tucked below CTAs in 12px monospace at reduced opacity. Signals maturity, active development, and developer-friendliness without cluttering the message.
