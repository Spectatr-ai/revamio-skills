# Competitor battlecard — fields by section

`revamio_get_competitors` takes a comma-separated `sections` string (default
`profiles`). The response has `sections_returned`, a `notes.ai_traffic` warning,
a `units` map, and one array per requested section.

## `profiles`
`name`, `domain`, `threat_level` + `threat_level_label`, `organic_traffic`
(visits/mo), `organic_keywords`, `intersecting_keywords`, `keyword_overlap_pct`,
`authority_score`, `avg_position`, `chatgpt_traffic`, `gemini_traffic`,
`claude_traffic`. **AI-traffic columns are nullable: null = NOT MEASURED, a
literal 0 = measured zero.** Internal IDs/timestamps are not exposed.

## `compare`
`name`, `domain`, `is_you` (your own row in the table), `competitive_score`
(0.0–1.0 composite), `visits`, `organic_traffic`, `organic_keywords`,
`avg_position`, `page_1_keywords`, `intersecting_keywords`, `seo_score`,
`geo_score`, AI-traffic fields.

## `gaps`
`cluster_name`, `primary_keyword`, `gap_score`, `search_volume`, `difficulty`,
`coverage`, `competitor_strength`, `recommended_action`, `content_angle`.
(Identified by `cluster_name` — raw cluster UUID is intentionally dropped.)

## `intersection`
Per rival domain: `competitor_domain`, `your_domain`, `total_intersecting`, and a
capped `keywords` envelope (each `keyword`, `volume`, `cpc`, `intent`,
`your_position`, `their_position`).

## `content` (gated — `content_intelligence`; 402 if not entitled)
`competitor_name`, `title`, `url`, `publish_date`, `word_count`, `content_gaps`,
`topics`.

## `alerts`
`competitor_name`, `alert_type`, `urgency`, `title`, `description`,
`detected_at`, `status`, `recommended_action`, `source_url`.

## `revamio_get_competitor_moves` (separate tool, last 90 days)
`competitor_name`, `competitor_domain`, `event_type` + `event_type_label` +
`event_type_valid`, `title`, `description`, `event_date`, `partner_name`,
`geography`, `strategic_implication`, `recommended_action`, `source_url`,
`status`. `event_type` ∈ {NEW_PARTNERSHIP, GEO_EXPANSION, NEW_PRODUCT, KEY_HIRE}.
`strategic_implication` and `recommended_action` are **null until the events
enrichment pass runs** — never infer them; the tool's `note` counts how many are
still pending.

## Graceful degradation
- `content` → 402: omit the section, note "content intelligence not on plan".
- moves with null strategy: list the move, mark response "pending analysis".
- any section empty / not yet scanned: say so; never fabricate rows.
