# JSON-LD templates

Fill with real data from `revamio-context.md`. Never invent ratings, awards,
review counts, founders, or `sameAs` profiles that weren't confirmed.

## Organization (+ sameAs)
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "<company>",
  "url": "<https://...>",
  "logo": "<https://.../logo.png>",
  "description": "<positioning statement, brand voice>",
  "sameAs": ["<linkedin>", "<x>", "<crunchbase>", "<github>"]
}
```

## Product
The live GEO audit often uses a product-centric taxonomy and may report a
missing `Product` type. Emit `offers`/`aggregateRating` **only** with real,
confirmed pricing and review data — never invent a price, currency, rating, or
review count. Drop any sub-field you can't ground.
```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "<product>",
  "description": "<positioning statement, brand voice>",
  "brand": {"@type": "Brand", "name": "<company>"},
  "url": "<https://.../product>"
}
```

## WebSite (+ SearchAction)
Include `potentialAction`/SearchAction **only if the site has a working search**
(e.g. `/search?q=`). Ask the user first; if there's no real search endpoint,
emit the WebSite block WITHOUT `potentialAction` — a SearchAction pointing at a
nonexistent search is misleading structured data.
```json
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "<company>",
  "url": "<https://...>",
  "potentialAction": {
    "@type": "SearchAction",
    "target": "<https://.../search?q={query}>",
    "query-input": "required name=query"
  }
}
```

## FAQPage (use real Q&A for top citation-gap queries)
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [{
    "@type": "Question",
    "name": "<the actual query a user asks an AI engine>",
    "acceptedAnswer": {
      "@type": "Answer",
      "text": "<40–60 word answer-first reply, brand voice, one real fact>"
    }
  }]
}
```

## Article / BlogPosting (E-E-A-T signals)
```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "<title>",
  "datePublished": "<ISO>",
  "dateModified": "<ISO>",
  "author": {"@type": "Person", "name": "<author>", "url": "<author page>"},
  "publisher": {"@type": "Organization", "name": "<company>", "logo": {"@type": "ImageObject", "url": "<logo>"}}
}
```

## BreadcrumbList
```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {"@type": "ListItem", "position": 1, "name": "Home", "item": "<https://...>"},
    {"@type": "ListItem", "position": 2, "name": "<section>", "item": "<url>"}
  ]
}
```

## Notes
- FAQPage rich results are restricted by Google to gov/health, but FAQ schema
  is still useful as a GEO structural signal for ChatGPT/Perplexity. Use it for
  AI citation, not for expecting a Google rich result.
- `dateModified` freshness helps Perplexity. Keep it honest.
- Schema's direct AI-citation lift is small on its own — pair with real,
  citable content (`revamio-aeo-content`).
