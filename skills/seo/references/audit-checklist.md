# Full-site audit: scope and prioritization framework

Used by `/seo audit` to scope what the parallel sub-agents look at and how
their findings get merged into one prioritized action plan.

## Audit categories (mapped to sub-agents)

| Category | Agent | Covers |
|---|---|---|
| Technical | `seo-technical-auditor` | robots.txt, sitemap, HTTPS/redirects, indexability, URL structure, status codes, performance proxies, security headers, hreflang |
| Content & On-Page | `seo-content-auditor` | titles/meta, headings, content depth/uniqueness, search intent, E-E-A-T, internal linking, images, readability |
| Structured Data | `seo-schema-auditor` | JSON-LD/microdata detection, required-property validation, content-consistency, gaps |
| AI-Search (GEO) | `seo-geo-auditor` | passage citability, direct-answer structure, entity clarity, sourcing, primary-source alignment |

## Building the page sample

Full crawls aren't practical or polite from a chat session. Build a
representative sample (roughly 8-15 URLs for a typical site) before
spawning sub-agents:

1. Fetch `/robots.txt` and `/sitemap.xml` (follow sitemap indexes one
   level deep).
2. From the sitemap, pick: the homepage, 2-3 top-level category/listing
   pages, 2-3 leaf/detail pages (product, property, article), the
   about/contact/trust pages, and 1-2 pages that look highest-value by
   URL (e.g. under `/blog/`, `/services/`).
3. If there's no sitemap, crawl one hop from the homepage's nav/footer
   links instead, and flag the missing sitemap itself as a technical
   finding.
4. Pass the same URL list to every sub-agent so their findings are about
   the same pages and can be cross-referenced.

## Severity definitions (shared across all agents)

- **Critical** — actively blocking indexing/ranking or a policy violation
  risk (e.g. sitewide noindex, robots.txt blocking everything, structured
  data that misrepresents the page).
- **High** — meaningfully suppressing visibility or conversion on
  important pages (thin/duplicate content on money pages, missing titles,
  broken canonical structure).
- **Medium** — a real but bounded opportunity (missing alt text, weak
  internal linking, missing recommended schema properties).
- **Low** — polish/best-practice items with marginal individual impact.

## Synthesizing the final action plan

After all sub-agents return their findings lists:

1. Pool every finding into one list.
2. Score each on **Impact** (map severity → 3/2/1/0.5 for
   Critical/High/Medium/Low) and **Effort** (S=1, M=2, L=3), then rank by
   `Impact / Effort` descending. This surfaces high-impact/low-effort
   "quick wins" first without hiding genuinely large but critical fixes.
3. Group the ranked list into three tiers for the report:
   - **P0 — Fix now** (Critical/High severity, regardless of effort)
   - **P1 — High ROI** (best Impact/Effort ratio among the remainder)
   - **P2 — Backlog** (everything else, still worth doing)
4. De-duplicate overlapping findings from different agents (e.g. the
   content auditor and GEO auditor both flagging the same thin page) into
   a single line that credits both angles.
5. Present the plan as a markdown table: `# | Priority | Category | Page |
   Finding | Recommendation | Impact | Effort`, followed by a short
   narrative summary (3-6 sentences) of the site's overall SEO health and
   the single highest-leverage next action.
