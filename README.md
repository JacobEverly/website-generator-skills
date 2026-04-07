# Website Generator Skills for Claude Code

A complete skill suite for generating production-grade websites with Claude Code. Includes a 6-skill pipeline, 18 design refinement skills, and a curated design library.

## Pipeline

```
capture-inspo → design-brief → site-gen → site-loop → site-refine
                                                    ↘ site-images
```

| Skill | Purpose |
|-------|---------|
| `capture-inspo` | Capture design inspiration from URLs, screenshots, or descriptions |
| `design-brief` | Generate a structured DESIGN.md from vibes, goals, and references |
| `site-gen` | Generate a single production-grade page from DESIGN.md |
| `site-loop` | Generate a complete multi-page website with shared components |
| `site-refine` | Autonomous refinement loop with quality gates |
| `site-images` | Generate custom images using Gemini image generation |

## Design Refinement Skills

These skills can be applied at any stage to refine the output:

| Skill | Purpose |
|-------|---------|
| `adapt` | Responsive design across devices |
| `animate` | Purposeful animations and micro-interactions |
| `audit` | Comprehensive quality audit |
| `bolder` | Amplify safe designs to be more visually striking |
| `clarify` | Improve UX copy and microcopy |
| `colorize` | Add strategic color to monochromatic designs |
| `critique` | UX evaluation with actionable feedback |
| `delight` | Add moments of joy and personality |
| `distill` | Strip to essence, remove complexity |
| `extract` | Extract reusable components and patterns |
| `frontend-design` | Create distinctive, production-grade interfaces |
| `harden` | Error handling, i18n, edge cases |
| `normalize` | Match design system consistency |
| `onboard` | Design onboarding flows and empty states |
| `optimize` | Performance across loading, rendering, bundle size |
| `polish` | Final quality pass before shipping |
| `quieter` | Tone down overly aggressive designs |
| `teach-impeccable` | One-time setup for persistent design guidelines |

## Design Library

The `design-library/` directory contains:

- **Base theme** — shared aesthetic foundation
- **References** — animation stack, typography, agent-friendly patterns
- **DESIGN.md examples** — minimal SaaS, bold marketing, dark developer tool
- **Inspirations** — Linear, Vercel, Stripe, Resend, Raycast
- **Patterns** — reusable UI patterns (comparison bars, canvas backgrounds, etc.)

## Installation

Copy skills and design library to your Claude Code config:

```bash
cp -r skills/* ~/.claude/skills/
cp -r design-library ~/.claude/design-library/
```

Then invoke with `/design-brief`, `/site-gen`, `/site-loop`, etc.
