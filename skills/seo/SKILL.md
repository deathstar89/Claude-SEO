---
name: seo
description: SEO toolkit — full-site audits via parallel sub-agents, deep single-page analysis, schema markup audit/generation, AI-search (GEO) optimization, and industry-templated sitemap generation. Invoke for "/seo audit <url>", "/seo page <url>", "/seo schema <url>", "/seo geo <url>", or "/seo sitemap generate".
---

# /seo

Read `args` and dispatch on the first word (the subcommand). If `args` is
empty or the subcommand is unrecognized, show the five subcommands below
with one line each and ask which one to run.

| Subcommand | Args | Section |
|---|---|---|
| `audit` | `<url>` | [Full-site audit](#audit-full-site-audit) |
| `page` | `<url>` | [Single-page analysis](#page-single-page-deep-analysis) |
| `schema` | `<url>` | [Schema audit](#schema-schema-markup-audit) |
| `geo` | `<url>` | [AI search / GEO](#geo-ai-search-optimization) |
| `sitemap generate` | — | [Sitemap generation](#sitemap-generate-sitemap-generation) |

## Shared conventions

- **Fetching**: use `WebFetch` for rendered page content and `Bash`
  (`curl -sI`, `curl -s`) for raw HTML, headers, `robots.txt`, and
  `sitemap.xml` when you need the unrendered response. If neither tool
  can reach the target (network policy, auth wall, JS-only rendering this
  environment can't execute), say so plainly and ask the user to paste
  the HTML or a page export rather than guessing at content.
- **Be polite**: never crawl an entire site. Sample a handful of
  representative pages (see `references/audit-checklist.md` for how to
  pick them). Space out requests; don't parallelize raw fetches against
  the same host from a single agent.
- **Evidence over assertion**: every finding should quote or cite the
  specific thing observed (a title tag, a heading, a JSON-LD snippet), not
  a generic claim.
- **Don't fabricate data**: never invent metrics you can't measure from
  static analysis (e.g. a specific Core Web Vitals score requires a real
  browser/Lighthouse run, which this skill doesn't have). Say what you
  can't measure and why, rather than approximating it as fact.
- **URL normalization**: if the user gives a bare domain, try `https://`
  first, follow one redirect to find the canonical host, and use that for
  all subsequent fetches in the session.

---

## `audit`: Full-site audit

**Usage**: `/seo audit <url>`

Produces a single prioritized action plan by running four specialized
sub-agents in parallel, then synthesizing their findings.

### Steps

1. Normalize the URL. Fetch `robots.txt` and `sitemap.xml` to build a
   representative page sample (8-15 URLs) per
   `references/audit-checklist.md`. If there's no sitemap, note that as a
   finding and build the sample from homepage navigation links instead.
2. Spawn these four agents **in parallel** (one message, four `Agent`
   tool calls), each given: the root URL, the page sample list, and a
   pointer to read its own definition's checklist plus
   `references/audit-checklist.md` for shared severity definitions:
   - `seo-technical-auditor`
   - `seo-content-auditor`
   - `seo-schema-auditor`
   - `seo-geo-auditor`
   Ask each to return its findings list in the structured format its
   agent definition specifies — not prose.
3. Once all four return, follow the synthesis procedure in
   `references/audit-checklist.md` (`## Synthesizing the final action
   plan`): pool findings, score by Impact/Effort, de-duplicate overlap
   between agents, and bucket into P0/P1/P2.
4. Present the final report as:
   - A 3-6 sentence executive summary of overall SEO health and the
     single highest-leverage next action.
   - A markdown table: `# | Priority | Category | Page | Finding |
     Recommendation | Impact | Effort`.
   - A short note on what wasn't checked (e.g. real Core Web Vitals,
     backlink profile, keyword rankings) since this audit is
     content/markup/technical-signal based, not a full toolchain audit —
     be explicit about that boundary so the user doesn't overtrust scope
     this skill doesn't cover.

---

## `page`: Single-page deep analysis

**Usage**: `/seo page <url>`

A single-pass, single-page deep dive — no sub-agents needed, the scope is
one page.

### Steps

1. Fetch the page (rendered content via WebFetch, raw HTML via `curl` for
   tag-level checks that need exact markup).
2. Walk every section of `references/onpage-checklist.md`: metadata,
   content structure, content quality/E-E-A-T, media, structured data
   (apply `references/schema-guide.md` for this page's inferred role),
   and page-level technical/UX signals.
3. Output per the format at the end of `onpage-checklist.md`: a status
   line per section, specific findings with quoted evidence, and a closing
   **Top 5 fixes** list ordered by impact.

---

## `schema`: Schema markup audit

**Usage**: `/seo schema <url>`

Detect → validate → generate, for one page or (if the user says "site
wide"/"whole site") a small sample of key pages.

### Steps

1. If auditing a whole site, build a small page sample per
   `references/audit-checklist.md` and run the remaining steps once per
   page, then roll up a sitewide summary (are sitewide entities like
   `Organization`/`WebSite` declared consistently?). If it's a single URL,
   just do that page.
2. For each page, delegate to (or apply directly, if scope is a single
   page) the `seo-schema-auditor` logic: detect all structured data,
   infer the page's role, and validate against
   `references/schema-guide.md`'s type-by-page-role table — required
   properties present and non-empty, values matching visible content, no
   duplicate/conflicting declarations.
3. Report findings using the format in `seo-schema-auditor`'s output
   section (Category/Severity/Finding/Impact/Recommendation/Effort).
4. **Generate**: for missing or broken schema identified in step 2-3,
   produce ready-to-paste JSON-LD per the `## Generating markup` rules in
   `references/schema-guide.md` — real data only, `"TODO"` placeholders
   for anything you don't have, never fabricated values. End with a
   reminder to validate via Google's Rich Results Test before shipping.

---

## `geo`: AI search optimization

**Usage**: `/seo geo <url>`

### Steps

1. Fetch the page's text content as an LLM/crawler would encounter it.
2. Apply the full `references/geo-guide.md` framework: identify the
   page's strongest existing self-contained passage (if any), then run
   the passage self-containment test and primary-source alignment check
   against the page's most answer-worthy sections.
3. Report per `geo-guide.md`'s output format: per-page findings across
   the seven GEO categories with quoted passages and before/after
   rewrites for content fixes, closing with a sitewide note on the
   strongest primary-source claim available to lean into.
4. Keep this distinct from `/seo page` — don't re-report generic on-page
   issues (titles, alt text) here; stay focused on citability for AI
   answer engines specifically.

---

## `sitemap generate`: Sitemap generation

**Usage**: `/seo sitemap generate` (optionally with a URL/industry hint,
e.g. `/seo sitemap generate https://example.com` or `/seo sitemap
generate for a SaaS product`)

### Steps

1. Determine the industry template from `references/sitemap-templates.md`
   — infer it from a supplied URL by fetching and skimming the homepage,
   or ask the user directly if no URL/hint was given.
2. Determine the real URL list: crawl the site (homepage + nav/footer
   links, one or two hops) if a URL was given, or ask the user for an
   export/list if not. Never invent URLs.
3. Exclude non-canonical URLs (redirects, 404s, noindexed, thin duplicate
   filter/pagination pages) per the "Mechanics" section of
   `sitemap-templates.md`.
4. Emit a valid `sitemap.xml` (or `sitemap-index.xml` + child files if
   over ~50,000 URLs) as a code block, applying the chosen template's
   priority/changefreq bands and accurate `<lastmod>` where determinable.
5. Close with: which URLs were excluded and why, and which high-value
   page types the template suggests that the site appears to be missing.
