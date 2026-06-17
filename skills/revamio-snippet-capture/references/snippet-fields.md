# Snippet capture — fields & rubric

## Fields from `revamio_get_snippet_opportunities` (detail=standard)

### `featured_snippets[]`
| Field | Meaning |
|---|---|
| `query` | the search query the snippet answers |
| `opportunity_type` / `opportunity_type_label` | `unowned` (nobody owns it) or `winnable` (a weak owner you can displace) |
| `priority_score` / `priority_label` | 0.0–1.0 ranking (higher = act first) |
| `search_volume` | estimated searches/month (real) |
| `search_volume_estimate` | estimate flag/value — treat as approximate |
| `current_owner` | who holds the snippet now (may be null = unowned) |
| `cluster_name` | the keyword cluster this belongs to (group by it) |
| `gap_description` | why it's winnable |
| `estimated_impact` | expected upside text |

Only `unowned`/`winnable` rows are returned — there is no "owned" noise.

### `paa_boxes[]`
| Field | Meaning |
|---|---|
| `question` | the People-Also-Ask question |
| `winnable` | bool — you can realistically capture it |
| `brand_appears` | bool — whether you already show up |
| `status_label` | human label combining winnable + brand_appears |
| `priority_score` / `priority_label` | 0.0–1.0 ranking |
| `search_volume` / `search_volume_estimate` | est. searches/month |
| `current_answer_source` | who currently answers it (may be null) |
| `cluster_name` | the keyword cluster (group by it) |

There is no `opportunity_type` on PAA rows — use `winnable`/`brand_appears`.

The top-level `units` block documents the meaning of `search_volume` and
`priority_score`; surface them with those units.

## Rewrite rubric (per box)
- **Answer-first**: the first sentence IS the answer; support after.
- **40–55 words** for a paragraph snippet/PAA; a tight numbered list for
  "how to" queries; a 2-column comparison for "X vs Y" queries.
- **Self-contained**: reads correctly lifted out of the page (no "as above").
- **Brand voice**: tone + vocabulary from `revamio-context.md`; obey the
  never-use list.
- **One real fact max**, only if it exists in the context file — never fabricate
  a statistic, source, customer, or number.
- **Match the phrasing** of `query`/`question` in the heading so the engine
  maps the block to the box.
