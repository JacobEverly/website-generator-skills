---
name: resend-homepage
source: https://resend.com
tags: [developer-tool, dark, minimal, api, email, saas, code-first]
captured: 2026-03-19
---

## Visual Analysis

### Color

- **Background:** Pure black `#000000` (`rgb(0, 0, 0)`) -- the entire page sits on true black, not near-black. This is a bold choice that makes everything else pop.
- **Primary text:** Off-white `#f0f0f0` (`rgb(240, 240, 240)`) -- slightly softened from pure white, reducing eye strain on the dark background.
- **Secondary text:** Muted gray `#a1a4a5` (`rgb(161, 164, 165)`) -- used for nav links, subtitles, and supporting copy. Cool-toned, not warm.
- **Tertiary text:** `rgba(241, 247, 254, 0.71)` -- a semi-transparent blue-white used for footer links and less prominent text. Achieves a frosted, receding effect.
- **Accent purple:** `rgb(146, 129, 247)` / `rgb(186, 167, 255)` -- a soft lavender-violet used sparingly for highlights, borders, and the "Integrate tonight" section icon glow.
- **Accent green:** `rgba(70, 254, 165, 0.83)` / `rgba(68, 255, 164, 0.62)` -- a vibrant mint green used for status indicators ("Delivered" badge), deliverability metrics (98%), and success states.
- **Accent blue:** `rgb(59, 158, 255)` / `rgb(112, 184, 255)` -- used for links, the email preview CTA button (`rgb(0, 163, 255)`), and code syntax highlighting.
- **Accent warm tones:** Yellow `rgb(255, 202, 22)` / `rgb(255, 214, 10)` for warnings/stars, red-pink `rgb(255, 149, 146)` / `rgba(255, 100, 101, 0.92)` for bounced/error states, orange for category badges.
- **Gradients are everywhere but subtle:**
  - Hero text uses `linear-gradient(to right bottom, rgb(255,255,255) 30%, rgba(255,255,255,0.5))` -- text that fades from bright to translucent.
  - Section dividers use horizontal line gradients: `linear-gradient(90deg, transparent, gray 50%, transparent)` -- creating glowing separator lines.
  - Cards use `linear-gradient(rgb(27,27,27), rgb(3,3,3))` -- barely-there dark-to-darker gradients.
  - Radial glow: `radial-gradient(70% 80% at 50% 0%, rgba(255,255,255,0.06) 3%, transparent)` -- a subtle white radiance at the top of sections.
  - The purple accent gradient: `linear-gradient(to right bottom in oklab, rgb(146,129,247), rgb(154,84,220))` -- modern oklab color space for smoother transitions.
- **Background accents:** Extremely low-opacity colored washes -- `rgba(34, 255, 153, 0.118)` green, `rgba(131, 84, 254, 0.21)` purple, `rgba(0, 119, 255, 0.227)` blue -- used as tinted backgrounds on SDK/language selector pills.
- **CSS custom property `--background: #000`** confirms the intentional pure-black foundation.

### Typography

- **Three distinct font families creating a clear hierarchy:**
  1. **Domaine** (serif) -- Used exclusively for the hero H1 ("Email for developers") and the closing CTA ("Email reimagined. Available today."). A refined, high-contrast serif that signals editorial quality and sophistication. Weight 400, letter-spacing `-0.96px` (tight).
  2. **ABC Favorit** (sans-serif, `--font-display`) -- The display/heading font for all H2 section headings ("Integrate tonight", "First-class developer experience", etc.). A geometric sans with subtle personality. Weight 400, aggressive letter-spacing of `-2.8px` at 56px, creating dense, impactful headlines. Line-height is 1.2x (67.2px at 56px).
  3. **Inter** (sans-serif, `--font-sans`) -- Body text, navigation, buttons, UI elements. The workhorse. 16px base, 24px line-height (1.5x), weight 400 for body, 500-600 for interactive elements.
  4. **Commit Mono** (monospace, `--font-mono`) -- Code blocks and technical content. 14px in the code editor. A distinctive monospace choice over standard system fonts.
