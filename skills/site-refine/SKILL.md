---
name: site-refine
description: Autonomous refinement loop for generated websites. Two quality gates (screenshot-worthy + mobile-fast), dispatches specialized skills, loops until both pass. Max 3 iterations.
user-invokable: true
args:
  - name: mode
    description: "full" (default) runs the complete loop. "polish" runs only the final polish pass. "critique" runs only the critique without fixes.
    required: false
---

Autonomous refinement loop that evaluates a generated website against two quality gates and dispatches specialized skills until both pass. Runs without human intervention after DESIGN.md approval.

## Overview

```
┌─────────────────────────────────────────────┐
│              CRITIQUE PHASE                  │
│  Read all pages → evaluate against gates     │
└──────────────┬──────────────────────────────┘
               │
       ┌───────▼───────┐
       │  Both pass?    │──── YES ──→ Run /polish → DONE
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

### Step 4: Score and Decide

For each gate, assign: **PASS** or **FAIL** with specific reasons.

**If both PASS** → Skip to Step 6 (Polish).

**If either FAILS** → Proceed to Step 5 (Fix), up to 3 total iterations.

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
3. Confirm DESIGN.md conformance

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
