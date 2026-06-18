# Ad-intel fields & verdict rubric

## `revamio_get_keywords` row (the keywords half)
`phrase`, `data_source` (`dataforseo` real | `ai_synthesized` demand-hypothesis),
`cluster_name`, `position` (Google rank; null = not ranked), `search_volume`
(/mo), `cpc_usd` (USD), `difficulty` (0–100), `search_intent`, `etv` (est.
traffic value USD/mo), `url`, `competitor_domain`, `competitor_position`,
`opportunity_score`, `no_rank_data`.

## `revamio_get_ads` (the ads half — Scale/Dev gated)
- `overview` — `total_active`, `competitor_ads`, `own_ads`, `keywords_tracked`.
- `has_strategy` (bool) — whether a synthesized conclusion exists.
- `ads[]` — `advertiser_domain`, `advertiser_name`, `is_own_ad`, `source`,
  `platform`, `ad_format`, `headline`, `body_text`, `call_to_action`,
  `landing_url`, `is_active`, `started_at`, `messaging_theme`, `sentiment_tone`,
  `ai_summary`.
- `keyword_bids[]` — `keyword`, `advertiser_domain`, `is_own_domain`,
  `avg_position`, `times_seen`, `first_seen_at`, `last_seen_at`.
- `strategy` — `landscape_summary`, `competitor_profiles[]`, `messaging_gaps[]`,
  `recommendations[]` (or **null** until synthesized; check `has_strategy`).

Ad data can be **unavailable** two ways, and both degrade identically: a **402**
means ad tracking isn't on the user's plan (Scale or internal Dev only); an
**empty-success** (`total_active: 0`, `strategy: null`, no `ads[]`) means the
plan allows it but nothing has been tracked yet. In either case, degrade to a
keyword-only/organic-only brief and say so — never fabricate ad copy, bids, or a
strategy.

## Verdict rubric (per keyword)
| Signal pattern | Verdict |
|---|---|
| High `cpc_usd` + high `difficulty` + `position` null + competitor in `keyword_bids` | **Paid-first** (they pay because they can't rank; buy the lane) |
| Winnable `difficulty` (≤ ~40) + low `cpc_usd` + `position` near page 1 | **Organic-first** (cheaper to earn) |
| High `search_volume` + high commercial `search_intent` + high `etv` | **Both** (defend paid + organic) |
| Low `search_volume` / informational intent / low `etv` | **Skip** |

Commercial/transactional `search_intent` weights toward paid; informational
weights toward organic content.
