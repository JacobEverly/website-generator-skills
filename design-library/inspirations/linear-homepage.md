---
name: linear-homepage
source: https://linear.app
tags: [project-management, dark, minimal, animation-heavy, developer-tool, ai-native, product-development, monospace-accent, opacity-hierarchy]
captured: 2026-03-19
---

## Visual Analysis

### Color

- **Background (primary):** `rgb(8, 9, 10)` -- near-black, not pure black. This is the dominant surface color across the entire page.
- **Background (elevated surfaces):** Subtle steps up from the base -- `rgb(9, 10, 11)`, `rgb(15, 16, 17)`, `rgb(16, 17, 18)`, `rgb(18, 19, 20)`, `rgb(22, 23, 24)`. Surfaces are distinguished by 1-3 RGB value increments rather than color shifts.
- **Background (interactive/cards):** `rgba(255, 255, 255, 0.02)` through `rgba(255, 255, 255, 0.08)` -- white at extremely low opacity layered on top of the dark base. This creates a frosted-glass depth effect.
- **Text (primary):** `rgb(247, 248, 248)` -- warm off-white, not pure white. Used for hero headings and high-priority content.
- **Text (secondary):** `rgb(208, 214, 224)` -- cool silver-gray. Used for section descriptions and body paragraphs at 24px.
- **Text (tertiary):** `rgb(138, 143, 152)` -- medium gray. Used for nav items, sub-descriptions, and supporting copy. This is the workhorse color.
- **Text (quaternary):** `rgb(98, 102, 109)` -- darker gray for hints, labels, timestamps, and low-priority metadata.
- **CSS variable system:** `--color-text-primary`, `--color-text-secondary`, `--color-text-tertiary`, `--color-text-quaternary`, `--color-bg-primary`, `--color-green`, `--color-red`.
- **Accent (brand/primary):** `rgb(94, 106, 210)` -- muted indigo/periwinkle. Used sparingly for the skip-to-content link and status indicators. Not a loud brand color.
- **Accent (status colors, functional only):** `rgb(235, 87, 87)` (red), `rgb(39, 166, 68)` (green), `rgb(16, 185, 129)` (teal), `rgb(6, 182, 212)` (cyan), `rgb(139, 92, 246)` (purple), `rgb(247, 156, 224)` (pink), `rgb(255, 196, 124)` (amber), `rgb(143, 164, 255)` (periwinkle), `rgb(85, 205, 255)` (sky blue), `rgb(137, 209, 150)` (mint). These appear exclusively inside product UI mockups as status/label indicators.
- **CTA/Button fill:** `rgb(230, 230, 230)` -- light warm gray (not white) for primary buttons. Text inside buttons is `rgb(8, 9, 10)` (inverted).
- **Borders:** `rgba(255, 255, 255, 0.05)` to `rgba(255, 255, 255, 0.12)` -- white at 5-12% opacity. Also `rgb(35, 37, 42)` and `rgb(36, 40, 44)` as solid alternatives.
- **Code diff highlights:** Red (`rgba(255, 0, 0, 0.1)`) and green (`rgba(0, 255, 5, 0.07)`) at extremely low opacity for code diff backgrounds.
- **Gradients:** Primarily radial gradients for subtle spotlight effects: `radial-gradient(50% 50%, rgba(255, 255, 255, 0.04) 0px, rgba(255, 255, 255, 0) 90%)`. Edge-fade gradients to blend content into the background: `radial-gradient(52.53% 57.5% at 50% 100%, rgba(8, 9, 10, 0) 0px, rgba(8, 9, 10, 0.5) 100%)`. Dashed grid lines via: `repeating-linear-gradient(to right, rgb(35, 37, 42) 0px, rgb(35, 37, 42) 3px, transparent 3px, transparent 7px)`.

### Typography

