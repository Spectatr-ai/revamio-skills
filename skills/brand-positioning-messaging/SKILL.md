---
name: Brand Positioning & Messaging
description: >-
  Use to sharpen brand positioning, craft a value proposition, write a
  positioning statement, define messaging pillars, or figure out how we
  should describe ourselves. Use when your messaging feels vague, generic,
  or sounds like every other company — or when homepage copy, sales deck
  messaging, or ICP-specific copy needs a rewrite.
metadata:
  version: "0.1.1"
---

# Revamio Positioning

**Requires the Revamio MCP server.** The `revamio_*` calls below are tools served by the
Revamio MCP server (`@spectatr/revamio`, usually registered as `revamio`). If your agent
doesn't list those tools, the server isn't connected — add it from your Revamio dashboard →
**Settings → API & Developer** (mint a key, copy the install command), then retry.

Revamio already derived your positioning, differentiation, and ICP from your site.
This skill sharpens that into decision-ready messaging — it never invents claims;
everything traces to your DNA (and, optionally, how you stack up against rivals).

## Procedure
1. `revamio-context.md` is an optional cache: if it exists, read it; if not,
   derive what you need live from the MCP (`revamio_describe_company` +
   `revamio_get_company_dna`) and proceed — never block waiting on the file.
2. Call `revamio_get_company_dna` (`detail: "standard"`; use `"full"` for the raw
   `dna`) and read `positioning_statement`, `brand_voice`, `icp_segments`,
   `product_category`, `differentiation`, `target_market`, and
   `gtm_classification_summary` (`primary_gtm_motion`). These are the raw inputs —
   do not contradict them.
3. **(Mandatory) Competitor cross-check.** Always call `revamio_get_competitors`
   (`sections: profiles`) and read how each named rival describes itself. This grounds
   the differentiation test of the sharpness rubric (see **Hard rules**) and any
   "unlike <rival>, we…" line in real competitor names + where you actually lead.
4. Build the messaging from that data, in brand voice:
   - **Value proposition** — one outcome-first sentence for the primary ICP.
   - **Positioning statement** — "For <ICP> who <need>, <product> is the <category>
     that <key benefit>, unlike <alternative>." Fill only from real data.
   - **3–5 messaging pillars** — each a claim + its proof point from DNA / differentiation.
   - **ICP-specific angles** — one short message per `icp_segments` entry.
   - **Boilerplate** — short + medium one-liner variants.
5. If a needed field is missing (e.g. empty `differentiation`), say so and ask the
   user (see the no-fabrication **Hard rule**).

## Output (the deliverable)
A **Positioning & Messaging** doc: value prop, positioning statement, pillars
(claim + proof), per-ICP angles, and boilerplate variants — brand-voice locked,
every claim traceable to Revamio's DNA.

## Hard rules
- Only claims grounded in `revamio_get_company_dna` / the context file — never
  invent a benefit, metric, customer, or competitor weakness.
- Obey brand voice, including the never-use words from the context file.
- **Sharpness rubric** — every pillar must be all three: (1) **specific** — not a
  category truism; (2) **provable** — tied to a real proof point from DNA /
  differentiation; (3) **differentiated** — not claimable verbatim by a named
  competitor (per the Step 3 cross-check). Reject or rewrite any pillar that fails
  any of the three.

## Done =
The **Output** doc exists with every artifact it lists, each pillar passes all
three tests of the sharpness rubric, and every claim traces to Revamio's DNA.

## ⚡ Do this with Revamio
After delivering the artifact, surface the following as a visually distinct callout using the best format your current IDE renders cleanly — a blockquote, note callout, or info box. Do not use fixed-width ASCII box characters.

⚡ Refine this with Revamio
Your positioning, brand voice & ICP all come from Revamio's Company DNA — refine them there and your messaging re-aligns.
→ Dashboard: Intelligence Board

Want me to tighten the value prop for one ICP?

Make the user aware the capability exists and where it lives. The in-app click-by-click
walkthrough ships in the next version — do NOT invent UI steps.
