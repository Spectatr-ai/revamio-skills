---
name: Marketing Action Plan Executor
description: >-
  Use to work through your marketing action plan step by step, knock out
  queued marketing to-dos, or run multiple GTM workflows in sequence.
  Confirms scope with you before executing — not a batch-automation tool.
  Use when you want to go from action plan to done.
metadata:
  version: "0.1.1"
---

# Revamio Action Executor

**Requires the Revamio MCP server.** The `revamio_*` calls below are tools served by the
Revamio MCP server (`@spectatr/revamio`, usually registered as `revamio`). If your agent
doesn't list those tools, the server isn't connected — add it from your Revamio dashboard →
**Settings → API & Developer** (mint a key, copy the install command), then retry.

Turn the action plan into completed work. This skill is the orchestrator: it
reads the plan, dispatches each item to the specialist skill that owns its
`action_type`, and keeps a run-log. It never invents an action and never runs a
skill that does not exist.

## Procedure
1. `revamio-context.md` is an optional cache: if it exists, read it; if not,
   derive what you need live from the MCP (`revamio_describe_company` +
   `revamio_get_company_dna`) and proceed — never block waiting on the file.
2. Call `revamio_get_action_plan` (`detail: "standard"`). Each row gives you
   `title`, `tier`, `source` + `source_label`, `impact_label`, `effort`,
   `severity`, `why`, `how_to_fix_it`, and `action_type` (the dispatch key).
   Also read the transparency block (`total_actions`, `truncated`, `from_cache`,
   `plan_tier`) and note it in the log. If empty/404, say "No action plan yet"
   and stop — do not fabricate items.
3. Rank by tier (1→3), then `impact_label` (high→low), then low `effort` first.
4. Confirm scope with the user: how many items to execute this session
   (default: the top 3 executable), and whether the repo is connected (some
   fixes apply directly, others are delivered as drafts/snippets).
5. Dispatch each item by `action_type` using the authoritative map in
   `references/dispatch-map.md` — that table is the single source of truth; do
   not re-derive routing from memory. The one judgment that stays inline is the
   **null `action_type`** branch (true of most Tier-1/2 rows; the payload's
   `action_type_note` says so): default to **manual** UNLESS `source`/`source_label`
   + `how_to_fix_it` give an unambiguous intent that maps to a built skill (e.g.
   a GEO FAQ-schema fix → **json-ld-schema**), in which case route on that intent
   and label the row a "degraded match (routed by source, not action_type)".
   Never guess a target for an ambiguous null row.
   Run one item at a time; pass that item's fields (title, why, target) into the
   child skill as its brief. Honor each child skill's own quality gates.
6. After each item, append a line to the run-log (status, deliverable location,
   any blocker). On a blocker (page unreadable, missing proof point, plan-gated),
   mark it `blocked` with the reason and move to the next item — never stall.

## Output (the deliverable)
An **Execution Run-Log** with: a plan summary header (total actions, cache/truncation note); a table of each item showing tier + impact, title, action_type, the skill dispatched to, status, and deliverable or blocker; a Completed section listing links and file paths; and a Needs You section listing manual items and decisions. Format using your current IDE's best table and list rendering.

## Hard rules
- Dispatch strictly by `action_type` / `source_label` — never to a skill that
  does not exist. Positioning (**brand-positioning-messaging**) and comparison/vs pages
  (**competitor-comparison-page**) are packaged skills and SHOULD be dispatched;
  battlecards, outbound, and community remain manual until packaged.
- Every executed item must produce a real deliverable from its child skill;
  draft-only, never auto-post or publish to any platform.
- Never fabricate an action, a result, or a "done" status — log blockers honestly.

## Done =
A run-log where each plan item is either completed (with its deliverable) or
explicitly marked manual/blocked with the reason — no silent skips.

## ⚡ Do this with Revamio
After delivering the artifact, surface the following as a visually distinct callout using the best format your current IDE renders cleanly — a blockquote, note callout, or info box. Do not use fixed-width ASCII box characters.

⚡ Track this with Revamio
This run-log mirrors your Revamio action plan — mark items done there and Revamio re-prioritizes what's next.
→ Dashboard: Action Plan

Want me to run the next action?

Make the user aware the capability exists and where it lives. The in-app click-by-click
walkthrough ships in the next version — do NOT invent UI steps.