- **Font family:** `"Inter Variable"` as primary, with fallback stack: `"SF Pro Display", -apple-system, "system-ui", "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif`. Monospace variant (`--font-monospace`) for code and temporal data.
- **H1 (hero):** 64px, weight 510, letter-spacing -1.408px (-2.2%), line-height 64px (1:1 ratio). Color: `rgb(247, 248, 248)`.
- **H2 (section headings):** 48px, weight 510, color `rgb(138, 143, 152)` (tertiary gray -- deliberately muted; the description text beside them is actually brighter).
- **Section descriptions:** 24px, weight 400, line-height 31.92px (1.33 ratio), letter-spacing -0.288px, color `rgb(208, 214, 224)`.
- **Body text:** 15px, weight 400, line-height 24px (1.6 ratio), letter-spacing -0.165px, color `rgb(138, 143, 152)`. Max-width constrained to 250-368px in multi-column layouts.
- **Nav links:** 13px, weight 400, color `rgb(138, 143, 152)`.
- **CTA buttons:** 13-15px, weight 510, color `rgb(8, 9, 10)` on light background.
- **Product UI text (in mockups):** 14px, line-height 21px, letter-spacing -0.182px. Max-width 550px for issue descriptions.
- **Monospace:** Used for changelog timestamps (`MAR 11, 2026`) and as an intentional accent typography in labels -- code aesthetic elevated to UI components.
- **Font size system:** 9 title sizes (`--title-1-size` through `--title-9-size`) plus 6 body sizes (`--text-large-size`, `--text-regular-size`, `--text-small-size`, `--text-mini-size`, `--text-micro-size`, `--text-tiny-size`). Each size has paired letter-spacing and line-height via CSS custom properties.
- **Font weight system:** Light, Normal (400), Medium, Semibold (510 used as the hero weight -- a variable font custom axis value between 500 and 600).
- **Text wrapping:** `text-wrap: balance` and `text-wrap: pretty` for refined line breaks in headings and body text. `text-overflow: ellipsis` with `-webkit-line-clamp: 2` for truncation.
- **Responsive breakpoints for type:** Font sizes scale down at 1024px and 640px viewports.
- **Key insight -- weight 510:** Inter Variable's custom weight axis allows 510, which sits in a perceptual sweet spot that feels precise and engineered, neither medium nor semibold. This micro-decision compounds across every heading and CTA.

### Layout

- **Overall structure:** Full-width dark canvas. No visible grid lines or gutters. Spacing feels organic and generous.
- **Navigation (sticky header):** Logo left, nav links center-right (13px, 12px horizontal padding each), CTA buttons far right. A 1px vertical divider line separates nav links from "Log in" / "Sign up". Height ~64px.
- **Hero section:** Left-aligned headline occupying ~60% of viewport width. Subtitle below the headline in tertiary gray. A "New" badge + feature announcement link on the right. Below: a full-width product UI mockup showing an issue detail view with sidebar, activity feed, and Cursor agent notification panel.
- **Value proposition strip (3 columns):** "Built for purpose" / "Powered by AI agents" / "Designed for speed" with paragraph descriptions. Each column features an animated isometric grid illustration above the text. Columns use flexbox with even distribution.
- **Feature sections (repeating pattern):** Left 40% contains the section heading (h2, muted). Right 60% contains the description paragraph (brighter) and numbered feature links. Below: full-width product screenshot/mockup. Sections separated by very generous whitespace (200-300px equivalent).
- **Numbered navigation system:** `1.0 Intake`, `2.0 Plan`, `3.0 Build`, `4.0 Diffs (Coming soon)`, `5.0 Monitor` with sub-features in a 2-column grid (`2.1 Projects` / `2.2 Documents` / `2.3 Initiatives` / `2.4 Visual planning`). Arrow indicators for links.
- **Changelog section:** 4-column card layout. Horizontal timeline at top with colored dots (first dot red/active, rest gray). Each card: title, description excerpt, monospaced date. Cards have subtle dark backgrounds.
- **Customer quotes section:** Hidden on laptop breakpoint (class `hide-laptop`), suggesting a testimonials carousel for smaller screens.
- **Footer CTA section:** Large centered text "Built for the future. Available today." with two side-by-side buttons ("Get started" filled warm gray, "Contact sales" ghost/outlined).
- **Footer:** 5-column link grid (Product, Features, Company, Resources, Connect) with logo at far left. Legal links (Privacy, Terms, DPA) below as a final row. Column headers are h3.
- **Responsive breakpoints:** 1280px, 1024px, 768px, 640px. Design shifts significantly at tablet/mobile -- not just shrunk.
- **Spacing philosophy:** Extremely generous vertical spacing between sections. Content density is low, letting each section command full attention during scroll. The page breathes.

### Components

