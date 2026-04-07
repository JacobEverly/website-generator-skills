# Pattern: Internal Dashboard

Lessons from the Boundless Data Dashboard redesign. Internal dashboards have fundamentally different needs from marketing sites.

## How Dashboards Differ from Landing Pages

| Dimension | Marketing Site | Internal Dashboard |
|-----------|---------------|-------------------|
| Spacing | Generous (80-120px) | Tight (16-24px) |
| Animation | Entrance reveals, scroll effects | None — data appears instantly |
| Typography | Display fonts, large heroes | System fonts, dense tables |
| Layout | Full-width sections, asymmetric | Sidebar nav + content, grid metrics |
| Color | Neutral chrome, color through imagery | Status colors are functional (green/yellow/red) |
| Images | Hero images, screenshots | Charts, sparklines, status indicators |
| Priority | Visual impact, conversion | Scan speed, drill-down efficiency |

## Dashboard Layout Pattern

```
┌──────────┬─────────────────────────────────────┐
│          │  Page Title        Period Toggle     │
│  Sidebar │─────────────────────────────────────│
│  240px   │  Hero Metrics (3-4 cards)           │
│  (64px   │─────────────────────────────────────│
│  when    │  Main Content                        │
│  collapsed)│  (tables, charts, sections)        │
└──────────┴─────────────────────────────────────┘
```

- Sidebar nav (not top tabs) — vertical, collapsible, icons + labels
- Page content max-width: 1200px
- Section gap: 24px (not 80-120px)
- Card padding: 20px

## Table Headers — Keep Them Quiet

The biggest visual mistake in dashboard redesign: over-styling table headers.

**Don't:**
- Uppercase + tracking-wider + bold + dotted underlines + large font (looks clunky)
- 12px+ with heavy formatting

**Do:**
- 11px, regular weight, no uppercase, no dotted underlines
- `whitespace-nowrap` to prevent wrapping
- Sort arrows as the only interactive affordance
- Muted color (zinc-500), no borders

```tsx
// Good table header
<th className="text-right px-3 py-2 text-[11px] text-zinc-500 font-medium whitespace-nowrap">
  Accept (7d) ↕
</th>
```

## Metric Cards

- Background: one step above page bg (zinc-900 on zinc-950)
- No borders, no shadows
- Large mono value (text-2xl font-mono)
- Small uppercase label above (text-[11px] text-zinc-500)
- Status dot top-right
- Trend arrow + delta inline with value

## Status Color System

Immutable for any dashboard with health monitoring:
```css
--status-green:   #34d399;  /* emerald-400 — healthy */
--status-yellow:  #fbbf24;  /* amber-400 — warning */
--status-red:     #f87171;  /* red-400 — critical */
--status-neutral: #71717a;  /* zinc-500 — no data */
```

Status colors appear as:
- 6px dot indicators (w-1.5 h-1.5)
- 5% opacity background tints (bg-emerald-500/5)
- 3px left borders on alert cards

Never as text color — readability suffers on dark backgrounds.

## Progressive Disclosure for Alerts

Long alert/flag lists should ALWAYS collapse into a summary by default.

```tsx
// Summary bar (collapsed) — shows count + severity
"17 Critical Issues, 4 Warnings" [chevron ▼]

// Expanded — shows individual items
[click to expand full list]
```

This prevents alert fatigue and keeps the page scannable.

## Loading Skeletons

Every dashboard page should have a `loading.tsx` with:
- Skeleton metric cards (3-4 in a row)
- Skeleton table (header + 5-6 rows)
- `animate-pulse` on zinc-800 shapes

This makes pages feel instant even during cold API fetches.

## Font-Mono for All Data

Every numeric value, address, percentage, and timestamp should use `font-mono`:
- Table cells with numbers
- Metric card values
- Address displays (0x...)
- Timestamps and dates
- Threshold values

This ensures tabular alignment and signals "this is data, not prose."

## Dark Mode as Default

Internal dashboards run on monitors all day. Dark mode reduces eye strain and makes status colors pop. Light mode available via toggle but dark is primary.

## Data Logic Lessons

Better design exposes bad data. When you redesign a dashboard:
1. **Check aggregation logic** — are averages weighted correctly? (cycle-weighted vs equal-weight)
2. **Check denominator bugs** — expiration rates can exceed 100% if the denominator is wrong
3. **Filter stale data** — orders from prior periods expiring in the current period distort metrics
4. **Exclude inactive entities** — requestors with 0 activity in the period shouldn't pull down averages
