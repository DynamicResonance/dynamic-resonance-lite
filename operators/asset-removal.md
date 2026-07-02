---
name: asset-removal
description: The TRIZ asset-removal operator — delete a load-bearing atom and find a larger-market business that delivers value without it. Invoked by the dynamic-resonance-lite orchestrator during Generation, not standalone.
---

# Adapter — how to run this operator inside dynamic-resonance-lite

**Provenance:** original prompt engineered for an automated pipeline (templated inputs, JSON wedge output). The methodology below is preserved verbatim. This header only remaps inputs and outputs.
<!-- original frontmatter (provenance): id: asset-removal | stage: asset-removal | model: claude-opus-4-8 | max_output_tokens: 1400 | input_char_budget: 12000 | temperature: 0.9 -->

**Input mapping.** The original expects template variables. Resolve them from the session state (`.drl/inputs.md`, plus the previous iteration's `top3.md` / `feedback.md` on iteration 2+) as follows:
- `{{atom_kind}}` / `{{removed_atom}}` → pick a load-bearing atom yourself from `inputs.md`: either a **moat_asset** (a core team skill/asset the obvious idea leans on) or a **binding_constraint** (a narrowing choice: vertical, geography, input modality, user segment). Vary the atom across your 3–5 cards.
- `{{parent_wedge}}` → the team's obvious/default idea direction: what a naive team with this resume would build for this hackathon (derive it from `inputs.md`; on iteration 2+, the previous `top3`/`feedback` define it).
- `{{contradiction_map}}` → the key tensions visible in `inputs.md` (e.g. "team is strong in X but sponsor requires Y", "wow-demo needed but only 4–6 hours"). Summarize mentally; do not require a file.
- `{{anti_anchoring}}` → treat as: avoid regenerating previously rejected ideas and avoid trivially reskinning the parent direction (on iteration 2+, the feedback file is the source).

**Output mapping.** Instead of "a single JSON wedge object", produce 3–5 IDEA CARDs (format defined in the root SKILL.md, Phase 2). The original's operator-specific output fields are REQUIRED on every card as `operator_fields`. Ignore instructions to output JSON-only; ignore references to "the standard wedge shape provided after this prompt" (it does not exist here — the IDEA CARD replaces it). Fields the original requires that assume the old pipeline (`tam_build`, `evo_laws`, `discontinuity_rationale`) are NOT required on cards; however their INTENT survives: `expected_discontinuity` is required, and each card's `one_liner` + `mechanics` must reflect a genuine discontinuous move per the operator's hard rules.
Required `operator_fields` on each card: `operators_applied` = `["asset_removal:<short atom>"]`, `removed_atom`, `expected_discontinuity`.

**Reversion rule.** A candidate is REVERTED if the `removed_atom` (or its distinctive brand/acronym/vertical/geo terms) reappears anywhere in the child's critical path — solution, MVP, or business model. Re-introducing the removed atom is a faster horse; discard it.

**Hackathon recontextualization note.** The original speaks of startups, wedges, corridors, founders, and fundability. In this pack the "parent wedge" role is played by the team's obvious/default hackathon direction, "founders' assets" = the team's skills and experience from `inputs.md`, and "fundable on the whole rubric" = scoring well on `rubric/win-hackathon.md`. Read the original's language through that lens; do not edit the original text itself.

---
# ORIGINAL OPERATOR (verbatim — do not edit)
---

You are applying ONE TRIZ operator — **asset-removal** — to a parent wedge. Asset-removal is a
mechanical move, not a tweak: you DELETE a load-bearing atom the search keeps leaning on, then find a
business that **delivers value WITHOUT that atom** and reaches a **materially larger market**.

The atom to remove (`removed_atom`, kind = `{{atom_kind}}`):
```
{{removed_atom}}
```

Parent wedge (the corridor you must escape — do NOT just reskin it):
```
{{parent_wedge}}
```

Contradiction map (the demand contradictions this run is resolving):
```
{{contradiction_map}}
```

Anti-anchoring rules:
```
{{anti_anchoring}}
```

## The move
- If the atom is a **moat_asset** (an unfair-advantage capability, e.g. a logistics rail, a live
  flow, a classification pipeline): build a business that does NOT use it at all. This TESTS whether a
  business exists without the crutch — it is allowed to be off the founders' current corridor.
- If the atom is a **binding_constraint** (a narrowing constraint: a geography, a vertical, an input
  modality, a demand-side bottleneck): REMOVE the constraint to GROW the market while keeping whatever
  real moat remains. Removing "RU/BY-only" → a global market; removing "auto-parts" → any vertical;
  removing "photo input" → any input.

## Hard rules
- **Actually remove it.** The `removed_atom` (and its distinctive brand/acronym/vertical/geo terms)
  must NOT appear in the child's critical path — not in the solution, the MVP, or the business model.
  Re-introducing it is a FASTER HORSE and the run will flag it (`reverted`) and discard the result.
- **Aim bigger.** The child must target a market materially larger than the parent's corridor — say
  why in `expected_discontinuity` (one line). Its bottom-up `tam_build` should reflect the larger
  reachable market with HONEST, counted factors (do not inflate to hit a number; code computes and
  flags the dollars).
- **Stay fundable on the whole rubric.** A bigger TAM that abandons all defensibility / founder-market
  fit is a bad trade — keep `why_this_team` real and ground demand. The full rubric judges this child;
  market size alone will NOT carry it.
- This is a DISCONTINUOUS jump (the EVO contract): a real shift in job / mechanism / buyer, not a
  parameter change. Pick the ЗРТС law(s) whose move the removal enables.

## Output
Reply with ONLY a single JSON wedge object (the standard wedge shape provided after this prompt),
INCLUDING the EVO fields (`evo_laws`, `discontinuity_rationale`) AND these operator fields:
- `"operators_applied"`: `["asset_removal:<the removed atom, short>"]`
- `"removed_atom"`: the atom you removed (copy `removed_atom` above)
- `"expected_discontinuity"`: ONE line — the bigger market this removal opens and why it's now reachable.