- **Primary button ("Sign up", "Get started", "Open app"):** Background `rgb(230, 230, 230)`, text `rgb(8, 9, 10)`, border-radius 4px, padding 0 12-16px, font-weight 510, font-size 13-15px. Warm gray rather than pure white.
- **Ghost/secondary button ("Contact sales"):** Dark background with visible border `rgba(255, 255, 255, 0.08)`, light text. Same 4px border-radius.
- **Nav links:** No background, 4px border-radius for hover state area, 12px horizontal padding. Color `rgb(138, 143, 152)`.
- **Product UI mockups (hero):** Full-fidelity Linear app screenshot showing issue detail view with 3-panel layout (sidebar navigation, main content area with issue description + activity, and right sidebar with metadata: status, priority, assignee, labels). Includes a Cursor agent notification panel overlaid at bottom-right. Realistic data: "Faster app launch" issue, "ENG-2703", agent examining and working.
- **Product UI mockup (Intake section):** Slack thread integration showing `#feedback` channel, alongside a kanban-style board with "Todo" (71) and "In Progress" (3) columns.
- **Product UI mockup (Plan section):** Timeline/Gantt view with month labels (FEB through SEP), project bars with milestones. Initiatives sidebar listing "Core Product" (99), "APAC Expansion" (21) with sub-items and counts. S-curve progress lines.
- **Product UI mockup (Build section):** Issues list view and agent task assignments.
- **Product UI mockup (Diffs section):** Side-by-side code diff view with syntax highlighting, line numbers, and red/green diff backgrounds. File path header: `kinetic-ios/src/screens/Home/HomeScreen.tsx`.
- **Product UI mockup (Monitor section):** Analytics dashboard with project update cards showing progress descriptions, risk flags, and status indicators.
- **Animated grid dots (hero decorative):** 5x5 grid of dots with staggered opacity animations. Each of 25 dots has a unique keyframe (`grid-dot-0-0-agent` through `grid-dot-4-4-agent`). Uses `steps(1, end)` for discrete, tick-like illumination. Cascade moves left-to-right, top-to-bottom. 3200ms cycle. Alternative "upDown" variant at 2800ms with vertical sweep.
- **Isometric grid illustrations:** 3D wireframe grid shapes used for the "Built for purpose" / "Powered by AI agents" / "Designed for speed" section -- subtle geometric/technical aesthetic.
- **Changelog timeline:** Horizontal line connecting colored dots. First dot is red/active, subsequent dots are smaller and gray. Cards below with title, excerpt, and monospace date.
- **Sidebar UI (in mockups):** Dark sidebar `rgb(16, 17, 18)` with icons at 14px in circular containers (`aspect-ratio: 1/1`, `border-radius: 50%`, `object-fit: cover`). Items: Inbox, My issues, Reviews, Pulse, Workspace section with Initiatives/Projects/More, Favorites.
- **Status badges:** Small colored dots paired with text labels -- "In Progress" (green), "High" priority with bar-chart icon.
- **"New" announcement badge:** Appears in hero section near feature link "Linear Diffs (Beta)" with right arrow.
- **Border-radius scale:** 2px (tiny), 4px (buttons/nav -- the default), 6px (small cards), 8px (medium cards), 12px (large containers/cards), 16px (hero mockup), 400px/9999px (pills).
- **Box shadows:** Extremely subtle. Card shadow: `rgba(0, 0, 0, 0.08) 0px 0px 1px, rgba(0, 0, 0, 0.07) 0px 1px 1px, rgba(0, 0, 0, 0.04) 0px 3px 2px`. Inset depth: `rgba(0, 0, 0, 0.2) 0px 0px 12px inset`. Mockup float: `rgba(8, 9, 10, 0.1) 0px 0px 0px 1px, rgba(8, 9, 10, 0.4) 0px 0px 64px`. Large blur: `rgba(8, 9, 10, 0.6) 0px 4px 32px`.

### Motion

- **Grid dot cascade animation:** 5x5 animated dot grid in hero area. Each of 25 dots has a unique keyframe animation (`grid-dot-X-Y-agent`). Timing: 3200ms infinite loop using `steps(1, end)` -- mechanical, binary on/off illumination. Cascade pattern: Row 0 at 0-12.5%, Row 1 at 18-37.5%, Row 2 at 43-62.5%, continuing downward. Alternate "upDown" variant at 2800ms with vertical sweep.
- **`steps(1, end)` as signature design choice:** Where most SaaS sites use smooth easing, Linear uses stepped/discrete transitions that feel precise, intentional, mechanical -- like a circuit board or digital signal. Reinforces brand identity of speed and precision. Animation serves narrative (intelligence, processing) not decoration.
- **Gradient hover/spotlight effects:** Subtle radial gradients (`rgba(255, 255, 255, 0.03)` to `rgba(255, 255, 255, 0.04)`) on interactive elements for a light-follow-cursor effect.
- **Scroll-triggered section reveals:** Feature sections use intersection-observer-based reveals (framework-driven, React/Next.js).
- **Animation cycle lengths:** Long, non-distracting: 2800-3200ms. Deliberate avoidance of short, attention-grabbing loops.
- **No excessive motion:** No parallax scrolling, no bouncing elements, no scroll-jacking. Radical restraint. Motion exists only to communicate state.

## What Makes It Work

- **Inverted hierarchy of brightness:** H2 section headings are rendered in tertiary gray (`rgb(138, 143, 152)`) while description paragraphs beside them are brighter (`rgb(208, 214, 224)`). This is counterintuitive but brilliant -- users scan section topics quickly via muted headings while brighter descriptions pull them in for detail. The hierarchy is semantic (heading vs. body) but the visual weight is reversed.

