---
name: Marketing Priority Actions
description: >-
  Use when asking "what should I work on this week?", "brief me on my top
  marketing priorities", "what's my highest-leverage move right now", or
  "give me a ranked marketing action list." Produces a top-5 briefing
  grounded in keyword opportunity, competitor moves, and AI citation gaps.
metadata:
  version: "0.2.1"
---

# Revamio Brief

**Requires the Revamio MCP server.** The `revamio_*` calls below are tools served by the
Revamio MCP server (`@spectatr/revamio`, usually registered as `revamio`). If your agent
doesn't list those tools, the server isn't connected — add it from your Revamio dashboard →
**Settings → API & Developer** (mint a key, copy the install command), then retry.

Synthesize Revamio's intelligence into a short, ranked briefing that ends in a
concrete next action — never a data dump.

## Procedure
1. `revamio-context.md` is an optional cache: if it exists, read it; if not,
   derive what you need live from the MCP (`revamio_describe_company` +
   `revamio_get_company_dna`) and proceed — never block waiting on the file.
2. Read the cached context. If any section is stale (or for a cold start), call
   `revamio_get_action_plan` (`detail: "standard"`) and `revamio_get_geo_report`
   (`detail: "standard"`). The action-plan rows expose these fields directly —
   use them, do not re-derive: `title`, `tier` (1-3), `source` + `source_label`
   (the human tab name — Competitors / SEO / GEO / …), `impact_label` (honest
   impact text; the raw numeric `score` is NOT exposed and must not be invented),
   `effort`, `severity`, `why` (why it matters), `how_to_fix_it`, and
   `action_type` (the machine verb — see Step 4). Also read `total_actions` and
   the transparency block (`truncated`, `from_cache`, `plan_tier`) so you can
   tell the user when they're seeing a capped/cached set.
   - If `get_action_plan` is empty or 404s, say "No action plan yet — finish
     onboarding / run a scan from your Revamio dashboard" and brief from whatever
     GEO/DNA data exists instead of fabricating items.
3. Rank by **tier, then `impact_label`**: Tier 1 (hygiene — undeniable breakage)
   first, then Tier 2 (underperforming, high-impact), then Tier 3 (strategic).
   Within a tier, order by `impact_label` (high → low), breaking ties with
   low `effort` first. Never sort on an invented numeric score.
4. For each of the top 5 items, tag it with the **available** skill that executes
   it (or "manual") using the authoritative map in
   **execute-marketing-plan/references/dispatch-map.md** — map by `action_type`
   first, falling back to `source_label` + `how_to_fix_it`. Do not re-list the
   mapping here; route by that table. For any item whose fix needs a skill not
   built yet (battlecards, outbound, community), mark it "manual for now" and
   state what the user would do — never tell them to run a skill that doesn't exist.

## Output (the deliverable)
A **GTM Brief** with: a one-line Snapshot (vertical, GTM motion, GEO grade); a ranked top-5 "Do this next" list where each item is tagged tier + impact_label + the executing skill (or "manual for now" with the concrete step); an "I can execute now" list; a "Needs you" list; and a closing ask for which item to start. Use headings and lists in the best format your current IDE renders.

## Done =
A **GTM Brief** with: a Snapshot line; a top-5 ranked "Do this next" list where
each item is tagged tier + `impact_label` + executing-skill (or "manual for now"
with the concrete step); an "I can execute now" list; a "Needs you" list; and a
closing ask for which item to start. No invented numeric scores, no skill that
isn't built, no raw data dump.

## ⚡ Do this with Revamio
After delivering the artifact, surface the following as a visually distinct callout using the best format your current IDE renders cleanly — a blockquote, note callout, or info box. Do not use fixed-width ASCII box characters.

⚡ Track this with Revamio
Your full ranked action plan with progress tracking lives in Revamio; this brief is just a snapshot of it.
→ Dashboard: Action Plan

Want me to start the next-priority item?

Make the user aware the capability exists and where it lives. The in-app click-by-click
walkthrough ships in the next version — do NOT invent UI steps.