- **Heading scale:**
  - H1 hero: 96px, domaine serif, `-0.96px` tracking, line-height 1:1 (96px)
  - Section H2s: 56px, ABC Favorit, `-2.8px` tracking (extremely tight, `-5%`), line-height 1.2x
  - Sub-section H2s: 20px, ABC Favorit, normal tracking
  - Feature H4s: 20px or 16px, ABC Favorit, `-0.8px` tracking at smaller sizes
  - Closing CTA: 76.8px, domaine serif
- **Font weight strategy:** Almost everything is weight 400 (regular). Bold is reserved for buttons (600 semibold) and the nav CTA. This restraint is key to the elegant feel -- the typographic hierarchy is built through size and font family, not weight.
- **Letter-spacing philosophy:** Negative tracking everywhere. Headers are compressed (up to -5%), body is normal. This creates a dense, modern, editorial feel.

### Layout

- **Container:** Max-width ~1200px centered, with generous horizontal padding.
- **Section rhythm:** Extremely generous vertical spacing between sections -- roughly 120-200px of breathing room. Each section feels like its own world.
- **Left-aligned section pattern:** Major section headings (H2) are left-aligned at the container edge, with supporting text below. Feature cards then span the full width or use a 2-column grid.
- **Hero:** Full-width, centered content. The H1 is left-of-center due to the 3D cube visual on the right. Two CTA buttons side by side beneath the subtitle.
- **Two-column feature cards:** Sections like "First-class developer experience" and "Go beyond editing" use a 2-column grid of equal-width cards with consistent padding.
- **Logo bar:** Centered, 6+5 grid of customer logos with "Companies of all sizes trust Resend" text above. Logos are white/desaturated on the black background.
- **Code section:** Full-width card with a tabbed interface (Node.js, Next.js, Remix, etc.) -- occupies the entire container width.
- **Sticky navigation:** Fixed top bar that persists on scroll.
- **Alternating center/left alignment:** The "Integrate tonight" section is center-aligned (code block centered), while "First-class developer experience" snaps to left alignment. This alternation creates visual variety.
- **Full-page flow:** Hero > Logo bar > Integrate (code) > Developer experience (feature cards) > Write (editor preview) > Go beyond editing (analytics cards) > Develop with React (code + preview split) > Reach humans (deliverability grid) > Everything in your control (dashboard preview) > Beyond expectations (testimonials) > Closing CTA.

### Components

