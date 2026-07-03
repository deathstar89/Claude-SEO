# Schema.org / structured data guide

Used by `/seo schema` and `seo-schema-auditor`. Covers detection,
validation, and generation. This is a working reference, not exhaustive —
when a type isn't listed here, check schema.org and Google's Search
Central structured data docs for the current required/recommended
properties before generating markup.

## Detection

Structured data can appear as:
- **JSON-LD** (recommended by Google): `<script
  type="application/ld+json">{...}</script>`, usually in `<head>` or end
  of `<body>`.
- **Microdata**: `itemscope itemtype="https://schema.org/X"` with
  `itemprop` attributes inline in HTML.
- **RDFa**: `typeof`/`property` attributes.

Extract every block, parse JSON-LD as JSON (a parse failure is a Critical
finding on its own), and record every `@type` present per page.

## Type-by-page-role table

| Page role | Primary type(s) | Required properties (Google) | Recommended |
|---|---|---|---|
| Homepage / sitewide | `Organization` or `LocalBusiness` subtype, `WebSite` | `name`, `url` | `logo`, `sameAs`, `WebSite.potentialAction` (SearchAction) if site search exists |
| Local business / single location | `LocalBusiness` (or a specific subtype, e.g. `LodgingBusiness`) | `name`, `address` | `telephone`, `geo`, `openingHoursSpecification`, `priceRange`, `image` |
| Vacation rental / property listing | `LodgingBusiness` or `Product`/`Accommodation` + `Offer` | `name`, `address` (LodgingBusiness) or `name`+`offers` (Product) | `image`, `amenityFeature`, `numberOfRooms`/`occupancy`, `aggregateRating`, `review` |
| Product / bookable item with price | `Product` + `Offer` | `name`, `offers.price`, `offers.priceCurrency`, `offers.availability` | `image`, `aggregateRating`, `review`, `brand` |
| Article / blog post | `Article` or `BlogPosting` | `headline`, `image`, `datePublished` | `dateModified`, `author` (with `name`), `publisher` |
| FAQ content that's visibly on the page | `FAQPage` with `mainEntity[].Question/Answer` | `mainEntity` with `name` (question) + `acceptedAnswer.text` | — |
| Step-by-step content visibly on the page | `HowTo` | `name`, `step` | `totalTime`, `image` per step |
| Category/listing page with a visible trail | `BreadcrumbList` | `itemListElement` with `position`, `name`, `item` | — |
| Reviews/testimonials visibly on the page | `Review` / `AggregateRating` (nested under the entity being reviewed) | `ratingValue`, and for `AggregateRating` a `ratingCount` or `reviewCount` | `author`, `reviewBody` |
| Events (e.g. seasonal offers, open days) | `Event` | `name`, `startDate`, `location` | `endDate`, `offers`, `image` |
| Jobs page | `JobPosting` | `title`, `description`, `datePosted`, `hiringOrganization`, `jobLocation` | `validThrough`, `employmentType` |

## Validation rules to enforce

1. **Required properties present and non-empty** — an empty string or a
   placeholder value ("N/A") counts as missing.
2. **Content match** — every fact in the markup (price, rating, review
   count, address, availability) must match what's visibly rendered on
   the page. Mismatches violate Google's structured-data guidelines and
   can trigger a manual action — treat as Critical.
3. **Don't fabricate content to justify markup** — only recommend
   `FAQPage`, `Review`, or `HowTo` markup for Q&A/review/step content that
   already, visibly exists on the page. If it doesn't exist, the
   recommendation is to *add the visible content first*, not just the
   markup.
4. **One canonical entity, referenced not re-declared** — sitewide
   `Organization`/`LocalBusiness` and `WebSite` nodes should be declared
   consistently (ideally once, with an `@id`, referenced via `@id` from
   other pages) rather than re-declared with different data on every
   page.
5. **Use the most specific applicable type** — e.g. `LodgingBusiness`
   over generic `LocalBusiness` for a villa rental site, `BlogPosting`
   over generic `Article` for a blog.

## Generating markup

When asked to generate JSON-LD:

- Only populate properties you have real data for from the page (or that
  the user supplies). Leave a clearly marked `"TODO"` placeholder rather
  than inventing values like phone numbers, prices, or ratings.
- Emit complete, valid, minified-but-readable JSON-LD ready to paste into
  `<head>`, wrapped in a single ` ```json ` code block per type.
- Prefer `@id` + `sameAs` linking over duplicating an entity's data
  across multiple JSON-LD blocks on the same page.
- After generating, remind the user to validate with Google's Rich
  Results Test and the Schema Markup Validator before shipping, since
  this skill cannot execute Google's live validators itself.
