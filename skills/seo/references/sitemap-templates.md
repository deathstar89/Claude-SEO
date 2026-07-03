# Sitemap generation: industry templates

Used by `/seo sitemap generate`. Each template lists the page types an
XML sitemap for that industry should cover, suggested `<priority>` and
`<changefreq>` bands (advisory only — Google largely ignores these now,
but they're still useful for internal prioritization and for sitemap
consumers that do respect them), and any specialized sitemap type
(image/video/news) worth splitting out.

## Choosing a template

Ask the user which industry best fits if it isn't already obvious from
context, or infer it from the site's content during a crawl. Industries
below are starting points — combine or adapt as needed.

### E-commerce
- Homepage (1.0, daily)
- Category/collection pages (0.8, weekly)
- Product pages (0.6-0.8 depending on catalog size, weekly if
  stock/price changes often)
- Search/filter landing pages — usually **exclude** from the sitemap if
  they're thin/duplicate; only include curated filter pages with real
  content.
- Blog/guides (0.5, monthly)
- Legal/policy pages (0.3, yearly)
- Consider a separate **image sitemap** for product photography.

### Blog / content publisher
- Homepage (1.0, daily)
- Category/topic hub pages (0.7, weekly)
- Individual articles (0.6, set `<lastmod>` accurately — this one matters
  more than priority for freshness signals)
- Author pages (0.3, monthly)
- Consider a **news sitemap** if publishing time-sensitive content
  eligible for Google News.

### Local business / service provider
- Homepage (1.0, weekly)
- Service pages, one per service (0.8, monthly)
- Location pages, one per service area if multi-location (0.8, monthly)
- About/team, contact (0.5, yearly)
- Reviews/testimonials page if standalone (0.4, monthly)
- Blog/resources (0.5, monthly)

### SaaS / software
- Homepage (1.0, weekly)
- Product/feature pages (0.8, monthly)
- Pricing (0.8, monthly)
- Use case / solution pages (0.7, monthly)
- Docs/help center — usually a **separate sitemap** given volume and
  different update cadence
- Blog (0.5, monthly)
- Legal (0.3, yearly)

### Real estate & vacation rentals (villas, holiday lets)
- Homepage (1.0, weekly)
- Destination/location hub pages, one per region (0.8, monthly)
- Individual property/listing pages (0.7-0.9, weekly — availability and
  pricing change often, keep `<lastmod>` accurate so crawlers revisit)
- Amenities/collection pages (e.g. "villas with private pools") if they
  carry unique curated content, not just a filtered re-listing (0.6,
  monthly)
- Booking/enquiry, about, contact (0.5, yearly)
- Guest reviews page if standalone (0.4, monthly)
- Blog/travel guides (0.5, monthly)
- Consider an **image sitemap** for property photography — high-quality
  imagery is often the primary search/discovery asset for this vertical.

### Marketplace (multi-vendor)
- Homepage (1.0, daily)
- Category pages (0.8, weekly)
- Vendor/seller profile pages (0.6, weekly)
- Listing pages (0.5-0.7, daily if inventory turns over fast — consider
  excluding listings that expire quickly and would just bloat/rot the
  sitemap)

## Mechanics

- Split any sitemap over 50,000 URLs or 50MB uncompressed into multiple
  files under a `<sitemapindex>`.
- Only include canonical, indexable (200 status, no noindex) URLs — never
  include redirects, 404s, noindexed pages, or paginated duplicate pages.
- Set `<lastmod>` to the actual last content-modification date, not the
  generation date — accurate `<lastmod>` is the one sitemap signal
  crawlers still weight meaningfully.
- Reference the sitemap (or index) from `robots.txt` via a `Sitemap:`
  line and submit it in Search Console/Bing Webmaster Tools.

## Output format for `/seo sitemap generate`

1. Confirm/infer the industry template and the list of real URLs (crawl
   the site or ask the user for a page list/export if none exists).
2. Produce a valid `sitemap.xml` (or a `sitemap-index.xml` plus child
   files if the URL count warrants splitting) as a code block, using the
   chosen template's priority/changefreq bands.
3. Call out any URLs you excluded and why (redirects, thin pages,
   duplicates), and any high-value page types the template suggests that
   the site doesn't seem to have yet (e.g. no destination hub pages on a
   villas site).
