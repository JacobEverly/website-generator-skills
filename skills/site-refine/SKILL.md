---
name: site-refine
description: Autonomous refinement loop for generated websites. Three quality gates (screenshot-worthy + mobile-fast + animation-renders-correctly), dispatches specialized skills, loops until all pass. Max 3 iterations.
user-invokable: true
args:
  - name: mode
    description: "full" (default) runs the complete loop. "polish" runs only the final polish pass. "critique" runs only the critique without fixes.
    required: false
---

Autonomous refinement loop that evaluates a generated website against three quality gates and dispatches specialized skills until all pass. Runs without human intervention after DESIGN.md approval.

## Overview

```
┌─────────────────────────────────────────────┐
│              CRITIQUE PHASE                  │
│  Gate 1 — "Would I screenshot this?"         │
│  Gate 2 — "Does it load fast on mobile?"     │
│  Gate 3 — "Do animations render correctly?"  │
│           (runs /animation-check in browser) │
└──────────────┬──────────────────────────────┘
               │
       ┌───────▼───────┐
       │  All 3 pass?   │──── YES ──→ Run /polish → DONE
       └───────┬───────┘
               │ NO
       ┌───────▼───────┐
       │ Iteration < 3? │──── NO ──→ Run /polish → DONE (with warnings)
       └───────┬───────┘
               │ YES
       ┌───────▼──────────────────────────────┐
       │           FIX PHASE                   │
       │  Identify top 3 issues                │
       │  Dispatch matching skills             │
       │  Apply fixes                          │
       └───────┬──────────────────────────────┘
               │
               └──→ Back to CRITIQUE PHASE
```

## Mode: Full (default)

### Step 1: Load Context

1. Read the project's `DESIGN.md`
2. Read the base theme: `~/.claude/design-library/boundless-base.md`
3. Read all page files and components in the project
4. Note the DESIGN.md's Hard Rules

### Step 2: Critique — Gate 1: "Would I Screenshot This?"

This gate tests visual distinctiveness and personality. Ask yourself:

**Is this visually distinctive?**
- Does it have a clear aesthetic point of view?
- Would a designer be proud to show this in their portfolio?
- Is there at least ONE moment that surprises or delights?
- Does it feel like a real company's site, not a template?

**Does it have personality?**
- Does the display font choice give it character?
- Is the motion style adding to the experience?
- Are there any unique visual elements that break from the expected?
- Does the copy voice feel human and specific, not generic?

**Anti-slop auto-fail triggers** — if ANY of these are detected, the gate fails immediately:

Scan all generated files for these patterns:
1. **Card borders or card shadows** — grep for `border` on card-like divs, `shadow-` on content cards
2. **Gradient text** — grep for `bg-gradient` + `bg-clip-text` + `text-transparent`
3. **Colored headings** — headings using color classes other than neutral/base
4. **Equal 50/50 columns** — `grid-cols-2` without custom template, `w-1/2` paired layouts
5. **Identical card grid** — 3+ cards with same classes/structure in a row
6. **Generic centered hero with gradient bg** — centered text + gradient background in first section
7. **Cyan-on-dark / neon accents** — cyan, lime, neon color values
8. **Colored primary buttons** — primary buttons with non-neutral background (should be near-black)
9. **Bounce/elastic animations** — `animate-bounce`, `animate-pulse`, elastic easing
10. **"Trusted by" labels** — text above logo bars

### Step 3: Critique — Gate 2: "Does It Load Fast on Mobile?"

This gate tests production readiness:

- **Mobile-first?** — Are there responsive classes? Does single-column come first?
- **Performance** — No unnecessary animations on mobile, images use next/image, no layout shift
- **Zero CLS on text** — Hero sections use `useTextLayout` hook (Pretext) to pre-calculate text height, preventing layout shift during entrance animations. If a hero has animated text but no `useTextLayout` or equivalent `minHeight` reservation, this gate fails.
- **Touch targets** — Buttons/links at least 44x44px
- **Text readability** — Body text 16px+, adequate line-height, sufficient contrast
- **No horizontal scroll** — Nothing breaks out of viewport on mobile
- **Fast load** — No heavy dependencies, no blocking animations, semantic HTML for fast parse

### Step 3.5: Critique — Gate 3: "Do Animations Render Correctly?"

This gate tests that animations actually work when they run — not just that the code looks well-formed. Invoke the `/animation-check` skill, which uses Playwright MCP to:

1. Launch (or connect to) the dev server
2. Navigate at 375px, 768px, and 1440px viewports
3. Wait for fonts, hero entrance, and scroll-triggered animations to settle
4. Scroll through the page so scroll-triggered animations fire
5. Run programmatic checks: CLS, horizontal overflow, sibling overlap, clipped text, lingering non-ambient animations, elements stuck mid-animation, console errors
6. Capture full-page screenshots at each viewport
7. Visually evaluate each screenshot for typography fit, alignment, empty-space gaps, overlap, CTA visibility

**Preconditions:**
- Dev server must be running. If not, instruct the user to `npm run dev` and pause the loop.
- Playwright MCP must be available. If not, skip Gate 3 with a warning — do not fail the whole refine loop.

