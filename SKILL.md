---
name: dynamic-resonance-lite
description: Generate winning hackathon ideas from your team's resume and the hackathon rules. Runs a full pipeline: ingests team skills + required sponsor tech + judging criteria, generates 9-15 idea candidates via three TRIZ operators (asset removal, actor substitution, constraint-into-product), scores them with a 100-point hackathon-winning rubric, presents the top 3 as investor-style pitches, and iterates on your feedback until you settle on an idea. Use whenever the user mentions a hackathon, needs project ideas for a competition, or runs /dynamic-resonance-lite.
---

# Dynamic Resonance Lite — Orchestrator

You run a 5-phase pipeline that turns a team's profile and a hackathon's rules into scored,
pitch-ready ideas, looping on feedback until the user settles on one.

**Progressive disclosure.** Read a sub-skill file only when its phase begins — never load them
all up front. The sub-files are: `operators/asset-removal.md`, `operators/actor-substitution.md`,
`operators/constraint-into-product.md`, `rubric/win-hackathon.md`, `templates/pitch-format.md`.

**Core rules.** Run phases strictly in order — never skip or reorder. All working state lives in a
`.drl/` directory in the current project folder. Keep everything you say to the user concise; the
`.drl/` files carry the detail. See **Boundaries** at the end before you begin.

---

## Phase 0 — Session check

1. Check whether a `.drl/` directory exists in the current working directory.
2. If it exists and contains previous iterations (`.drl/iteration-*` folders or `.drl/winner.md`),
   briefly summarize the session state — which iteration it reached, roughly how many ideas were
   generated, and whether a winner was chosen — then ask the user to choose:
   - **Resume** this session (continue where it left off), or
   - **Archive** it (rename `.drl/` to `.drl-archive-<today's date>/`) and start fresh.
3. If `.drl/` does not exist, create it and proceed to Phase 1.

Do not read iteration contents in bulk here — read just enough to summarize.

---

## Phase 1 — Ingestion

Goal: collect three inputs — the team, the hackathon rules / required tech, and the judging criteria.
The user has ideally dropped files into the project folder already.

1. **Ask for the inputs.** Ask the user to point at their files for: (a) team resume(s),
   (b) hackathon rules / required sponsor tech, (c) judging criteria. Make clear that any format and
   any combination works — file paths, a folder to scan, pasted text, or a URL to the hackathon page.
   If the user just says "look in the folder," list the project files (excluding `.drl/` and hidden
   directories), infer which file is which input, and confirm your mapping with the user before reading.
2. **Handle missing inputs:** One input file may satisfy several roles at once — e.g. a hackathon
   rules page that embeds the judging criteria counts as both (b) and (c). Treat an input as missing
   only if it appears in none of the provided materials.
   - No resume → instead ask 3–4 quick questions about the team: core skills, notable past projects,
     strongest parts of their stack, and team size.
   - No judging criteria → tell the user the built-in rubric's default hard gates and criteria will
     be used.
   - No sponsor / tech requirements → ask whether the hackathon is tech-agnostic. If it is, note that
     sponsor-centrality logic will be skipped downstream.
3. **Invite precision.** In one friendly line, tell the user that the more precisely the team
   describes itself, the sharper the generated ideas will be. Offer to accept any extra context —
   interests, constraints, and especially what they do NOT want to build.
4. **Distill into `.drl/inputs.md`** with these sections. Distill, don't copy — but preserve concrete
   facts and numbers:
   - `## Team` — skills, notable experience, size, stack strengths
   - `## Sponsor tech / required stack` — each item plus what it actually does
   - `## Judging criteria` — the hackathon's real criteria if provided, else "default rubric"
   - `## Constraints` — time window, team-size limit, submission requirements
   - `## Extra context` — anything else the user offered
5. **Confirm.** Show the digest to the user for a quick confirm/correct before proceeding.

---

## Phase 2 — Generation

1. Read `.drl/inputs.md`. Determine the current iteration number `N` — `01` if fresh; if this is a
   feedback loop, `N` = previous iteration + 1. Create `.drl/iteration-NN/` (zero-padded, e.g.
   `iteration-01`).
2. If this is iteration 2+, also read the previous iteration's `top3.md` and `feedback.md`. Treat the
   chosen/liked ideas and the user's feedback as seed material, and **explicitly avoid regenerating
   ideas the user rejected.** "Rejected" means ideas the user explicitly turned down or criticized —
   not merely unselected candidates from previous iterations; those may return (improved) if feedback
   or scoring points at them.
3. For each of the three operators, **in sequence**: read the operator file
   (`operators/asset-removal.md`, then `operators/actor-substitution.md`, then
   `operators/constraint-into-product.md`), follow its instructions, and generate **3–5 idea
   candidates** through that operator's lens. Never more than 5 per operator — beyond 5 the ideas
   start repeating.
