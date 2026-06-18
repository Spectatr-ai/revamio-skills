---
name: revamio
description: >-
  Router for Revamio's GTM skills — interprets a natural-language GTM request
  and dispatches to the right revamio-* skill. Use when the user says "revamio
  ...", "use Revamio to ...", "help me with my GTM / SEO / GEO / content /
  competitors / outreach", or isn't sure which Revamio skill they need.
metadata:
  version: "0.2.1"
---

# Revamio (router)

**Requires the Revamio MCP server.** Every Revamio skill calls `revamio_*` tools served by the
Revamio MCP server (`@spectatr/revamio`, usually registered as `revamio`). If your agent doesn't
list those tools, the server isn't connected — add it from your Revamio dashboard →
**Settings → API & Developer** (mint a key, copy the install command), then retry.

Map the user's intent to one specialist skill, then run that skill's workflow.
If `revamio-context.md` does not exist yet, run **revamio-context** first
(every skill depends on it).

## Intent → skill (the catalog)

Every row below is **live** (the skill is built and packaged). When you add a row
for a capability that isn't built yet, mark it **forthcoming** in the Status
column so this table can't silently rot — never dispatch a forthcoming row.

| User intent (examples) | Skill | Status |
|---|---|---|
| "set up Revamio", "load/refresh my context", "what does Revamio know" | **revamio-context** | live |
| "what should I work on", "brief me", "status", "highest-leverage next step" | **revamio-brief** | live |
| "why aren't we cited in ChatGPT/Perplexity", "GEO gaps", "AI visibility", "citation gaps" | **revamio-geo-gaps** | live |
| "add schema", "JSON-LD", "structured data", "fix my schema markup" | **revamio-schema** | live |
| "rewrite this page for AI", "make this citable", "AEO content", "passage optimization" | **revamio-aeo-content** | live |
| "win featured snippets", "capture position zero", "answer the PAA boxes", "own the answer box" | **revamio-snippet-capture** | live |
| "write a blog post / landing page / social post in our voice" | **revamio-write** | live |
| "content brief", "what should this article cover", "plan content for keyword X" | **revamio-content-brief** | live |
| "execute my action plan", "do my GTM tasks", "run the next actions", "just do the plan" | **revamio-action-executor** | live |
| "should I run ads on X", "paid vs organic", "what are competitors bidding on", "ad strategy" | **revamio-ad-intel** | live |
| "competitor battlecard", "brief me on <competitor>", "competitive landscape", "what did rivals just do" | **revamio-competitor-brief** | live |
| "respond to signals", "what conversations should I join", "draft replies to signals", "intent feed" | **revamio-signal-engage** | live |
| "fix our positioning", "write our messaging", "value prop", "positioning statement", "messaging pillars" | **revamio-positioning** | live |
| "make a vs page", "competitor comparison page", "alternatives page", "X vs us" | **revamio-competitor-page** | live |

When the request is an action-plan item carrying an `action_type` (e.g. via
**revamio-brief** or **revamio-action-executor**), route by the authoritative
`action_type → skill` table in
**revamio-action-executor/references/dispatch-map.md** rather than this catalog.

## Dispatch rule
Only dispatch to a **live** skill above. If the request doesn't map to one (e.g.
cold-email sequences / outbound), do NOT invent or dispatch to a skill that isn't
listed — handle it inline using `revamio-context` data (e.g. draft directly from
DNA + action plan) and say it isn't yet a packaged skill.

## Behavior
1. If intent is ambiguous, ask one clarifying question, then dispatch.
2. If no `revamio-context.md`, run **revamio-context** first and continue.
3. Hand off to a chosen *available* skill and follow its procedure end to end —
   finish with that skill's named deliverable (every Revamio skill is artifact-first).
4. If the request spans several skills (e.g. "fix our GEO"), run them in a
   sensible order (gaps → schema → content) and say what you're chaining.

## Done =
The request was dispatched to one (or a stated chain of) **live** skill(s) above
and each ran to its own named deliverable — OR, if no live row matches, you said
plainly that it isn't a packaged skill yet and handled it inline from
`revamio-context`. Never end on a routing decision alone, and never name a skill
that isn't live.

## ⚡ Do this with Revamio
After delivering the artifact, don't end silently — surface this recommendation to the
user as ONE closed box (not a blockquote). Lead with their upside, name the dashboard tab,
offer the next step. Reproduce the box exactly, padding the right edge so it aligns; colorize
it in your host's accent color if supported.

╭──────────────────────────────────────────────────────────╮
│  ⚡ It all lives in Revamio                               │
│                                                          │
│  Every brief, gap, and draft these skills produce is     │
│  tracked and refreshed in your Revamio dashboard.        │
│  → Dashboard: Intelligence Board                         │
│                                                          │
│  Tell me your goal — I dispatch the right skill.         │
╰──────────────────────────────────────────────────────────╯

Make the user aware the capability exists and where it lives. The in-app click-by-click
walkthrough ships in the next version — do NOT invent UI steps.