**Gate 3 passes when:**
- CLS < 0.05 at all viewports
- No horizontal overflow at any viewport
- No sibling overlap > 5px
- No elements stuck at `opacity < 1` or with non-identity transforms after the settle window
- No non-ambient animations still running after settle
- Screenshots show no clipped headlines, no empty hero space, no touch targets < 44px on mobile

**Gate 3 fails when any FAIL-severity finding is reported.** WARN-severity findings are tracked but don't block the gate on iteration 1; they're promoted to FAIL on iteration 2+.

Artifacts from `/animation-check` are saved to `.animation-check/` (screenshots + report.json + report.md). The refine loop reads `report.json` to drive Step 5.

### Step 4: Score and Decide

For each gate, assign: **PASS** or **FAIL** with specific reasons.

**If all three PASS** → Skip to Step 6 (Polish).

**If any FAILS** → Proceed to Step 5 (Fix), up to 3 total iterations.

**If max iterations reached** → Proceed to Step 6 (Polish) with warnings about remaining issues.

### Step 5: Fix — Dispatch Skills

Based on which gate(s) failed, identify the **top 3 issues** and dispatch the matching skill:

#### Screenshot Gate Failures → Personality Skills
| Issue | Skill to Run | What It Fixes |
|-------|-------------|---------------|
| Too safe/generic | `/bolder` | Amplifies visual impact |
| Too complex/cluttered | `/distill` | Strips to essence |
| No moments of joy | `/delight` | Adds memorable touches |
| Too monochromatic | `/colorize` | Adds strategic color |
| Drifted from DESIGN.md | `/normalize` | Realigns to design system |
| Copy is generic/unclear | `/clarify` | Improves UX copy |
| Too visually aggressive | `/quieter` | Tones down intensity |

#### Mobile Gate Failures → Production Skills
| Issue | Skill to Run | What It Fixes |
|-------|-------------|---------------|
| Not responsive | `/adapt` | Fixes responsive design |
| Slow/heavy | `/optimize` | Improves performance |
| Edge case fragility | `/harden` | Robustness improvements |

#### Animation Gate Failures → Rendering Skills / Direct Edits
Read `.animation-check/report.json` and map each FAIL entry:

| Finding | Action |
|---------|--------|
| CLS > 0.05 on hero | `/harden` → add `useTextLayout` hook and `minHeight` on hero text container |
| CLS > 0.05 on mid-page | `/harden` → reserve height for animated elements (e.g. counters, drawables) |
| Horizontal overflow | `/adapt` → mobile responsive fix (usually a marquee width or fixed-size image) |
| Sibling section overlap | `/polish` → add section vertical padding / section margin |
| Element stuck at `opacity: 0` | Direct edit the offending component. Most common causes: missing `'use client'` directive, `createScope` not set up correctly, `useEffect` runs before refs attach, `onScroll` trigger never fires because container has `overflow: hidden` |
| Element stuck at non-identity transform | Direct edit — anime.js block never completed. Check that `animate()` has a defined `to` value, that `createScope(...).revert()` isn't firing mid-animation from a React re-render |
| Lingering non-ambient animation | Direct edit — add `once: true` to the `onScroll` trigger, or remove the `loop: true` if unintentional |
| Clipped headline | `/adapt` → add `useAdaptiveHeadline` hook or reduce hero font-size at the failing breakpoint |
| Touch target < 44px | `/harden` → enforce min-size on buttons/links |
| Console error from anime.js/motion/gsap | Direct edit the file referenced in the error |

#### How to Dispatch
For each issue:
1. Identify the specific files/components affected
2. Run the matching skill targeting those files
3. After the skill completes, verify the fix didn't break other things
4. Move to the next issue

After all 3 fixes are applied, loop back to Step 2 (Critique).

### Step 6: Polish — Final Pass

Always run the `/polish` skill as the final step, even if both gates passed on the first try. Polish catches:
- Alignment inconsistencies
- Spacing irregularities
- Typography detail issues (tracking, line-height, weight)
- Color consistency
- Small accessibility gaps

### Step 7: Final Validation

After polish:
1. `npx tsc --noEmit` — must pass
2. One last anti-slop scan
3. One last `/animation-check` pass — `/polish` sometimes adjusts spacing in ways that reintroduce overlap or CLS. Re-verify before declaring done.
4. Confirm DESIGN.md conformance

### Step 8: Output

Report:
- **Iterations**: How many refinement loops ran
- **Gate results**: Final pass/fail status for each gate
- **Skills dispatched**: Which skills were run and what they fixed
- **Remaining issues**: Anything that couldn't be resolved (if max iterations hit)
- **Files modified**: List of all changed files

---

## Mode: Polish

Runs only Step 6 (Polish) — the final quality pass. Use when the site is already good but needs detail refinement.

## Mode: Critique

Runs only Steps 2-4 (Critique + Scoring) — evaluates and reports but doesn't fix anything. Use to assess quality before deciding what to do manually.
