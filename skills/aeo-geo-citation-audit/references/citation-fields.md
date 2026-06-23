# Citation & GEO-report fields

The field schema, null-handling, and rank semantics for the two calls Step 2
makes. The Buy/Solve/Learn intent taxonomy lives inline in SKILL.md (Step 3) —
it's needed on every path.

## `revamio_get_ai_citations`

Default `include: "gaps"` returns only uncited rows where a competitor is named.
Pass `include: "all"` for the full grid, or `engine` (e.g. `"chatgpt"`) to focus
one engine.

Returns:
- `summary`
- `engines[]` — per engine: `engine_display_name`, `citation_rate_pct`,
  `queries_cited`, `queries_checked`, `status`
- `total_gaps`
- `citations.items[]` — each:
  | Field | Meaning |
  |---|---|
  | `query_text` | the query checked against the engine |
  | `engine` | which AI engine (chatgpt, perplexity, …) |
  | `brand_mentioned` | bool — were you cited |
  | `is_gap` | bool — uncited row |
  | `competitors_mentioned[]` | who was cited in your place (the real "who beat you") |
  | `query_intent` / `query_intent_label` | one of `buying_signal`, `comparison`, `problem_query`, `category_query`, `brand_query` |
  | `recommendation_strength` | label form of the rank below |
  | `recommendation_strength_rank` | 0–4 (see rank semantics) |
  | `brand_mention_position` | where you appeared, if at all |

### Rank semantics (`recommendation_strength_rank`)
0–4 scale: `0` = not mentioned … `4` = recommended-first. **Lower rank is more
actionable** — order gaps weakest-first within a tier. A `0` (you're invisible)
is a bigger, more closable gap than a `3` (you're mentioned but not led with).

### Null-handling
- If either call returns 404 / no report, stop with the "run a GEO scan" message.
- If `competitors_mentioned` is empty for a query the user specifically cares
  about, fall back to a live WebFetch of that engine — but only then, not by default.

## `revamio_get_geo_report`

Call with `detail: "standard"`, `include_actions: true` for overall score +
grade, `score_components`, and `priority_actions`.

### `score_components` null-handling
Can be **null** on pre-v2 rows. When present it has exactly these keys (each with
a `label` + `score`):
- `recommendation_quality`
- `description_accuracy`
- `answer_box_ownership`
- `technical_discoverability`

There are **NO** separate schema / content / social_proof sub-scores — don't
invent them. When `score_components` is null, render it as "unavailable" in the
deliverable header.

**Do not look for `ai_engine_results[]` or `citation_gaps[]` in the geo_report response** — those are internal backend names not exposed by the MCP. The per-engine citation data lives exclusively in `revamio_get_ai_citations`'s `engines[]` + `citations.items[]`.
