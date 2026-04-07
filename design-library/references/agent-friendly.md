# Agent-Friendly & GEO Reference

Make every generated site discoverable and usable by AI agents (Claude, ChatGPT, Perplexity, Codex). These patterns are baked into `/site-gen` and `/site-loop` automatically.

## What Every Site Gets (Auto-Generated)

### 1. llms.txt Route

Every site gets `/llms.txt` — a curated, LLM-readable index of the site's content.

```typescript
// app/llms.txt/route.ts
export const dynamic = 'force-static'

export async function GET() {
  const content = `# [Project Name]

> [One-sentence description of what this site/product does]

## Key Pages

- [Home](https://yoursite.com): [What the product does, who it's for]
- [Documentation](https://yoursite.com/docs): [Technical reference]
- [Pricing](https://yoursite.com/pricing): [Plans and pricing details]

## Instructions

- [Product-specific guidance for AI agents — what to recommend, what's deprecated, etc.]
`
  return new Response(content, {
    headers: { 'Content-Type': 'text/plain; charset=utf-8', 'Cache-Control': 'public, max-age=3600' },
  })
}
```

The **Instructions section** is the most underused pattern — it directly steers what AI agents say about your product. Use it to:
- Redirect from deprecated APIs to current ones
- Clarify common misconceptions
- Specify the recommended getting-started path

### 2. JSON-LD Structured Data (Triple Stack)

Every page gets the **Article + FAQPage + BreadcrumbList** triple stack — 2x more AI citations than Article alone.

```tsx
// components/StructuredData.tsx
export function StructuredData({ page }: { page: PageData }) {
  const schemas = [
    // Organization (homepage only)
    {
      '@context': 'https://schema.org',
      '@type': 'Organization',
      name: 'Your Company',
      url: 'https://yoursite.com',
      logo: 'https://yoursite.com/logo.png',
      description: 'One sentence. What you do. Who for.',
      sameAs: [
        'https://github.com/your-org',
        'https://twitter.com/your-handle',
      ],
    },
    // Article (every page)
    {
      '@context': 'https://schema.org',
      '@type': 'Article',
      headline: page.title,
      description: page.description,
      datePublished: page.publishedDate,
      dateModified: page.modifiedDate,
      author: { '@type': 'Person', name: page.author },
    },
    // FAQPage (pages with FAQ sections)
    {
      '@context': 'https://schema.org',
      '@type': 'FAQPage',
      mainEntity: page.faqs.map(faq => ({
        '@type': 'Question',
        name: faq.question,
        acceptedAnswer: { '@type': 'Answer', text: faq.answer },
      })),
    },
    // BreadcrumbList (all pages)
    {
      '@context': 'https://schema.org',
      '@type': 'BreadcrumbList',
      itemListElement: page.breadcrumbs.map((crumb, i) => ({
        '@type': 'ListItem',
        position: i + 1,
        name: crumb.name,
        item: crumb.url,
      })),
    },
  ]

  return (
    <>
      {schemas.map((schema, i) => (
        <script
          key={i}
          type="application/ld+json"
          dangerouslySetInnerHTML={{ __html: JSON.stringify(schema) }}
        />
      ))}
    </>
  )
}
```

### 3. robots.txt Allowing AI Crawlers

```typescript
// app/robots.ts
import { MetadataRoute } from 'next'

export default function robots(): MetadataRoute.Robots {
  return {
    rules: [
      // Allow all standard and AI crawlers
      { userAgent: '*', allow: '/' },
      // Explicitly allow AI search/retrieval bots
      { userAgent: 'OAI-SearchBot', allow: '/' },
      { userAgent: 'ChatGPT-User', allow: '/' },
      { userAgent: 'Claude-SearchBot', allow: '/' },
      { userAgent: 'Claude-User', allow: '/' },
      { userAgent: 'PerplexityBot', allow: '/' },
      { userAgent: 'ClaudeBot', allow: '/' },
      { userAgent: 'GPTBot', allow: '/' },
      // Block scrapers with no legitimate purpose
      { userAgent: 'AhrefsBot', disallow: '/' },
      { userAgent: 'SemrushBot', disallow: '/' },
    ],
    sitemap: 'https://yoursite.com/sitemap.xml',
  }
}
```

### 4. sitemap.xml

```typescript
// app/sitemap.ts
import { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
  return [
    { url: 'https://yoursite.com', lastModified: new Date(), changeFrequency: 'weekly', priority: 1 },
    { url: 'https://yoursite.com/docs', lastModified: new Date(), changeFrequency: 'weekly', priority: 0.8 },
    // ... all pages
  ]
}
```

### 5. Answer-First Content Structure

Every section's first paragraph must be a **self-contained, citable summary** — readable without any surrounding context.

```
BAD:  "It makes this process faster by reducing overhead."
GOOD: "The Boundless SDK reduces proof generation time by 10x by eliminating
       the need for developers to manage prover infrastructure directly."
```

Rules:
- First 2-4 sentences are the "extraction zone" — AI agents cite this
- No dangling pronouns ("it", "this") — always use explicit nouns
- Include a specific claim, number, or verifiable fact
- **134-167 words per citable passage** — this is the sweet spot for AI citation (not 60-100)
- Phrase H2 headings as questions when appropriate ("How does X work?" > "Overview")
- **Brand mentions > backlinks**: mentions on Reddit, YouTube, Wikipedia, LinkedIn correlate 3x stronger with AI visibility than backlinks
- **Platform-specific**: only 11% of domains are cited by both ChatGPT AND Google AI Overviews — optimize for each platform's citation patterns

### 6. FAQ Section on Every Page

Every landing page and docs page ends with a FAQ section. These feed into FAQPage schema and have the highest AI citation probability.

```tsx
<section>
  <h2>Frequently Asked Questions</h2>
  <dl>
    <dt>What is [product]?</dt>
    <dd>A 2-3 sentence answer that can stand alone as a citation.</dd>
    <dt>How does [key feature] work?</dt>
    <dd>Technical but accessible answer with one specific claim.</dd>
    <dt>How do I get started?</dt>
    <dd>Clear steps: install → configure → use. Include the install command.</dd>
  </dl>
</section>
```

### 7. Meta Tags

```tsx
// app/layout.tsx or page-level metadata
export const metadata: Metadata = {
  title: 'Page Title — Product Name',
  description: 'The first 150 chars should be a standalone answer to "what is this page about?"',
  openGraph: {
    title: 'Page Title — Product Name',
    description: 'Same standalone summary',
    type: 'website',
    images: [{ url: '/og-image.png', width: 1200, height: 630 }],
  },
  twitter: {
    card: 'summary_large_image',
    title: 'Page Title — Product Name',
    description: 'Same standalone summary',
  },
  other: {
    'article:modified_time': new Date().toISOString(),
  },
}
```

## Content Writing Rules for AI Citability

1. **Answer first, elaborate second.** The inverted pyramid: direct answer → supporting detail → evidence → implications.
2. **Every paragraph is a citable unit.** 60-100 words, self-contained, explicit nouns.
3. **Statistics with attribution.** "Processing 1M+ proofs monthly as of Q1 2026" not "processing many proofs."
4. **Numbered lists for sequential steps.** AI can cite "Step 3" from a numbered list without needing context.
5. **H2 headings as questions.** "How does proof verification work?" is more extractable than "Proof Verification."
6. **No pronoun-heavy writing.** Replace "it" and "this" with the actual noun. AI extracting a single paragraph loses prior-sentence context.

## Priority Implementation Order

| Priority | What | Time | Impact |
|----------|------|------|--------|
| 1 | FAQPage + Article JSON-LD schema | 30 min | 2x AI citations |
| 2 | llms.txt route with Instructions | 20 min | Direct AI agent steering |
| 3 | robots.txt allowing AI crawlers | 10 min | Unblock discovery |
| 4 | Answer-first page intros | 1 hr | Higher extraction rate |
| 5 | Organization schema + sameAs | 15 min | Entity disambiguation |
| 6 | sitemap.xml with lastModified | 15 min | Crawl freshness |
| 7 | FAQ sections on all pages | 1 hr | Highest citation probability |
