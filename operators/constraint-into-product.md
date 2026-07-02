---
name: constraint-into-product
description: The TRIZ constraint-into-product operator — invert the pain by taking the constraint that makes the domain hard for everyone and making solving it the product and the moat. Invoked by the dynamic-resonance-lite orchestrator during Generation, not standalone.
---

# Adapter — how to run this operator inside dynamic-resonance-lite

**Provenance:** original prompt engineered for an automated pipeline (templated inputs, JSON wedge output). The methodology below is preserved verbatim. This header only remaps inputs and outputs.
<!-- original frontmatter (provenance): id: constraint-into-product | stage: constraint-into-product | model: claude-opus-4-8 | max_output_tokens: 1400 | input_char_budget: 12000 | temperature: 0.9 -->

**Input mapping.** The original expects template variables. Resolve them from the session state (`.drl/inputs.md`, plus the previous iteration's `top3.md` / `feedback.md` on iteration 2+) as follows:
- `{{turned_constraint}}` → pick the binding constraint yourself from `inputs.md`: something that makes this hackathon/domain hard for EVERYONE (a sponsor-tech limitation, a data-access barrier, a trust/verification gap, or the 4–6h build window itself if it genuinely is the shared pain). Vary across cards.
- `{{parent_wedge}}` → the team's obvious/default idea direction: what a naive team with this resume would build for this hackathon (derive it from `inputs.md`; on iteration 2+, the previous `top3`/`feedback` define it).
- `{{moat_assets}}` → the team's skills and experience from `inputs.md`.
- `{{contradiction_map}}` → the key tensions visible in `inputs.md` (e.g. "team is strong in X but sponsor requires Y", "wow-demo needed but only 4–6 hours"). Summarize mentally; do not require a file.
- `{{anti_anchoring}}` → treat as: avoid regenerating previously rejected ideas and avoid trivially reskinning the parent direction (on iteration 2+, the feedback file is the source).

**Output mapping.** Instead of "a single JSON wedge object", produce 3–5 IDEA CARDs (format defined in the root SKILL.md, Phase 2). The original's operator-specific output fields are REQUIRED on every card as `operator_fields`. Ignore instructions to output JSON-only; ignore references to "the standard wedge shape provided after this prompt" (it does not exist here — the IDEA CARD replaces it). Fields the original requires that assume the old pipeline (`tam_build`, `evo_laws`, `discontinuity_rationale`) are NOT required on cards; however their INTENT survives: `expected_discontinuity` is required, and each card's `one_liner` + `mechanics` must reflect a genuine discontinuous move per the operator's hard rules.
Required `operator_fields` on each card: `operators_applied` = `["constraint_into_product"]`, `turned_constraint`, `expected_discontinuity`.

**Reversion rule.** A candidate is REVERTED if the constraint stays background pain rather than becoming the product — i.e. the solution merely mentions the constraint as a feature instead of making solving it at scale the core value proposition. The constraint must be named explicitly in the solution; otherwise it is a faster horse and is discarded.

**Hackathon recontextualization note.** The original speaks of startups, wedges, corridors, founders, and fundability. In this pack the "parent wedge" role is played by the team's obvious/default hackathon direction, "founders' assets" = the team's skills and experience from `inputs.md`, and "fundable on the whole rubric" = scoring well on `rubric/win-hackathon.md`. Read the original's language through that lens; do not edit the original text itself.

---
# ORIGINAL OPERATOR (verbatim — do not edit)
---

You are applying ONE TRIZ operator — **constraint-into-product** — to a parent wedge. INVERT the pain:
take the binding constraint (the thing that makes this hard for EVERYONE in the corridor — sanctions,
customs complexity, broken payment rails) and make **solving it the product and the moat**.

The CONSTRAINT to turn into the product:
```
{{turned_constraint}}
```

Parent wedge (the constraint is currently background pain here — make it the CORE value):
```
{{parent_wedge}}
```

The founders' MOAT ASSETS that make solving this constraint credible:
```
{{moat_assets}}
```

Contradiction map (the technical contradictions + binding constraints this run resolves):
```
{{contradiction_map}}
```

Anti-anchoring rules:
```
{{anti_anchoring}}
```

## The move
- Customs complexity is the pain → "**compliance-as-a-service**: a guaranteed legal, duty-paid landed
  price into any restricted corridor." Sanctions are the pain → the sanctioned-corridor settlement layer.
  Broken payment rails are the pain → the cross-border payment guarantee. The thing everyone hates becomes
  the thing they PAY you for — and the founders' assets (customs pipeline, rail, flow) are why you can
  guarantee it.

## Hard rules
- **The constraint must BE the product.** The child's value proposition / solution is SOLVING the
  constraint at scale — not a buy-and-ship agent that merely mentions the constraint as a feature. If the
  constraint stays background pain, that's a FASTER HORSE and the run will flag it (`reverted`) and
  discard it. Name the constraint explicitly in the solution.
- **Bigger market.** Solving a constraint EVERYONE faces serves the whole corridor, not one ICP — the
  `tam_build` should reflect that with HONEST counted factors (code computes the dollars).
- **Stay fundable on the WHOLE rubric** (real founder-market fit, grounded demand). DISCONTINUOUS jump
  (the EVO contract): a real shift in job + mechanism.

## Output
Reply with ONLY a single JSON wedge object (the standard wedge shape provided after this prompt),
INCLUDING the EVO fields (`evo_laws`, `discontinuity_rationale`) AND these operator fields:
- `"operators_applied"`: `["constraint_into_product"]`
- `"removed_atom"`: null
- `"turned_constraint"`: the constraint you turned into the product (copy `turned_constraint` above)
- `"expected_discontinuity"`: ONE line — the bigger market that solving this constraint opens.