4. Write every candidate in the standard **IDEA CARD** format (defined here; the operators reference
   it):
   - `id`: `NN-<operator-shortname>-<k>` — operator shortnames are `asset`, `actor`, `constraint`
     (e.g. `01-asset-1`)
   - `name`: 2–4 word working title
   - `one_liner`: one sentence — what it is
   - `operator_move`: one line — what the operator did (what was removed / substituted / turned into
     product)
   - `mechanics`: 2–4 sentences — how it works end to end
   - `sponsor_tech_role`: how the required tech sits in the critical path (or
     "n/a — tech-agnostic hackathon")
   - `demo_moment`: the single "wow" reveal a judge sees, in one sentence
   - `build_sketch`: what gets built in 4–6 hours — 2–3 bullets
   - `operator_fields`: each operator defines 2–4 additional required fields of its own (declared in
     the operator file) — e.g. `removed_atom`, `substituted_actor`, `turned_constraint`,
     `expected_discontinuity`. Include them exactly as the operator specifies.
5. **Reversion guard.** Each operator file defines what makes a candidate "reverted" — a faster-horse
   that undid the operator's move. After generating each operator's batch, check every candidate against
   that operator's reversion rule using its declared `operator_fields`. Discard reverted candidates and
   regenerate replacements until the batch holds 3–5 non-reverted ideas. Log discarded ones with a
   one-line reason at the bottom of `candidates.md` under `## Reverted (discarded)`.
6. Write all candidates to `.drl/iteration-NN/candidates.md`. Do **not** dump every candidate into the
   conversation — it's too long. Tell the user how many were generated per operator, then proceed to
   scoring.

---

## Phase 3 — Scoring

1. Read `.drl/iteration-NN/candidates.md` and `rubric/win-hackathon.md`.
2. **Hard gates.** If the hackathon provided real rules/criteria (from `inputs.md`), use THOSE as the
   hard gates, keeping the rubric's gates only where they don't conflict. Otherwise use the rubric's
   default gates. Reject any candidate that fails a gate and record a one-line rejection reason.
3. **Score** every surviving candidate on the rubric's 10 weighted criteria (100 points total). If the
   hackathon's own judging criteria were provided, additionally check each candidate against them, and
   let the real criteria outweigh rubric defaults wherever they conflict.
4. Write `.drl/iteration-NN/scores.md`: a compact table — `id`, `name`, `score`, one-line strongest
   point, one-line weakest point — plus the list of gate rejections.
5. Select the **top 3** by score. On a tie, prefer the candidate with the stronger `demo_moment`.

---

## Phase 4 — Presentation

1. Read `templates/pitch-format.md`.
2. For each of the top 3, produce the full pitch block per that template: Pitch one-liner, Problem,
   Solution, How it Works, Impact.
3. Write the three blocks to `.drl/iteration-NN/top3.md` AND present them to the user as plain text in
   the conversation, clearly numbered 1 / 2 / 3, each with its rubric score.
4. Keep it tight: each idea's block stays within the pitch template's word budget. No tables.

---

## Phase 5 — Feedback loop

1. After presenting, **always** explicitly offer these three options — never leave the next step
   implicit:
   - **(a) Settle** — pick one idea and finish.
   - **(b) Iterate** — give feedback on any of the ideas (what to change, merge, or push further),
     which seeds a new generation loop.
   - **(c) Reroll** — none fit; optionally say what direction to explore, triggering a full new
     generation round.
2. **If Settle:** write `.drl/winner.md` containing the chosen idea's full pitch block, its idea card,
   and a short `## Next steps at the hackathon` section — what to build first in hours 0–2, 2–4, and
   4–6, derived from its `build_sketch`. Congratulate briefly and end.
3. **If Iterate or Reroll:** save the user's feedback verbatim to `.drl/iteration-NN/feedback.md`,
   then loop back to Phase 2 as iteration `N+1`. The loop runs until the user settles or stops.

---

## Boundaries

- Never skip a phase or reorder phases.
- Never invent team skills, sponsor tech, or judging criteria that are not present in the inputs (or in
  the user's answers).
- Never fabricate numbers presented as facts in pitches. Statistics must be plausible, clearly framed
  as estimates ("~", "est."), and must never cite invented sources. (The pitch template governs the
  details.)
- All numbered "wow" claims live in the pitch; the idea cards stay mechanical and factual.
- Everything user-facing is concise. The `.drl/` files carry the detail.
- The user may be on Claude Code, Cursor, or another agent. Never reference agent-specific tools — say
  "read the file," "ask the user," "write to `.drl/`."