- **Navigation bar:** Clean horizontal bar, transparent background blending with hero. Left: Resend logo (custom wordmark in a rounded, slightly italic serif). Center: Features, Company, Resources, Help, Docs, AI (all with dropdown chevrons), Pricing. Right: "Log In" (ghost style) + "Get Started" (outlined pill, `border-radius: 16px`, 2px white border at 5% opacity).
- **CTA buttons (primary):** Pill-shaped (`border-radius: 16px`), white text, no fill -- instead they use a barely-visible 2px border (`oklab(white / 0.05)`). 16px font, 600 weight, `padding: 0 20px`. The ghost border creates a sophisticated, understated feel. Hover likely fills.
- **CTA buttons (secondary):** Same pill shape, gray text (`#a1a4a5`), transparent 1px border. "Documentation", "Check the Docs", "Contact Us".
- **SDK language pills:** Horizontal scrollable row of icon+label pills with tinted backgrounds at ~10-20% opacity. Each language gets its own accent color: green for Node.js, purple for Serverless, red for Ruby, blue for Python, purple for PHP, cyan for Go, orange for Rust, etc. Rounded, small padding.
- **Code editor component:** Dark card with a tabbed framework selector at top (Node.js, Next.js, Remix, Nuxt, Express, Hono, Redwood, Bun, Astro). Syntax highlighting with muted colors. Line numbers in gray. Code in Commit Mono at 14px. The card has a subtle border (`1px solid rgba(217, 237, 255, 0.365)`).
- **Feature cards:** Dark gradient backgrounds (`linear-gradient(rgb(27,27,27), rgb(3,3,3))`), rounded corners, subtle ring shadows (`rgba(176, 199, 217, 0.145) 0px 0px 0px 1px`). Contains an icon, heading, description text, and "Learn more" link. Internal card content shows miniature UI previews (e.g., email status badges, webhook event logs).
- **Status badges:** Small pills with colored backgrounds -- "Delivered" (green), "Clicked" (purple), "Bounced" (red), "Spam" (dark). Rounded corners, tight padding.
- **Dashboard preview cards:** Show actual product UI -- analytics with large percentage numbers (98%, 41%), status dots, metric labels. Cards have the same dark gradient + subtle border treatment.
- **3D icons:** Each major section has a 3D-rendered icon (cube for hero, envelope for "Integrate", lens for "Write", atom for "React"). These are high-quality WebGL or pre-rendered assets with reflective/metallic surfaces, sitting on the pure black background. They provide the only "rich" visual texture on an otherwise flat page.
- **Email preview split-view:** Left pane shows TSX code (file tree + source), right pane shows the rendered email. The email itself uses standard email formatting (white background, blue CTA button) -- a deliberate contrast to the dark surrounding.
- **Deliverability feature grid:** 3-column by 3-row grid of small feature items, each with an icon and title. Minimal, list-like.
- **Footer:** Multi-column link layout (Products, Resources, Company, Legal). "All systems operational" status badge with green dot. Social icons (circular, `border-radius: full`). Very standard SaaS footer but on pure black.
- **Horizontal separator lines:** Thin gradient lines that glow from center outward -- `linear-gradient(90deg, transparent, gray 50%, transparent)`. Used between major sections. Sometimes enhanced with a conic-gradient dot pattern.

### Motion

- **CSS animation definitions found in custom properties:**
  - `hero-text-slide-up-fade` (1s ease-in-out) -- Hero text slides up with fade on load.
  - `webgl-scale-in-fade` (1s ease-in-out) -- 3D icons scale in with fade.
  - `header-slide-down-fade` (1s ease-in-out) -- Navigation slides down on page load.
  - `fade-in` (0.2s ease) / `fade-out` (0.2s ease) -- Standard micro-interactions.
  - `shine` (1s ease, 0.8s delay) -- A shine/shimmer effect, likely on the CTA button border.
  - `disco` (6s linear infinite) -- An infinitely cycling border color animation (rainbow effect for a featured element).
  - `scroll-x` (180s linear infinite) -- A slow horizontal scroll, likely for the logo bar marquee.
  - `scroll-x` broadcast variant (48s linear infinite) -- Faster horizontal scroll.
  - `pulse` (2s infinite) -- Subtle pulsing on status indicators.
  - `caret-blink` (1.2s infinite) -- Terminal cursor blink in code areas.
  - `plop` / `plop2` / `plop3` -- Staggered entrance animations at 0.1s, 0.2s, 0.4s delays.
  - `enterFromRight`, `enterFromLeft`, `exitToLeft`, `exitToRight` (0.25s ease) -- Dropdown transitions.
  - `scaleIn` (0.2s ease), `scaleOut` (0.16s ease) -- Modal/popover appearances.
  - Collapsible/accordion: 0.3s ease-in-out slide-down/slide-up.
- **Default transition:** `0.15s` with `cubic-bezier(0.4, 0, 0.2, 1)` easing -- snappy, not floaty.
- **Overall motion philosophy:** Entrance animations are slow and theatrical (1s for hero elements), micro-interactions are fast (0.15-0.25s). The page has scroll-triggered reveals. The 3D icons have subtle animation. The logo bar scrolls horizontally on an infinite loop.

