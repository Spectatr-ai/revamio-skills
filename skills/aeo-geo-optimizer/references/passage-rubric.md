# Passage rubric for AI citation

Grounded in the Princeton GEO study (KDD 2024) and 2026 citation research.

## Structure
- **Answer-first.** State the answer in the first sentence of a block, then
  support it. AI engines extract the lead.
- **Self-contained blocks, 134–167 words.** Each block must make sense lifted
  out of the page (no "as mentioned above", no dangling pronouns).
- **Question-shaped headings.** H2/H3 phrased as the user's actual question
  ("How does X handle Y?") — matches AI query fan-out.
- **Front-load.** Put the most citable material in the first ~30% of the page.
- **Short, definitional openers** for snippet/PAA targets: 40–55 words.

## Citation boosters (apply per key block)
Lift figures below are benchmark estimates from the Princeton GEO study (KDD
2024) under controlled conditions — directional, not guaranteed for every site.
Apply them as good practice, and don't quote the percentages to users as proven.
- **One statistic** with a source (~+33% in the study).
- **One quotation** from a named expert/customer (~+30%).
- **Cite sources** inline (~+27%).
- **Fluent, specific language** — concrete nouns and numbers, not adjectives.

## Anti-patterns (remove)
- Walls of text with no heading anchors.
- Pronoun-heavy openings ("It does this...").
- Invented stats/quotes — never fabricate; if no real number exists, ask the
  user (gate via `marketing-context` proof question) or omit the claim.
- Keyword stuffing, CTA overload, thin boilerplate.

## Brand voice
Pull tone + vocabulary + never-use words from `revamio-context.md`. Citability
must not flatten the voice — apply the humanizer pass from `brand-voice-writer`'s
`references/` if output reads AI-generated.

## Per-engine nuance
- **Perplexity** weights freshness — update `dateModified`, keep facts current;
  it selects at passage level.
- **ChatGPT** favors authority + comprehensive coverage (longer, sourced).
- **Claude** favors primary sources and transparent methodology.
- **Google AI Overview / AI Mode** are separate surfaces — don't assume one
  win transfers to the other.
