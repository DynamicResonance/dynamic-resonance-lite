---
name: actor-substitution
description: The actor/ontology-substitution operator — substitute the CLASS of an actor in the value chain (or project it forward in time) to open a market saturated for today's actors. Invoked by the dynamic-resonance-lite orchestrator during Generation, not standalone.
---

# Adapter — how to run this operator inside dynamic-resonance-lite

**Provenance:** original prompt engineered for an automated pipeline (templated inputs, JSON wedge output). The methodology below is preserved verbatim. This header only remaps inputs and outputs.
<!-- original frontmatter (provenance): id: actor-substitution | stage: actor-substitution | model: claude-opus-4-8 | max_output_tokens: 1400 | input_char_budget: 12000 | temperature: 0.9 -->

**Input mapping.** The original expects template variables. Resolve them from the session state (`.drl/inputs.md`, plus the previous iteration's `top3.md` / `feedback.md` on iteration 2+) as follows:
- `{{current_actors}}` → derive the actor set of the team's default idea direction / the hackathon's domain from `inputs.md`: who decides, who pays, who is served, who mediates, who supplies, who regulates.

**Output mapping.** Instead of "a single JSON wedge object", produce 3–5 IDEA CARDs (format defined in the root SKILL.md, Phase 2). The original's operator-specific output fields are REQUIRED on every card as `operator_fields`. Ignore instructions to output JSON-only; ignore references to "the standard wedge shape provided after this prompt" (it does not exist here — the IDEA CARD replaces it). Fields the original requires that assume the old pipeline (`tam_build`, `evo_laws`, `discontinuity_rationale`) are NOT required on cards; however their INTENT survives: `expected_discontinuity` is required, and each card's `one_liner` + `mechanics` must reflect a genuine discontinuous move per the operator's hard rules. The original's output contract speaks of "visions"; here each vision materializes as one IDEA CARD whose `one_liner` carries the vision thesis.
Required `operator_fields` on each card: `operators_applied` = `["actor_substitution"]`, `substituted_actor` (the full structured object exactly as the original specifies: `axis` / `from` / `to` / `from_side` / `to_side` / `temporal_projection`), `expected_discontinuity`.

**Reversion rule.** A candidate is REVERTED if the actor CLASS does not actually move — the same humans doing the same job with an "AI-powered" label, or (when the current actor set is already agentic) another agent bolted onto the same side of the value chain. The guard KEEPS the card iff `to` moved CLASS (human ↔ agent) OR SIDE (buy ↔ sell ↔ regulate ↔ supply) vs `from`, OR is a forward temporal projection; same side + same class is REVERTED and discarded.

**Hackathon recontextualization note.** The original speaks of startups, wedges, corridors, founders, and fundability. In this pack the "parent wedge" role is played by the team's obvious/default hackathon direction, "founders' assets" = the team's skills and experience from `inputs.md`, and "fundable on the whole rubric" = scoring well on `rubric/win-hackathon.md`. Read the original's language through that lens; do not edit the original text itself.

---
# ORIGINAL OPERATOR (verbatim — do not edit)
---

You are applying ONE system-evolution operator — **ACTOR / ONTOLOGY SUBSTITUTION** — at the
**VISION** altitude. Every other commodity-escape move recombines the actors that exist in the input
corpus TODAY (remove an asset, zoom to the supersystem, turn the constraint into the product). They
stay inside the present-tense ontology. This operator does the one thing they cannot: it **substitutes
the CLASS of an actor in the value chain, or projects the actor set forward in time** — so a domain
that is saturated for the CURRENT actors can be wide open for a DIFFERENT class of actor.

This is GENERAL. It applies to ANY actor in the chain, not just the buyer:

- **Buyer class substitution** — the buyer stops being a human and becomes an AI agent / autonomous
  system (the worked example below).
- **Role swaps** — regulator → buyer; supplier → competitor or platform; the object of the service
  becomes itself an actor ("the customer is an agent").
- **Channel substitution** — human-mediated → agent-mediated / machine-to-machine (A2A).
- **Temporal projection** — ask "who or WHAT is this actor in ~3 years?" and design for that arrived
  future, not today's incumbent.

## Worked example (ONE instance of the general move — do NOT overfit to "buyers")
In procurement, human purchasing managers are already being replaced by AI agents. Apply the operator to
the BUYER, not the seller: the next step is **AI agents replacing the human BUYER** — autonomous agents
that plan, source and purchase on a business's behalf. The reframed vision is not "a better dashboard for
human buyers" (saturated, human actor) but **sourcing / verification / settlement INFRASTRUCTURE built
FOR AI buyers** — the agent-to-agent procurement economy. Same capability, a NEW actor class, an
UN-crowded market because the incumbents all serve the human actor.

## Current actor set in this value chain (substitute or forward-project ONE class)
```
{{current_actors}}
```

## The move
1. Name the dominant actor CLASSES in the value chain above (who decides, who pays, who is served, who
   mediates, who supplies, who regulates).
2. Pick ONE and SUBSTITUTE its class or PROJECT it forward in time (human → agent; mediator → machine;
   object → actor; today's incumbent → its 3-year successor).
3. Write a VISION where the team's transferable capability serves that NEW actor class. The right to
   win must come from a real capability — not "anyone could build it."

## Hard rules
- **The actor CLASS must actually move.** A vision where the SAME humans (marketers, agencies, reps,
  managers) do the same job with an "AI-powered" label has NOT substituted an actor — it is a faster
  horse and the run will flag it `reverted` and discard it. Who PAYS / DECIDES / is SERVED must change.
- **If one side of the value chain is ALREADY agentic, substitute a DIFFERENT actor — do NOT add
  another agent on the same side.** When the CURRENT ACTOR SET already contains AI / autonomous /
  agentic actors (e.g. the SELLER or the agency is already an AI agent), an "autonomous agent that
  does X" on that same side is a cosmetic rename and will be flagged `reverted`. Move the substitution
  to an actor class the current frame does NOT yet treat as an agent — most often the **BUYER**
  (human buyer → AI buyer / agent economy / machine-to-machine procurement), or the regulator, the
  object being served, or a forward-projected actor that does not exist in the set today.
- **Novelty lives at the VISION, not the wedge or bridge.** Materialize the reframe as the vision's
  core thesis; the wedge and bridge are execution instruments downstream.
- **Be bold about the market existing yet.** It is CORRECT to name a market of future actors (e.g. AI
  buyers) that is small or forming today — the novelty check rewards "no incumbents", and the
  claim|reality cascade tests downstream whether the future actor has any real demand signal and flags
  it if not. Do NOT self-censor to the present here; propose the discontinuity. Reality disciplines it,
  not you.
- **Discontinuous, fundable.** This is the EVO contract — a genuine shift in WHO the actor is, with an
  honest path to a $1B+ ceiling where the team's capability grants the edge.

## REQUIRED output — declare the substitution structurally
For EACH vision, in ADDITION to the normal vision fields, return a `substituted_actor` object that declares
exactly what you substituted. The reversion guard reads THIS structured declaration — NOT your wording — so it
can tell a compliance agent (a NEW actor) from an ad agent (the old one) even when both say "agent":

```json
"substituted_actor": {
  "axis": "buyer|seller|supplier|regulator|channel|object|other",
  "from": "<the original actor, e.g. 'human media buyer' or 'ad agency'>",
  "to": "<the substituted / forward-projected actor, e.g. 'autonomous AI buyer agent'>",
  "from_side": "buy|sell|regulate|supply|other",
  "to_side": "buy|sell|regulate|supply|other",
  "temporal_projection": true|false
}
```

The guard KEEPS the vision iff `to` moved CLASS (human ↔ agent) OR SIDE (buy ↔ sell ↔ regulate ↔ supply) vs
`from`, OR is a forward temporal projection. Same side + same class (e.g. a seller-side agent renamed to another
seller-side agent) = REVERTED and discarded. So make the move REAL and declare it honestly.
