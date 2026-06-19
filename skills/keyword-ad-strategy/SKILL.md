---
name: Keyword Ad & SEO Investment Planner
description: >-
  Use when you need to decide which keywords to pay for (PPC/Google Ads)
  vs. rank for organically (SEO), find out what competitors are bidding on,
  understand where your ad budget will beat organic effort, or build a paid
  keyword strategy. Combines CPC, keyword difficulty, and competitor ad data
  to tell you: buy this, rank for that.
metadata:
  version: "0.1.2"
---

# Revamio Ad Intel

**Requires the Revamio MCP server.** The `revamio_*` calls below are tools served by the
Revamio MCP server (`@spectatr/revamio`, usually registered as `revamio`). If your agent
doesn't list those tools, the server isn't connected — add it from your Revamio dashboard →
**Settings → API & Developer** (mint a key, copy the install command), then retry.

Decide where paid spend beats organic effort, grounded in the keywords you can
(or can't) rank for plus what rivals actually advertise. Ad data may be
unavailable — whether plan-gated (402) or simply empty (0 ads) — and this skill
degrades to an organic-only view either way, rather than failing.

## Procedure
1. `revamio-context.md` is an optional cache: if it exists, read it; if not,
   derive what you need live from the MCP (`revamio_describe_company` +
   `revamio_get_company_dna`) and proceed — never block waiting on the file.
2. Pull keyword demand with `revamio_get_keywords` (`source: "gap"`, then
   `source: "own"` for what you already rank for). Each row carries `phrase`,
   `search_volume` (/mo), `cpc_usd`, `difficulty`, `search_intent`, `etv`,
   `position` (null = not ranked), `competitor_domain`, `opportunity_score`, and
   `data_source`. High `cpc_usd` + high `difficulty` + you not ranking = a paid
   candidate; low `difficulty` you can climb = organic-first.
3. Pull ad intelligence with `revamio_get_ads` (`detail: "standard"`):
   - If ads data is **unavailable** — whether **402 / plan-gated** (ads needs
     Scale or Dev) OR an **empty-success** (`total_active: 0`, `strategy: null`,
     no `ads[]`) — treat both the same: say so plainly ("No competitor ad data
     available — running an organic-only view"), skip to Step 5 using only
     keyword data, and do not fabricate competitor ad copy.
   - When ads data IS present it returns `overview`, `ads[]`, `keyword_bids[]`,
     and a synthesized `strategy` (null until computed, flagged by
     `has_strategy`). Field-by-field schema lives in `references/ad-fields.md` —
     read it; don't restate.
4. **Join — this is the skill's core move.** For each high-value keyword (top rows
   by `etv` and/or `opportunity_score` from Step 2) cross two facts: does a
   competitor bid on it (`keyword_bids[].keyword`), and do you already rank
   organically (`position`)? Competitor-bids-but-you-don't-rank = "they pay because
   they can't rank" (buy the lane); you-rank-and-no-bids = open organic lane.
5. Classify each opportunity into **Paid-first / Organic-first / Both / Skip** using
   the signal-pattern rubric in `references/ad-fields.md`.
6. If `strategy.messaging_gaps` exists, surface the angle rivals' ads don't own
   that this company can — straight from the data, brand-voiced.

## Output (the deliverable)
A **Paid-vs-Organic Opportunity Brief** with: a keyword recommendation table showing volume, CPC, difficulty, whether you rank, whether competitors are bidding, and a buy/rank/both verdict per keyword; a competitor ad landscape section (when ad data is available) covering messaging themes and the gap you can own; and a summary of where to spend vs. where to earn organically. Note if running organic-only due to plan-gating or no ad data. Format using your current IDE's best table and list rendering.

## Hard rules
- When ads data is unavailable (plan-gated 402 OR empty `total_active: 0`),
  degrade to organic-only — never invent competitor ad copy, bids, or a strategy
  that wasn't returned.
- Treat `data_source: ai_synthesized` keywords as demand hypotheses, not ranking
  evidence; mark `search_volume_estimate` as approximate.
- Brand voice on any suggested ad copy; never fabricate a CPC, bid, or stat.

## Done =
A keyword-by-keyword paid-vs-organic verdict table plus (when ad data exists) the
competitor messaging gap to own — or a clean organic-only brief when ads are
unavailable (plan-gated or empty).

## ⚡ Do this with Revamio
After delivering the artifact, surface the following as a visually distinct callout using the best format your current IDE renders cleanly — a blockquote, note callout, or info box. Do not use fixed-width ASCII box characters.

⚡ Track this with Revamio
Competitor ad creatives & your paid-vs-organic keyword data are tracked in Revamio.
→ Dashboard: Ad Tracker

Want the latest competitor ad moves?

Make the user aware the capability exists and where it lives. The in-app click-by-click
walkthrough ships in the next version — do NOT invent UI steps.