## What Makes It Work

- **True black as a canvas.** Most "dark mode" sites use near-black (gray-900/950). Resend uses `#000000`. This creates maximum contrast for the 3D icons and gradient effects, making the page feel like objects floating in infinite space. It requires extreme care with contrast ratios and border treatments -- and they execute it flawlessly.
- **Three-font hierarchy with restraint.** The combination of Domaine (serif, for emotional hooks), ABC Favorit (geometric sans, for functional headings), and Inter (for everything utilitarian) creates a typographic system that is both expressive and systematic. Crucially, they avoid using bold weight as a crutch -- the hierarchy is built through font family and size alone, which feels far more sophisticated.
- **The ghost button idiom.** Their primary CTA uses a nearly-invisible 2px border (white at 5% opacity) on a transparent background. This is counterintuitive -- most SaaS sites scream with filled, high-contrast CTAs. Resend's approach works because it matches the overall restraint, makes the few filled elements feel like product UI, and signals confidence.
- **3D icons as section anchors.** Each major section has a single 3D-rendered icon with metallic/glass materials. These serve as visual "rest stops" between text-heavy sections, creating rhythm and communicating craft.
- **Gradient lines as section dividers.** Instead of whitespace alone, Resend uses gradient lines that glow from center outward. This reinforces the "objects floating in darkness" aesthetic and adds polish without adding visual weight.
- **Product UI as marketing.** Several sections show actual dashboard screenshots styled as dark cards -- status badges, analytics numbers, code editors. This sells the product by letting it speak for itself rather than abstracting features into illustrations.

## Reusable Patterns

- **Pure-black background + off-white text system:** `#000` background, `#f0f0f0` primary text, `rgba(white, 0.71)` secondary text, `#a1a4a5` tertiary. A clean 3-tier text hierarchy on true black.
- **Ghost pill buttons:** `border-radius: 16px`, 2px border at 5% opacity, no fill, 600 weight text. Primary in white, secondary in gray. Extremely subtle but still actionable.
- **Tinted pill selectors:** For SDK/language/framework selectors -- small pills with 10-20% opacity colored backgrounds matching the item's brand color. Provides color coding without overwhelming.
- **Dark gradient cards:** `linear-gradient(rgb(27,27,27), rgb(3,3,3))` background with `1px ring-shadow at ~15% opacity`. Rounded corners (12-16px). Contains feature content with embedded UI previews.
- **Gradient separator line:** `linear-gradient(90deg, transparent 0%, gray 50%, transparent 100%)` as a 1px-tall element. Creates a glowing horizontal rule that fades at the edges.
- **Radial top-glow on sections:** `radial-gradient(70% 80% at 50% 0%, rgba(255,255,255,0.06), transparent)` applied to section containers. Adds a subtle luminous wash at the top.
- **Code block with framework tabs:** Tabbed interface for multi-framework code examples. Dark background, monospace font (Commit Mono), line numbers, syntax highlighting in muted pastels.
- **Scroll-triggered entrance animations:** Hero uses 1s slide-up-fade, 3D elements use 1s scale-in-fade, UI elements use staggered "plop" animations with 0.1-0.4s delays.
- **Logo bar with trust copy:** Customer logos in white/desaturated on black, centered, with a single line of gray trust text above. Simple, high-signal social proof.
- **Left-aligned section headings:** Large H2 (56px, tight tracking) left-aligned, with 1-2 lines of gray body text below, then content spanning the full width. Creates a strong reading entry point.
- **Status color system:** Green = delivered/success, purple = clicked/interaction, red = bounced/error, yellow = warning. Applied as small filled pills with rounded corners.
- **Serif for emotion, sans for function:** Reserve serif typography exclusively for the opening and closing emotional hooks. Use geometric sans for all section headings. Use utilitarian sans for everything else. Never mix within a role.