- **The product IS the marketing:** Rather than abstract illustrations or stock photography, every section showcases pixel-perfect, full-fidelity screenshots of the actual Linear app. The hero immediately drops you into an issue detail view with a live Cursor agent notification. The "Plan" section shows a real timeline with Initiatives. The "Diffs" section shows real TypeScript code diffs. This builds credibility with the developer audience and eliminates the gap between marketing promise and product reality.

- **Radical restraint in color:** The entire page operates on a monochromatic gray scale with white-at-low-opacity as the accent system. No hero gradient, no brand color splash, no colorful CTAs. The only color appears functionally inside product mockups (status indicators, syntax highlighting, diff markers). This anti-pattern signals seriousness and tools-for-craftspeople energy, and lets product screenshots -- which do contain color -- become the visual focal points.

- **Numbering as information architecture:** `1.0 Intake > 2.0 Plan > 3.0 Build > 4.0 Diffs > 5.0 Monitor` with sub-features (`2.1 Projects`, `2.2 Documents`) transforms a typical features page into a guided product development lifecycle walkthrough. It creates a mental model of the product's scope and a sense of comprehensive coverage.

- **Weight 510 as brand signature:** Inter Variable's custom weight axis allows 510 -- a weight outside traditional font-weight systems (400/500/600). It feels precise and engineered. This micro-decision compounds across every heading and CTA to create a distinctive typographic feel that no competing tool can replicate by just picking "font-weight: 500" or "font-weight: 600."

- **Near-black vs. pure black:** `rgb(8, 9, 10)` background avoids the harshness of `#000000`. Combined with `rgb(247, 248, 248)` text (not `#FFFFFF`), contrast is high but never jarring. This 98% dark approach creates depth -- surfaces can step up to `rgb(15, 16, 17)` and still register as distinct layers.

- **Monospace as accent typography:** Monospace appears not just in code blocks but as intentional accent typography for timestamps, labels, and micro-interactions. This elevates the code aesthetic to a UI design element, reinforcing developer-first positioning without becoming a "code editor" theme.

## Reusable Patterns

- **4-tier text contrast system:** Primary (`rgb(247, 248, 248)`) / Secondary (`rgb(208, 214, 224)`) / Tertiary (`rgb(138, 143, 152)`) / Quaternary (`rgb(98, 102, 109)`) on dark backgrounds. Replaces color-based hierarchy with a luminance-only system. Highly accessible, consistent, and composable.

- **White-at-low-opacity surface layering:** `rgba(255, 255, 255, 0.02)` through `rgba(255, 255, 255, 0.08)` on a dark base. Surfaces stack and compose without a rigid token system. Borders follow the same pattern at `rgba(255, 255, 255, 0.05)` to `rgba(255, 255, 255, 0.12)`.

- **CSS variable-driven type scale:** 9 title sizes + 6 body variants, each with paired letter-spacing and line-height. Weights mapped to specific tiers. Excellent system for design tokens.

- **Stepped animation for precision feel:** `steps(1, end)` on timed animations creates a mechanical, digital quality that reads as "engineered" rather than "designed." Ideal for developer tools and technical products.

- **Numbered feature walkthrough:** Structure marketing sections as `X.0 Category > X.1 Feature > X.2 Feature` with arrows. Gives users a mental model of product scope before signup. Works for complex products with multiple feature areas.

- **Split heading/description layout:** H2 on the left (40%, muted color), description paragraph on the right (60%, brighter color), full-width visual below. Each section functions as an independent storytelling unit.

- **Product screenshots as hero content:** Skip abstract illustrations. Show the actual product at full fidelity with realistic data, including live-feeling details (agent notifications, timestamps, real code) to make it feel active and trustworthy.

- **Warm gray primary buttons:** `rgb(230, 230, 230)` (not pure white) is softer and more sophisticated. Ghost buttons with subtle borders for secondary actions. 4px border-radius is tight and precise -- no pill shapes for primary actions.

- **Monospace for temporal/tabular data:** Changelog dates, version numbers, and counts in monospace for natural alignment and a technical aesthetic.

- **Subtle radial gradient spotlights:** `radial-gradient(50% 50%, rgba(255, 255, 255, 0.04), rgba(255, 255, 255, 0) 90%)` adds barely-perceptible glow to cards/sections without being visible as a distinct element.

- **Border-radius scale (2/4/6/8/12/16px):** 6-step system mapping to element importance. 4px default for interactive elements. Larger containers get 12-16px. Avoids the over-rounded aesthetic common in modern SaaS.

- **`text-wrap: balance` for headings:** Ensures organic, balanced line breaks in multi-line headings without manual `<br>` tags.

- **Opacity-based state hierarchy:** Use opacity/transparency levels (not color shifts) for hover, active, disabled, and layering states. Creates sophisticated depth with minimal palette management.
