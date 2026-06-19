# Context question bank

Each cluster maps to a context field. Run the **gap check** first; only ask the
questions whose data Revamio left missing or weak. Ask at most 3–6 total. Phrase
naturally; one cluster at a time. Write every answer back tagged `[source: user]`.

---

## Voice
**Gap check:** `get_company_dna.brand_voice` empty, generic, or no concrete
do/don't vocabulary. The never-use list pre-fills from
`brand_voice.avoidance_list` — only ask the never-use question below if that
array is empty or obviously thin.
- "Paste 2–3 sentences you've written that sound exactly like your brand (a
  landing-page line, a tweet, an email opener)."
- "Any words or phrases you'd never want associated with your brand?"
  (skip if `brand_voice.avoidance_list` already has them)
- "Who's the voice in the room — teacher, challenger, peer, straight-shooter?"

## ICP
**Gap check:** `icp_segments` thin, no pain quotes, no trigger.
- "In your customers' own words, what's the pain they describe before they find
  you? (A real quote from a call, review, or ticket is ideal.)"
- "What event makes someone go from 'interested' to 'I need this now'?"
- "Who is explicitly NOT a fit — who do you turn away?"

## Positioning
**Gap check:** `positioning_statement`/`differentiation` generic; no named rival.
- "When you lose a deal, who do you lose it to — and what do they say that wins?"
- "What's the one thing you do that the obvious alternative genuinely can't?"

## Proof
**Gap check:** no metric, customer name, or case study anywhere in the DNA.
- "Give me one real, citable proof point — a number, a named customer, a result.
  (I won't invent these, so brand-voiced content needs at least one.)"

## Priorities
**Gap check:** no clear current 90-day priority anywhere in the seed. (There is
no `north_star` field on DNA — do not look for one; derive priorities from the
action plan + this question.)
- "What's the single metric that, if it moved, means you're winning this
  quarter?"
- "If you could only ship one GTM thing in the next 90 days, what is it?"

## Surface / stack (only if a downstream skill needs it)
**Gap check:** asked on demand by `json-ld-schema` / `aeo-geo-optimizer`.
- "What runs your site — Next.js, WordPress, Webflow, custom? And is the repo
  open here so I can apply changes directly?"

---

## Rules
- Never ask for data already present and confident in the Revamio seed.
- Skipped question → record the field as `unknown`; never block the workflow.
- Keep the proof-point ask if nothing citable exists — it gates content quality.
- On re-runs, ask only about fields still `unknown` or flagged stale.
