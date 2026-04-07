---
name: site-images
description: Generate custom images for websites using Gemini (nanobanana). Heroes, illustrations, icons, OG images — all aligned to DESIGN.md style. Maintains a project image library.
user-invokable: true
args:
  - name: type
    description: "hero", "illustration", "icon", "og", or "all" — what kind of images to generate
    required: true
  - name: prompt
    description: Description of what to generate. If omitted, derives from DESIGN.md.
    required: false
  - name: design-md
    description: Path to DESIGN.md (defaults to ./DESIGN.md)
    required: false
---

Generate custom images for a website project using Gemini image generation (nanobanana), aligned to the project's DESIGN.md style.

## Prerequisites

Nanobanana must be set up. If not configured, direct the user to:
https://ccforpms.com/nano-banana/setup

Nanobanana uses the Gemini API for image generation. Ensure `GEMINI_API_KEY` is set in the environment or in `.env`.

## Step 1: Load Design Context

1. Read the project's `DESIGN.md`
2. Note the **Image Direction** section specifically
3. Note the **Accent Palette** and **Personality Layer**
4. Read the base theme for color defaults

## Step 2: Build the Image Brief

Based on DESIGN.md + the user's request, create a structured prompt for each image:

### For Hero Images
- **Style**: Match DESIGN.md's overall aesthetic (editorial? abstract? photographic?)
- **Colors**: Use accent palette colors, never UI chrome colors
- **Mood**: Match the Vibe section from design brief
- **Composition**: Leave space for text overlay if needed (typically left or center)
- **Format**: 16:9 or 3:2 landscape, min 1920px wide
- **Do NOT**: Include text in the image, use stock photo clichés, use generic tech imagery

### For Illustrations
- **Style**: Consistent with hero but can be more abstract/simplified
- **Colors**: Derived from accent palette
- **Size**: Match the section they'll appear in
- **Purpose**: Reinforce the section's message, not just decorate

### For Icons
- **Style**: Line, solid, or duo-tone — match DESIGN.md specification
- **Colors**: Neutral or accent, never chrome colors
- **Size**: SVG preferred, design at 24x24 or 32x32 base
- **Consistency**: All icons in a set must share the same style

### For OG Images (Open Graph / Social Cards)
- **Layout**: Project name + tagline + brand visual
- **Size**: 1200x630px (standard OG)
- **Style**: Simplified version of hero aesthetic
- **Text**: Include project name and tagline (these CAN have text)
- **Background**: Use accent palette or imagery style from DESIGN.md

## Step 3: Generate Images

Use Gemini image generation to create each image:

### Prompt Engineering for Gemini
Structure prompts as:
```
{style description}, {subject/content}, {color palette}, {mood/atmosphere}, {composition notes}, {technical specs}
```

Example:
```
Minimal abstract geometric composition, overlapping translucent shapes suggesting network connections, palette of warm gray (#F4F4F4) with subtle indigo (#4F46E5) accents, calm and precise mood, negative space on left third for text overlay, high resolution, clean edges, no text
```

### Quality Guidelines
- Generate 2 variants for hero images, pick the best
- Verify generated images match DESIGN.md color palette
- Check that images work at both light and dark themes if applicable
- Ensure images don't conflict with text overlay areas

## Step 4: Optimize and Place

1. **Optimize**: Convert to WebP, compress appropriately
2. **Name consistently**: `hero-{description}.webp`, `illustration-{section}.webp`, `icon-{name}.svg`, `og-image.png`
3. **Place in project**: `public/images/` directory
4. **Update components**: Add the images to the relevant page components with proper `next/image` usage:
   ```tsx
   <Image
     src="/images/hero-abstract.webp"
     alt="{descriptive alt text}"
     width={1920}
     height={1080}
     priority  // for hero images
     className="rounded-2xl"
   />
   ```

## Step 5: Maintain Image Library

Track all generated images in `public/images/README.md`:

```markdown
# Project Image Library

## Style Guide
- Palette: {from DESIGN.md}
- Mood: {from DESIGN.md}
- Generator: Gemini via nanobanana

## Images
| File | Type | Section | Prompt Summary |
|------|------|---------|----------------|
| hero-abstract.webp | hero | Home | Abstract geometric, indigo accents |
| ... | ... | ... | ... |
```

This ensures consistency when generating additional images later.

## Step 6: Output

Report:
- Images generated (file paths)
- Which sections they're placed in
- Any style notes for maintaining consistency
- Reminder to verify images render correctly at all breakpoints
