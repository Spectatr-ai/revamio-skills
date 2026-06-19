# action_type → skill dispatch map

Single source of truth for routing a Revamio action-plan item to the skill that
executes it. `revamio`, `marketing-priorities`, and `execute-marketing-plan` all route
by this table — do not re-list it elsewhere; point here.

`action_type` is the machine verb on each `revamio_get_action_plan` row (and on
each `revamio_get_geo_report` `priority_actions` row). It comes from a fixed
allowlist; everything outside it is treated as manual.

| `action_type` | Dispatch to | Notes |
|---|---|---|
| `rewrite_article` | **aeo-geo-optimizer** | Pass the page/query target; child reads the live page + applies the passage rubric. |
| `generate_schema` | **json-ld-schema** | JSON-LD; applies to repo if connected, else paste-ready. |
| `write_article` | **seo-content-optimizer** → **brand-voice-writer** | Brief first, then draft. |
| `create_page` | **seo-content-optimizer** → **brand-voice-writer** | New page = brief + draft. |
| `positioning` / `messaging` | **brand-positioning-messaging** | Sharpen positioning/messaging pillars against competitor profiles. |
| `vs` / `alternatives` / `comparison_page` | **competitor-comparison-page** | Build a comparison/vs/alternatives page. |
| `submit_listing` | **manual** | Off-page (directory/listing submission) — give the user the step. |
| `outreach` | **manual** | Outbound/cold email is not a packaged skill yet — draft inline from context if asked, never auto-send. |
| `plan` | **manual** | Strategy/decision item — surface it for the user to decide. |
| (GEO citation gap, no clear code action) | **aeo-geo-citation-audit** | When `source_label` is GEO and the verb is ambiguous, route to the gap-queue skill. |

## Null / unrecognized `action_type` (most Tier-1/2 rows)
`action_type` is set only on some rows (e.g. `rewrite_article`,
`generate_schema`); on most it is null — the payload's `action_type_note` says
so. When it is null or unrecognized, fall back to `source_label` + `how_to_fix_it`:
- SEO / GEO surface with an unambiguous intent → **seo-content-optimizer** or
  **aeo-geo-optimizer** (or **json-ld-schema** for a GEO FAQ/structured-data
  fix); label the row a "degraded match (routed by source, not action_type)".
- Competitors / Communities / Ads / anything ambiguous or without a built skill
  → **manual**. Never guess a target for an ambiguous null row.

## Never
- Dispatch to `revamio-battlecard`, `revamio-outbound`, or `revamio-community` —
  they are not built. Handle those inline or mark manual.
- Auto-post, publish, or send anything. Children produce drafts/files only.
