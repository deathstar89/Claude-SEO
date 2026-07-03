# Single-page deep analysis checklist

Used by `/seo page <url>`. This is a single-page, single-pass version of
the content + technical + schema checks, done directly (no sub-agents)
since the scope is one page.

## 1. Metadata

- `<title>`: present, unique, ~50-60 chars, leads with the primary
  topic/entity, not truncated in a SERP preview at that length.
- `<meta name="description">`: present, ~140-160 chars, reads as a
  compelling summary (not keyword list), matches what the page delivers.
- `<link rel="canonical">`: present, self-referencing (or intentionally
  pointing elsewhere with a clear reason), absolute URL.
- `<meta name="robots">` / `x-robots-tag` header: confirm indexable
  unless intentionally excluded.
- Open Graph / Twitter card tags: present for social sharing
  (`og:title`, `og:description`, `og:image`, `og:url`).
- `hreflang` (if multi-market): present and reciprocal.

## 2. Content structure

- Exactly one `<h1>`, matching the page's actual topic.
- Logical h2/h3 hierarchy — no skipped levels, no headings used purely
  for styling.
- Answer-first paragraphs under question-style headings where relevant.
- Word count and depth appropriate for the page's intent (a property
  listing needs different depth than a pillar blog post — judge by intent,
  not a fixed number).
- Scannability: lists, tables, short paragraphs, bolded key terms where
  it aids skimming (not overused).

## 3. Content quality & E-E-A-T

- Specific, first-hand detail vs. generic template copy.
- Author/byline and credentials if it's informational content; business
  identity and contact info if it's commercial.
- Dates: published/updated, especially for time-sensitive claims.
- Trust signals: reviews with specifics, credentials, certifications,
  policies (cancellation, pricing terms) stated plainly.
- Internal links: contextual, descriptive anchor text, pointing to
  genuinely related pages; no orphaning of this page from the rest of the
  site (check what links here, not just what it links out to).
- External links: cite primary sources for factual claims not original
  to the business.

## 4. Media

- Every meaningful `<img>` has descriptive, non-keyword-stuffed `alt`
  text; decorative images have empty `alt=""`.
- Images have explicit `width`/`height` (or aspect-ratio CSS) to avoid
  layout shift.
- File sizes reasonable for web delivery; lazy-loading (`loading="lazy"`)
  on below-the-fold images.
- Video (if present): has a text transcript or surrounding descriptive
  copy so its content isn't invisible to crawlers.

## 5. Structured data

- Run the checks in `schema-guide.md` for this page's role. At minimum:
  confirm valid JSON-LD parses, required properties present, and it
  matches visible content.

## 6. Technical/UX signals visible at the page level

- Mobile viewport meta tag present.
- No obvious render-blocking pile-up of synchronous scripts before any
  CSS.
- Primary CTA is clear and not competing with more than one or two other
  asks.
- Page loads over HTTPS with no mixed-content resources.

## Output format for `/seo page`

Organize the report by the six sections above. For each section give a
short status line (Good / Needs work / Missing) plus specific findings
with evidence (quote the actual title/heading/alt text found). Close with
a **Top 5 fixes** list ordered by impact, each one or two sentences,
concrete enough to hand to a developer or content editor as-is.
