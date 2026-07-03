# Claude SEO

An SEO toolkit for [Claude Code](https://claude.com/claude-code), delivered
as a `/seo` skill with five subcommands. It audits sites the way a human
SEO consultant would — fetching real pages, quoting real evidence, and
never fabricating metrics it can't measure — rather than guessing at
scores.

## Install

Add this repo as a Claude Code plugin (via a marketplace entry, or by
cloning it into a directory Claude Code loads plugins from) so the `seo`
skill and its four supporting sub-agents are available in your session.

## Usage

```
# Full site audit: parallel sub-agents produce a prioritized action plan
/seo audit https://example.com

# Deep single-page analysis: on-page elements, content quality, schema
/seo page https://example.com/about

# Schema markup audit: detect, validate, generate
/seo schema https://example.com/

# AI search optimization: passage citability + primary-source-aligned recommendations
/seo geo https://example.com/

# Generate a sitemap with industry templates
/seo sitemap generate
```

## How it works

| Command | What it does |
|---|---|
| `/seo audit <url>` | Builds a representative page sample, runs four sub-agents in parallel (`seo-technical-auditor`, `seo-content-auditor`, `seo-schema-auditor`, `seo-geo-auditor`), then merges their findings into one Impact/Effort-ranked, P0/P1/P2 action plan. |
| `/seo page <url>` | A single-pass deep dive on one page: metadata, heading structure, content quality & E-E-A-T, media, structured data, and page-level technical signals — closing with a Top 5 fixes list. |
| `/seo schema <url>` | Detects existing JSON-LD/microdata/RDFa, validates required properties and content-consistency against schema.org and Google's rich-result requirements, and generates ready-to-paste JSON-LD for gaps — using real data only, never fabricated values. |
| `/seo geo <url>` | Evaluates passages for AI-answer-engine citability: self-containment, direct-answer structure, entity clarity, sourcing/specificity, and primary-source alignment — a distinct lens from traditional on-page SEO. |
| `/seo sitemap generate` | Generates a valid `sitemap.xml` using an industry template (e-commerce, blog, local business, SaaS, real estate/vacation rentals, marketplace), from a real crawled or supplied URL list. |

## Structure

```
.claude-plugin/plugin.json      Plugin manifest
skills/seo/SKILL.md             Main dispatcher for all five subcommands
skills/seo/references/          Checklists and frameworks each subcommand applies
agents/                         Sub-agents spawned in parallel by /seo audit
```

## Scope & honesty about limits

This toolkit works from fetched HTML/text and static analysis. It does
not run a real browser, so it cannot report actual Core Web Vitals,
cannot see backlink profiles, and cannot see keyword ranking data. Every
report says explicitly what wasn't checked rather than approximating
those numbers.
