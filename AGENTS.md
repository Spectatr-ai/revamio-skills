# Revamio GTM Skills — Agent Context

Revamio is a GTM (go-to-market) intelligence product. It crawls a company,
derives its DNA, ICP, positioning, competitors, keywords, GEO/AEO citation
standing, buyer-intent signals, and an action plan. These skills turn that
real, company-specific intelligence into actual work: briefs, content,
comparison pages, schema, snippet rewrites, signal replies, and more.

## How these skills get their data

The skills depend on the **Revamio MCP server** (`@spectatr/revamio`,
registered as `revamio`). Its `revamio_*` tools return the user's own company
data (DNA, action plan, competitors, keywords, GEO report, signals, ICP, ads,
influencers, snippet opportunities, AI citations). The server is read-only.

If the `revamio_*` tools are not connected, ask the user to add the Revamio MCP
server: get an API key from the Revamio dashboard → **Settings → API &
Developer**, then register the server (`npx -y @spectatr/revamio`, with
`REVAMIO_API_KEY` set). In Claude Code, installing the Revamio plugin wires the
server automatically and prompts for the key.

## `revamio-context` is an optional cache

`revamio-context` builds and caches a `revamio-context.md` file (brand voice,
ICP, positioning, GEO standing, and gaps it interviews the user to fill). This
file is an **optional cache, not a gate**: the other skills use it if it's
present, and otherwise read what they need live from the MCP
(`revamio_describe_company` + `revamio_get_company_dna`) and proceed — they never
block waiting on it. Run `revamio-context` when you want to build or refresh that
cache.

## The 15 skills

- **revamio** — router; interprets a natural-language GTM request and dispatches to the right `revamio-*` skill.
- **revamio-context** — build/refresh the company GTM context file from MCP data, then interview the user on gaps. Optional cache — other skills use it if present, else read live from the MCP.
- **revamio-brief** — "what to work on now" GTM briefing; flags what can be executed this session.
- **revamio-action-executor** — read the action plan and execute it end to end, dispatching each item to the right specialist skill.
- **revamio-positioning** — turn Company DNA into value prop, positioning statement, messaging pillars, ICP copy.
- **revamio-write** — brand-voice-locked content (blog posts, landing copy, social) from DNA, ICP, and keywords.
- **revamio-content-brief** — turn a keyword/topic into a full SEO+GEO content brief grounded in real keyword data and competitor gaps.
- **revamio-geo-gaps** — turn the GEO/AEO report into a ranked, engine-by-engine citation-gap action queue.
- **revamio-aeo-content** — rewrite a page/passage so AI engines will cite it, targeting uncited AI queries.
- **revamio-schema** — generate paste-ready schema.org JSON-LD that closes GEO schema-audit gaps; inject into the codebase when connected.
- **revamio-snippet-capture** — turn winnable featured-snippet and People-Also-Ask opportunities into a ranked capture queue with paste-ready rewrites.
- **revamio-competitor-brief** — one-page competitor battlecard from competitor profiles, comparison, gaps, and recent moves.
- **revamio-competitor-page** — draft a "you vs <competitor>" / "<competitor> alternatives" comparison landing page, SEO/GEO-optimized and brand-voiced.
- **revamio-ad-intel** — cross keyword data with competitor ad intelligence into a paid-vs-organic opportunity brief.
- **revamio-signal-engage** — turn the buyer-intent signal feed into a ranked engage queue with a drafted, brand-voiced reply per high-intent signal.

## Operating principles

Skills are **artifact-first** (they produce concrete deliverables, not just
advice), **brand-voice locked** (output matches the company's derived voice),
and they **never fabricate data** — they use only what the Revamio MCP returns.
If data is missing, the skill says so rather than inventing it.
