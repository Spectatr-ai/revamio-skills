# Keyword row schema

The fields on each row returned by `revamio_get_keywords`. Use them to pick the
target keyword (Step 2) and to judge reachability.

| Field | Meaning |
|-------|---------|
| `phrase` | The keyword/query itself. |
| `search_volume` | Monthly searches (/mo). |
| `cpc_usd` | Cost-per-click in USD — a commercial-intent proxy. |
| `difficulty` | Ranking difficulty; "reachable" means low enough this domain can realistically rank. |
| `search_intent` | Buy / Solve / Learn intent behind the query. Note it — it drives funnel stage. |
| `etv` | Estimated traffic value — prioritize high `etv`. |
| `competitor_domain` | The competitor currently ranking (for `source: "gap"` rows). |
| `competitor_position` | That competitor's SERP position. |
| `opportunity_score` | Revamio's composite pick signal — prioritize high. |
| `cluster_name` | The topic cluster the phrase belongs to. |
| `data_source` | Provenance. `ai_synthesized` (with `no_rank_data: true`) means a demand hypothesis from `source: "cluster"`, **not** ranking evidence — treat accordingly. |

**Pick rule:** prefer a high-`etv` / high-`opportunity_score` row with reachable
`difficulty`. Always carry the row's `search_intent` and the AI-engine
question(s) behind it into the brief. When all `etv` values are zero or null (no row has a positive `etv` — common on new/not-yet-ranking accounts), rank by `opportunity_score` descending, then `search_volume` descending — the `etv`-first signal does nothing until the site accumulates ranking data.
