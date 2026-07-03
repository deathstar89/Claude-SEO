# GEO (Generative Engine Optimization) guide

Used by `/seo geo` and `seo-geo-auditor`. GEO is about maximizing the
odds that AI answer engines (Google AI Overviews/AI Mode, ChatGPT search,
Perplexity, Copilot, Gemini) extract and cite a page's content — a
different (and additive) goal from ranking a blue link.

## Core principles

1. **Passages are the unit of retrieval, not pages.** Answer engines
   chunk pages into passages and retrieve/cite at that granularity. A
   great page with no single self-contained, quotable passage on a given
   question loses to a worse page that has one.
2. **Self-containment beats elegance.** A passage that can be lifted out
   of the page and still make full sense — naming its subject, stating
   the claim and the supporting detail together — is far more citable
   than prose that relies on the surrounding paragraphs for context
   ("it", "this", "as mentioned above").
3. **Lead with the answer.** Question-shaped headings followed
   immediately by a direct, concise answer (then elaboration) outperform
   marketing preambles that delay the actual information.
4. **Specificity is evidence.** Concrete numbers, names, dates, and
   original data read as more trustworthy to both the retrieval step and
   the generating model than vague claims ("many guests love our villas"
   vs. "94% of 2025 guests rated the pool area 5 stars").
5. **Be the primary source when you can be.** For facts that are yours to
   state authoritatively (your own prices, availability, policies,
   specs), state them plainly and keep them dated/current. For facts that
   aren't yours (general destination info, regulations, third-party
   data), cite the actual primary source — don't restate a secondary
   summary as if it were original, and don't contradict the primary
   source.
6. **Structure is a retrieval signal.** Lists, tables, definition-style
   term/description pairs, and FAQ blocks are disproportionately favored
   because they're easy to chunk and easy to quote verbatim.
7. **Trust signals must be crawlable text.** Author identity, business
   identity, and policy information baked into images or client-rendered
   widgets without a text fallback are invisible to the extraction step.

## Passage self-containment test

For a candidate passage, check:

- Does it name its subject explicitly rather than relying on pronouns
  tied to earlier sentences?
- Does it contain both the claim and the detail that supports it (not
  just the claim, with support two paragraphs away)?
- Would it still be accurate and clear if it were the *only* sentence
  quoted in an AI answer, with a link back to the page?

If no passage on the page passes this test for a question the page is
clearly trying to answer, that's the single most important GEO finding
for that page — recommend adding one direct, self-contained answer
passage near the top of the relevant section.

## Primary-source alignment check

- Identify factual/definitional claims on the page that aren't
  proprietary to the business (e.g. "the best time to visit the region
  is April-June").
- Check whether the claim aligns with what authoritative sources on that
  topic actually say. Flag contradictions or unsourced restatements of
  someone else's data.
- Where the business *is* the authority (its own listings, pricing,
  house rules), confirm the page states this itself rather than only
  through third-party aggregators — that's what makes it the citable
  primary source instead of the aggregator's page.

## Output format for `/seo geo`

For each key page: list the strongest existing passage (if any) as a
model to reuse, then up to 5 findings using the categories in
`agents/seo-geo-auditor.md` (Passage Structure / Direct-Answer Format /
Entity Clarity / Sourcing & Specificity / Structured Extractability /
Primary-Source Alignment / Trust Signal Readability), each with a
before/after rewrite when the fix is a content change. Close with a short
sitewide summary: is there a clear primary-source claim to lean into
(e.g. exclusive listings, direct-owner pricing) that competitors/
aggregators can't make, and where should it be stated most explicitly?
