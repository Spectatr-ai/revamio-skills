# Signal engage — fields, ranking & reply rubric

## `revamio_get_signals` fields
Header: `summary`, `total`, `high_intent_count`, `units`.
Each row (standard): `id`, `platform`, `signal_type`, `community_name`,
`source_url`, `raw_text`, `author_handle`, `buyer_intent` (0–100; ≥70 high,
40–69 medium, <40 low) + `buyer_intent_label`, `icp_match` (0–100),
`competitor_mentioned`, `matched_keywords[]`, `keyword_match_count`,
`priority_score` (0.0–1.0), `confidence`, `detected_at`,
`respond_by_remaining_minutes` (minutes until SLA closes; negative = overdue),
`is_overdue`, `engagement_velocity`.
`detail: "full"` adds: `response_angle`, `poster_context`,
`top_comment_preview`, `post_upvotes`, `post_comment_count`.

## `revamio_get_communities` ledger (the promo gate)
Each `ledger[]` row: `community_name`, `contributions`, `product_mentions`,
`actual_ratio`, `ratio_status` (`blocked` / `warning` / `healthy`),
`ratio_status_remediation` (always present — the actionable text),
`contributions_needed_for_mention`, `can_mention_product`.

## Ranking
1. `priority_score` descending.
2. Within a tie, `is_overdue` true first, then lowest
   `respond_by_remaining_minutes` (closest to expiry).
3. Prefer higher `icp_match` when intent + SLA are equal.

## Reply rubric
- **Value-first**: directly answer the poster's question/pain (from `raw_text`,
  or `response_angle` if present) before anything else.
- **Promo gate (hard)**: include a product mention ONLY when
  `can_mention_product` is true. If false, write a pure-value reply and surface
  `ratio_status_remediation` + `contributions_needed_for_mention` so the user
  knows the gate and how to unlock it.
- **Brand voice**: tone + vocabulary from `revamio-context.md`; obey the
  never-use list; sound human, not like an ad.
- **No fabrication**: only facts in the context file; never invent a stat,
  result, customer, feature, or competitor comparison.
- **Length**: match the platform — short and conversational on Reddit/X, never a
  copy-paste wall.

## Draft-only
This skill produces drafts the user reviews and posts themselves. It never calls
a write/post tool (the MCP is read-only) and never schedules or submits.
