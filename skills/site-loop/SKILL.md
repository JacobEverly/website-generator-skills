---
name: site-loop
description: Generate a complete multi-page website from DESIGN.md. Builds shared components first, then pages sequentially, maintaining design system consistency throughout.
user-invokable: true
args:
  - name: sitemap
    description: Comma-separated list of pages (e.g. "home, about, pricing, docs"). If omitted, generates from design brief.
    required: false
  - name: design-md
    description: Path to DESIGN.md (defaults to ./DESIGN.md)
    required: false
---

Generate a complete multi-page website, maintaining DESIGN.md consistency across all pages. Builds shared components first, then individual pages sequentially.

## Step 1: Load Design Context

1. Read the project's `DESIGN.md` (from `design-md` arg or `./DESIGN.md`)
2. Read the base theme: `~/.claude/design-library/boundless-base.md`
3. Read the tech UI reference: `~/.claude/design-library/references/techui.md`
4. Read the Pretext reference: `~/.claude/design-library/references/pretext.md`
5. Read any project CLAUDE.md for additional design context

If no DESIGN.md exists, STOP and tell the user to run `/design-brief` first.

## Step 2: Define the Site Map

If `sitemap` is provided, use it. Otherwise, derive from DESIGN.md's Section Plan.

For each page, define:
- **Route**: URL path (e.g., `/`, `/about`, `/pricing`)
- **Purpose**: What the page does in one sentence
- **Key sections**: What content blocks it needs
- **Shared components**: Which shared components it uses (nav, footer, etc.)

Create a task list to track progress.

## Step 3: Build Shared Components First

Before any pages, generate the components used across multiple pages:

### Required Shared Components
1. **Layout** (`app/layout.tsx`) — root layout with font imports, theme provider, metadata
2. **Navigation** (`components/nav.tsx`) — sticky nav following DESIGN.md, mobile menu
3. **Footer** (`components/footer.tsx`) — site footer with links, consistent with DESIGN.md
4. **Theme Provider** (`components/theme-provider.tsx`) — if dark mode is in DESIGN.md
5. **Text Layout Hook** (`hooks/useTextLayout.ts`) — Pretext-based text measurement for CLS-free hero animations (see `/site-gen` for implementation)
6. **OG Image Script** (`scripts/generate-og.ts`) — Canvas + Pretext OG image generator (see `/site-gen` for implementation)

### Optional Shared Components (if multiple pages use them)
- CTA section (reusable call-to-action block)
- Logo bar (customer/partner logos)
- Section wrapper (consistent spacing/max-width)
- Button variants (primary, secondary, ghost, text)

### Shared Component Rules
- Each component gets its own file in `components/`
- Components accept props for content variation but enforce DESIGN.md styling
- Navigation includes links to all pages in the sitemap
- All components follow the same code rules as `/site-gen` (no dynamic Tailwind, accessibility, etc.)

## Step 4: Generate Pages Sequentially

Generate pages one at a time, in this order:
1. **Home page** first (sets the visual standard)
2. **Most content-heavy page** second (tests the design system under stress)
3. **Remaining pages** in sitemap order

For each page:

### 4a. Generate with /site-gen Rules
Apply ALL the same rules from the `/site-gen` skill:
- Prompt enhancement (map sections to DESIGN.md tokens)
- Anti-slop guardrails
- Tailwind literal strings only
- Mobile-first responsive
- Semantic HTML + accessibility
- Proper TypeScript

### 4b. Consistency Check After Each Page
After generating each page, verify:
- **Typography**: Same font sizes/weights for equivalent elements across pages
- **Colors**: All colors match DESIGN.md tokens (no ad-hoc hex values)
- **Spacing**: Section gaps, padding match base theme values
- **Components**: Shared components render identically
- **Motion**: Animation timing/easing matches DESIGN.md Motion Style
- **Voice**: Copy tone matches DESIGN.md Copy Voice

If drift is detected, fix it before moving to the next page.

### 4c. Run Lightweight Audit
After each page, mentally run the `audit` skill checklist:
- Accessibility: semantic HTML, aria attributes, focus states
- Responsive: mobile-first, no hidden critical content
- Performance: no unnecessary re-renders, images optimized
- Theme: DESIGN.md conformance

Flag issues but don't block — fix at the end.

## Step 5: Wire Up Navigation

After all pages are generated:
1. Verify all navigation links work (correct `href` paths)
2. Add active state styling to current page in nav
3. Ensure mobile menu includes all pages
4. Add any cross-page CTAs (e.g., "Learn more" links between pages)

## Step 6: Final Validation

Run across the entire site:

1. **TypeScript**: `npx tsc --noEmit` — must pass with zero errors
2. **Anti-slop scan**: Check all pages for banned patterns (same as `/site-gen`)
3. **Cross-page consistency**: Verify shared components render the same everywhere
4. **Route check**: Every page in sitemap has a corresponding route file
5. **Navigation check**: Every page is accessible from the nav

## Step 7: Output

Present the complete site with:
- File tree of everything created
- Per-page summary (route, sections, key components)
- Any consistency issues found and how they were resolved
- Reminder that `/site-refine` can improve quality autonomously
