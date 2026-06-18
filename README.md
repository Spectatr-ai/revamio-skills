# Revamio GTM Agent Skills

Agent Skills that turn your real Revamio GTM intelligence into work — briefs,
content, comparison pages, schema, snippet rewrites, and signal replies — in
your brand voice. They ride on the **Revamio MCP server** (`@spectatr/revamio`),
whose `revamio_*` tools return your company's own data: DNA, action plan,
competitors, keywords, GEO/AEO citations, buyer-intent signals, ICP, and more.
The skills never fabricate data — they only use what the MCP returns.

## Prerequisite

A Revamio account and an API key. Create one in the Revamio dashboard →
**Settings → API & Developer** (`mk_live_…`). The Claude Code plugin route
prompts for this key automatically on install; the other routes need it set as
`REVAMIO_API_KEY` in your MCP client config.

## Install

| Channel | Command |
| --- | --- |
| **skills.sh / any agent (70+)** | `npx skills add Spectatr-ai/revamio-skills` |
| **Claude Code plugin** (bundles the MCP server + skills, prompts for your API key) | `/plugin marketplace add Spectatr-ai/revamio-skills` then `/plugin install revamio@revamio-skills` |
| **Cursor / other clients** | `npx skills add Spectatr-ai/revamio-skills -a cursor` |

For the skills.sh / Cursor / other-client routes you also need to register the
Revamio MCP server in your client's MCP config:

```json
{
  "mcpServers": {
    "revamio": {
      "command": "npx",
      "args": ["-y", "@spectatr/revamio"],
      "env": { "REVAMIO_API_KEY": "mk_live_…" }
    }
  }
}
```

The Claude Code plugin wires this up for you and prompts for the key.

## `revamio-context` is an optional cache

**`revamio-context`** builds and caches a `revamio-context.md` file (brand voice,
ICP, positioning, GEO standing) from your Revamio data and interviews you only on
the gaps Revamio couldn't derive. It's an **optional cache, not a gate**: the
other skills use that file if it's present, and otherwise read what they need
live from the MCP (`revamio_describe_company` + `revamio_get_company_dna`) and
proceed — they never block waiting on it. Run **`revamio-context`** when you want
to build or refresh that cache. Not sure which skill you need? Just ask
**`revamio`**, the router skill, in plain language.

## Skills

| Skill | What it does |
| --- | --- |
| `revamio` | Router; interprets a natural-language GTM request and dispatches to the right `revamio-*` skill. |
| `revamio-context` | Build/refresh the company GTM context file from Revamio's MCP data, then interview the user on the gaps. Optional cache — other skills use it if present, else read live from the MCP. |
| `revamio-brief` | Produce a "what to work on now" GTM briefing and flag what can be executed this session. |
| `revamio-action-executor` | Read the action plan and execute it end to end, dispatching each item to the right specialist skill. |
| `revamio-positioning` | Turn Company DNA into value proposition, positioning statement, messaging pillars, and ICP-specific copy. |
| `revamio-write` | Generate brand-voice-locked content (blog posts, landing copy, social) from DNA, ICP, and keywords. |
| `revamio-content-brief` | Turn a keyword/topic into a full SEO+GEO content brief grounded in real keyword data and competitor gaps. |
| `revamio-geo-gaps` | Turn the GEO/AEO report into a ranked, engine-by-engine citation-gap action queue. |
| `revamio-aeo-content` | Rewrite a page or passage so AI engines will cite it, targeting uncited AI queries. |
| `revamio-schema` | Generate paste-ready schema.org JSON-LD that closes GEO schema-audit gaps; inject into the codebase when connected. |
| `revamio-snippet-capture` | Turn winnable featured-snippet and People-Also-Ask opportunities into a ranked capture queue with paste-ready rewrites. |
| `revamio-competitor-brief` | Build a one-page competitor battlecard from competitor profiles, comparison, gaps, and recent moves. |
| `revamio-competitor-page` | Draft a "you vs <competitor>" / "<competitor> alternatives" comparison landing page, SEO/GEO-optimized and brand-voiced. |
| `revamio-ad-intel` | Cross keyword data with competitor ad intelligence into a paid-vs-organic opportunity brief. |
| `revamio-signal-engage` | Turn the buyer-intent signal feed into a ranked engage queue with a drafted, brand-voiced reply per high-intent signal. |

Skills point you to the relevant dashboard tab where helpful; in-app UI
walkthroughs are not yet included.

## License

MIT — see [LICENSE](./LICENSE).
