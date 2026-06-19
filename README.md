# Revamio GTM Agent Skills

Agent Skills that turn your real Revamio GTM intelligence into work — briefs,
content, comparison pages, schema, snippet rewrites, and signal replies — in
your brand voice. They ride on the **Revamio MCP server** (`@spectatr/revamio`),
whose `revamio_*` tools return your company's own data: DNA, action plan,
competitors, keywords, GEO/AEO citations, buyer-intent signals, ICP, and more.
The skills never fabricate data — they only use what the MCP returns.

## Prerequisite

A Revamio account and an API key. Create one in the Revamio dashboard →
**[Settings → MCP & Skills](https://dashboard.revamio.com/settings/api-keys)** (`mk_live_…`). The Claude Code plugin route
prompts for this key automatically on install; the other routes need it set as
`REVAMIO_API_KEY` in your MCP client config.

> **Get your Revamio API key:** create one in the Revamio dashboard →
> **[Settings → MCP & Skills](https://dashboard.revamio.com/settings/api-keys)**
> (`https://dashboard.revamio.com/settings/api-keys`). It looks like `mk_live_…`.
> Paste it when the Claude Code plugin prompts you on install, or pass it as the
> `REVAMIO_API_KEY` env var when adding the MCP manually (`npx -y @spectatr/revamio`).

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

> **Installing via skills.sh?** skills.sh ships the **skills only** — the
> Revamio MCP server is **not** bundled there. After
> `npx skills add Spectatr-ai/revamio-skills`, add the MCP separately by
> running `npx -y @spectatr/revamio` with your Revamio API key
> (`mk_live_…`, from the dashboard → **[Settings → MCP & Skills](https://dashboard.revamio.com/settings/api-keys)**).
> Installing via the Claude Code **plugin**
> (`/plugin install revamio@revamio-skills`) bundles the MCP automatically —
> no separate step.

## `marketing-context` is an optional cache

**`marketing-context`** builds and caches a `revamio-context.md` file (brand voice,
ICP, positioning, GEO standing) from your Revamio data and interviews you only on
the gaps Revamio couldn't derive. It's an **optional cache, not a gate**: the
other skills use that file if it's present, and otherwise read what they need
live from the MCP (`revamio_describe_company` + `revamio_get_company_dna`) and
proceed — they never block waiting on it. Run **`marketing-context`** when you want
to build or refresh that cache. Not sure which skill you need? Just ask
**`marketing-router`**, the router skill, in plain language.

## Skills

| Skill | What it does |
| --- | --- |
| `marketing-router` | Entry point — interprets any natural-language GTM question and dispatches to the right skill. Start here if unsure. |
| `marketing-context` | Build/refresh the company GTM context file from Revamio's MCP data. Optional cache — other skills use it if present, else read live from the MCP. |
| `marketing-priorities` | "What should I work on this week?" — produces a ranked top-5 GTM briefing grounded in keyword opportunity, competitor moves, and AI citation gaps. |
| `execute-marketing-plan` | Work through your marketing action plan step by step, dispatching each item to the right specialist skill. |
| `brand-positioning-messaging` | Sharpen brand positioning, craft a value proposition, write a positioning statement, and define messaging pillars. |
| `brand-voice-writer` | Write a blog post, landing page, email, or social post in your company's brand voice. Includes SEO and AEO optimization pass. |
| `seo-content-optimizer` | Turn a keyword or topic into a writer-ready SEO + AEO content brief grounded in real keyword data and competitor gaps. |
| `aeo-geo-citation-audit` | Site-wide audit of why your company isn't appearing in AI engine answers — ranked fix list per engine. |
| `aeo-geo-optimizer` | Rewrite a specific page or passage so AI engines (ChatGPT, Perplexity, Google AI Overview) will cite it. |
| `json-ld-schema` | Generate paste-ready JSON-LD structured data (Organization, Product, FAQ, Article, BreadcrumbList) and close GEO schema gaps. |
| `snippet-paa-optimizer` | Win featured snippets and People Also Ask boxes — ranked capture queue with paste-ready 40–55 word answer blocks. |
| `competitor-intelligence` | Build an internal one-page competitor battlecard from profiles, content gaps, and recent moves. |
| `competitor-comparison-page` | Draft a "you vs [competitor]" or "best alternatives to [competitor]" SEO landing page. |
| `keyword-ad-strategy` | Decide which keywords to pay for (PPC) vs. rank for organically — grounded in CPC, keyword difficulty, and competitor ad data. |
| `buyer-intent-replies` | Find Reddit, Twitter, LinkedIn, and forum threads where prospects are asking questions your product answers — drafts a brand-voiced reply per signal. |

Skills point you to the relevant dashboard tab where helpful; in-app UI
walkthroughs are not yet included.

## License

MIT — see [LICENSE](./LICENSE).
