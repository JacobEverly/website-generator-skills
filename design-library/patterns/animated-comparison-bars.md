# Pattern: Animated Comparison Bars

Scroll-triggered proportional bars with count-up numbers. Extremely effective for "us vs them" comparisons where the size difference IS the argument.

## When to Use

Any time you're comparing two numbers where one dwarfs the other (350 vs 500,000, $50K vs $0.01, 3 features vs 50 features). The visual contrast does the convincing — no explanation needed.

## Implementation

```tsx
"use client";
import { useEffect, useRef, useState } from "react";

function useCountUp(target: number, active: boolean, duration = 1200) {
  const [value, setValue] = useState(0);
  useEffect(() => {
    if (!active) return;
    const start = performance.now();
    function tick(now: number) {
      const elapsed = now - start;
      const progress = Math.min(elapsed / duration, 1);
      const eased = progress === 1 ? 1 : 1 - Math.pow(2, -10 * progress); // expo-out
      setValue(Math.round(eased * target));
      if (progress < 1) requestAnimationFrame(tick);
    }
    requestAnimationFrame(tick);
  }, [active, target, duration]);
  return value;
}
```

## Key Details

- **IntersectionObserver** triggers at threshold 0.4 (visible enough to notice)
- **Count-up** uses expo-out easing for satisfying deceleration
- **Bar widths** animate via CSS transition: `transition: width 1.2s cubic-bezier(0.16, 1, 0.3, 1)`
- **Small bar** should be proportionally correct (e.g., 0.07% for 350/500,000) with a min-width of 4px so it's visible
- **Large bar** grows to 100% width, delayed 150ms for dramatic sequence
- **Ratio callout** fades in after bars complete: "1,400× more coverage"
- **Bar height difference** reinforces the message: small bar at h-2, large bar at h-4
- Respects `prefers-reduced-motion` — shows final state immediately

## Reference: AnyFeed Problem Section

Chainlink (350 feeds) gets a barely-visible sliver. AnyFeed (500,000+ pairs) fills the entire width. Numbers count up simultaneously. "1,400× more coverage" fades in after.
