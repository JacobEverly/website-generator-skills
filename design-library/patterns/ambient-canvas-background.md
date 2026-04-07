# Pattern: Ambient Canvas Background

A canvas-based background animation with floating data elements. Adds personality without distraction. Used in hero sections to communicate "live data" or "active system."

## When to Use

- Price feeds, trading, financial products → floating prices
- Infrastructure/API products → floating endpoint paths or status codes
- Developer tools → floating code snippets or terminal commands
- Any product where ambient data movement reinforces the value prop

## Implementation

```tsx
"use client";
import { useEffect, useRef } from "react";

const DATA_ITEMS = [/* array of strings to display */];

interface Item { x: number; y: number; speed: number; text: string; opacity: number; size: number; }

export default function AmbientBackground() {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  // ... init items, requestAnimationFrame draw loop
  // Each item drifts upward, resets to bottom when off-screen
  // Edge gradient mask fades items at top/bottom

  return (
    <canvas
      ref={canvasRef}
      className="absolute inset-0 w-full h-full pointer-events-none"
      style={{
        maskImage: "linear-gradient(to bottom, transparent 0%, black 15%, black 85%, transparent 100%)",
        WebkitMaskImage: "linear-gradient(to bottom, transparent 0%, black 15%, black 85%, transparent 100%)",
      }}
      aria-hidden="true"
    />
  );
}
```

## Key Details

- **Canvas 2D** for performance (not DOM elements)
- **2x resolution**: `canvas.width = canvas.offsetWidth * 2` for retina
- **Opacity range**: 5-12% — visible but not distracting
- **Speed**: Very slow (0.006-0.018 per frame) — ambient, not frenetic
- **Edge fading**: CSS mask-image gradient dissolves items at top/bottom edges
- **Color coding**: Bold weight for labels (neutral), regular weight for values (green/red tinting)
- **prefers-reduced-motion**: Skip animation entirely, return empty canvas
- **20-30 items** is the sweet spot — enough to fill the space, not so many it's noisy

## Reference: AnyFeed PriceTicker

Token symbols (ETH, UNI, LINK) in bold + prices ($1,847.23) in regular weight. Green tint for up, red for down. Drift slowly upward. Gradient mask fades at edges.
