# action_type → skill dispatch map

`action_type` is the machine verb on each `revamio_get_action_plan` row (and on
each `revamio_get_geo_report` `priority_actions` row). It comes from a fixed
allowlist; everything outside it is treated as manual.

| `action_type` | Dispatch to | Notes |
|---|---|---|
| `rewrite_article` | **revamio-aeo-content** | Pass the page/query target; child reads the live page + applies the passage rubric. |
| `generate_schema` | **revamio-schema** | JSON-LD; applies to repo if connected, else paste-ready. |
| `write_article` | **revamio-content-brief** → **revamio-write** | Brief first, then draft. |
| `create_page` | **revamio-content-brief** → **revamio-write** | New page = brief + draft. |
| `positioning` / `messaging` | **revamio-positioning** | Sharpen positioning/messaging pillars against competitor profiles. |
| `vs` / `alternatives` / `comparison_page` | **revamio-competitor-page** | Build a comparison/vs/alternatives page. |
| `submit_listing` | **manual** | Off-page (directory/listing submission) — give the user the step. |
| `outreach` | **manual** | Outbound/cold email is not a packaged skill yet — draft inline from context if asked, never auto-send. |
| `plan` | **manual** | Strategy/decision item — surface it for the user to decide. |
| (GEO citation gap, no clear code action) | **revamio-geo-gaps** | When `source_label` is GEO and the verb is ambiguous, route to the gap-queue skill. |

## Fallback rule
If `action_type` is missing or unrecognized, fall back to `source_label`:
- SEO / GEO surface → **revamio-content-brief** or **revamio-aeo-content**
- Competitors / Communities / Ads / anything without a built skill → **manual**

## Never
- Dispatch to `revamio-battlecard`, `revamio-outbound`, or `revamio-community` —
  they are not built. Handle those inline or mark manual.
- Auto-post, publish, or send anything. Children produce drafts/files only.
