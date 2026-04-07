---
name: stripe-homepage
source: https://stripe.com
tags: [fintech, light, clean, gradient, enterprise, premium, developer, trust, scale]
captured: 2026-03-19
---

## Visual Analysis

### Color
- **Primary background**: Pure white (#FFFFFF) with off-white section alternation (#f8fafd)
- **Primary text**: Dark navy (#0a2540) -- not pure black, warmer and more sophisticated
- **Brand accent**: Purple/indigo (#533afd) -- used sparingly for CTAs, links, and interactive elements
- **Borders and dividers**: Soft cool gray (#e5edf5)
- **Hero background**: Signature gradient wave -- blues, purples, and teals blending into a dynamic mesh; served as optimized PNGs with animated overlay on capable devices
- **Color strategy is deliberately restrained**: the palette is 90% neutral (white, navy, gray) with purple as the sole accent. This restraint makes every colored element feel intentional and high-signal. No competing accent colors. No gradients in UI chrome -- gradients are reserved for the hero wave motif, making it iconic.
- **Dark navy (#0a2540) instead of black**: This is a signature Stripe move. It reads as authoritative without the harshness of pure black, creating a warmer, more approachable enterprise feel.

### Typography
- **Font family**: Custom proprietary sans-serif loaded via `--fontFamily` CSS variable (historically "Camphor" or a derivative; recent iterations appear to use a custom-commissioned variable font)
- **Weight range**: Light (300), Normal (400), Semibold (600), Bold (700) -- variable font with `font-variation-settings` support
- **Hero headline**: Very large, bold weight, tight line-height. Reads ~48-64px on desktop. The headline "Financial infrastructure to grow your revenue" is direct, benefit-oriented, and avoids jargon.
- **Subheadings**: Semibold, ~24-32px, used for section headers like "Flexible solutions for every business model"
- **Body text**: Normal weight (400), generous line-height (1.6-1.8x), likely 16-18px for readability
- **Statistics/data points**: Oversized bold numerals (e.g., "135+", "$1.9T", "99.999%") -- numbers are treated as visual anchors, almost like display type
- **Japanese/CJK support**: Explicit `font-variation-settings: "wght" 500-600` for CJK text, showing attention to internationalization at the typography level
- **Letter-spacing**: Minimal on body; headings appear to use default or very subtle tracking
- **Overall typographic personality**: Clean, confident, unhurried. The type does not shout -- it speaks clearly at appropriate volumes.

### Layout
- **Grid**: 12-column foundation with CSS custom property `--gridColumnCount`
- **Max-width**: ~1080-1264px for contained content; some sections go full-bleed
- **Responsive breakpoints**: Mobile (<600px), Tablet (600-899px), Desktop (>=900px)
- **Responsive padding**: Managed via `--columnPaddingNormal` token, adjusting per breakpoint
- **Section spacing**: Generous vertical rhythm, approximately 80-120px between major sections on desktop. White space is used aggressively -- Stripe treats empty space as a design element, not wasted real estate.
- **Hero structure**: Full-width with centered text over the signature wave background. Dual CTAs ("Get started" + "Sign up with Google") positioned inline below the subheadline.
- **Content hierarchy follows a conversion funnel**:
  1. Hero (vision + primary CTA)
  2. Logo carousel (social proof)
  3. Solutions grid (product depth)
  4. Statistics bar (scale validation)
  5. Enterprise case studies (credibility)
  6. Testimonials (voice of customer)
  7. Startup stories (aspiration)
  8. Platform section (expansion use case)
  9. Developer section (technical trust)
  10. News feed (momentum)
  11. Final CTA (conversion)
  12. Comprehensive footer (navigation)
- **Each section is self-contained**: modular blocks that could be reordered or A/B tested independently, yet the narrative flow feels deliberate and inevitable.

### Components
- **Buttons**:
  - Primary: Purple/indigo (#533afd) fill, white text, rounded corners (~6-8px radius), subtle hover darkening
  - Secondary: White/transparent fill, dark text, optional border, used for "Contact sales" and secondary actions
  - Ghost: Text-only links styled as inline CTAs ("Read the story", "View the guide") with arrow indicators
  - Consistent sizing: comfortable click targets with adequate internal padding

- **Cards**:
  - Shadow system with multiple tiers: `--cardShadowXSmall` through `--cardShadowXLarge` (CSS custom properties)
  - Standard border-radius: 4px and 8px
  - Customer story cards: thumbnail image + metric highlight + product tags + brief description
  - Solution cards: product illustration + heading + descriptive paragraph
  - Clean card surfaces with no internal borders or dividers -- content hierarchy managed through typography alone

- **Navigation bar**:
  - Fixed/sticky header, clean white background
  - Left: Stripe wordmark/logo
  - Center: Products, Solutions, Developers, Resources, Pricing
  - Right: "Sign in" (text link) + "Contact sales" (button)
  - Mega-dropdown menus on hover (product categories expand into rich panels)
  - BEM naming convention: `SiteHeader__container`

- **Logo carousel**: Horizontally scrolling customer logos (Amazon, Shopify, Google, NVIDIA, Ford, Uber, Anthropic, Figma) -- auto-rotating, clickable to case studies

- **Statistics display**: Large bold numerals with supporting descriptor text, arranged in a 4-column grid. Numbers function as visual anchors that break up text-heavy sections.
  - "135+ currencies and payment methods supported"
  - "$1.9T in payments volume processed in 2025"
  - "99.999% historical uptime for Stripe services"
  - "200M+ active subscriptions managed on Stripe Billing"

- **Testimonial blocks**: Customer quote + headshot (48px circle) + name + title + company. Four testimonials in a row, creating a wall of social proof.

- **Enterprise accordion/carousel**: Expandable case study panels (Hertz, URBN, Instacart, Le Monde) with key metrics and product badges

- **Developer section**: API stats prominently displayed (500M+ requests/day, 10K+ requests/sec, 150K+ transactions/min) with three integration path options (no-code, pre-integrated, custom)

- **News carousel**: 8 rotating editorial cards with thumbnails, creating a sense of ongoing momentum

- **Footer**: Dense, well-organized multi-column layout. 25+ product links, 15+ solution categories, developer resources, integrations, company info, legal. Language/region selector. Every link is a growth surface.

### Motion
- **Easing curve**: `cubic-bezier(.4, 0, .2, 1)` -- standard Material-like ease-out, smooth and professional without being bouncy or playful
- **Hero wave**: Signature animated gradient mesh on capable devices; gracefully degrades to static PNG (`wave-fallback-desktop.png`, `wave_crop.jpg`)
- **Carousel animations**: Logo carousel and news carousel with smooth horizontal transitions
- **Hover states**: Subtle color/opacity shifts on buttons and links; no dramatic transforms
- **Scroll behavior**: Sections appear to use subtle fade-in or slide-up reveals as they enter the viewport
- **Overall motion philosophy**: Restrained and purposeful. Animation serves wayfinding and delight, never distraction. The wave is the one "wow" moment; everything else is utility motion.

## What Makes It Work
- **Radical restraint creates authority.** Stripe uses fewer colors, fewer decorative elements, and more white space than almost any competitor. This restraint signals confidence -- a company that does not need to shout. Every element on the page earns its place.
- **Numbers as design elements.** Statistics like "$1.9T", "99.999%", and "500M+" are sized and styled as display typography, turning data points into visual anchors. This is not just social proof -- it is visual rhythm. The numbers break up text sections and create scannable landmarks.
- **Navy-not-black text with a single purple accent.** The #0a2540 dark navy is warmer than pure black, and #533afd purple is the sole accent in an otherwise neutral palette. This two-color discipline (navy + purple on white) creates an instantly recognizable visual identity that works at any scale.
- **The wave is the logo writ large.** The hero gradient wave echoes Stripe's parallelogram logo mark, creating brand cohesion between the smallest favicon and the largest page element. It is the one moment of visual extravagance on an otherwise austere page.
- **Conversion funnel as narrative arc.** The page reads like a story: here is our vision, here is who trusts us, here is what we build, here is the proof it works, here is how to start. Each section answers the next logical question a visitor would ask.

## Reusable Patterns
- **Single-accent color system**: Pick one brand color; use navy (not black) for text; use white and off-white for backgrounds. Resist adding a second accent. This forces visual clarity.
- **Statistics as visual anchors**: Size key numbers at 2-3x body text. Use bold weight. Arrange in a grid. Let the numbers do the talking -- no icons, no illustrations needed alongside them.
- **Tiered shadow system via CSS custom properties**: Define `--shadow-xs` through `--shadow-xl` tokens. Apply consistently to cards. Shadows create depth hierarchy without color.
- **Modular section architecture**: Each page section is a self-contained block with its own heading, content, and CTA. Sections can be reordered, A/B tested, or removed without breaking the page flow.
- **Progressive trust building**: Logo bar (breadth) then case studies with metrics (depth) then direct quotes (emotion) then developer stats (technical credibility). Layer different types of proof.
- **Generous white space as a premium signal**: 80-120px between sections. Do not fill every pixel. White space communicates that the company values the reader's attention and does not need to cram.
- **Hero pattern**: Big benefit-oriented headline + one-sentence supporting copy + dual CTAs (primary action + alternative sign-up method) + signature visual treatment behind. No feature lists, no product screenshots, no complexity in the hero.
- **Dense, comprehensive footer**: Treat the footer as a site map and growth surface. Every product, solution, and resource linked. Include region selector for global audiences.
- **BEM component naming**: `SiteHeader__container`, `Card--shadowMedium` pattern for maintainable, composable CSS architecture.
- **Responsive image optimization**: Serve images via CDN with width (`w=`) and quality (`q=`) parameters. Provide static fallbacks for animated elements.
