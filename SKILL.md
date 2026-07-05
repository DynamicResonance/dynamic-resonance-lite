---
name: dynamic-resonance-lite
description: Generate winning hackathon ideas from your team's resume and the hackathon rules. Runs a full pipeline: ingests team skills + required sponsor tech + judging criteria, generates 9-15 idea candidates via three operators (asset removal, actor substitution, constraint-into-product), scores them with a 100-point hackathon-winning rubric, presents the top 3 as investor-style pitches, and iterates on your feedback until you settle on an idea. Use whenever the user mentions a hackathon, needs project ideas for a competition, or runs /dynamic-resonance-lite.
---

# Dynamic Resonance Lite — Orchestrator

You run a 5-phase pipeline that turns a team's profile and a hackathon's rules into scored,
pitch-ready ideas, looping on feedback until the user settles on one.

**Progressive disclosure.** Read a sub-skill file only when its phase begins — never load them
all up front. The sub-files are: `operators/asset-removal.md`, `operators/actor-substitution.md`,
`operators/constraint-into-product.md`, `rubric/win-hackathon.md`, `templates/pitch-format.md`.

**Core rules.** Run phases strictly in order — never skip or reorder (Phase -1 is a silent pre-flight
that never blocks the pipeline). All working state lives in a
`.drl/` directory in the current project folder. Keep everything you say to the user concise; the
`.drl/` files carry the detail. See **Boundaries** at the end before you begin.

**Asking the user.** Whenever you ask the user to choose between defined options (resume/archive,
confirm mapping, settle/iterate/reroll, yes/no), present them as selectable options if your environment
supports presenting choices; otherwise ask in plain text. Group related questions into ONE interaction
— never a separate turn per question. Keep free-text questions (like "point me at your files")
conversational. A timed-out or auto-submitted interaction is NOT an answer. If a question tool returns
due to timeout, expiry, or auto-continue rather than an explicit user action, do NOT proceed on those
values: stop, state in one line that you are waiting for the user's actual choice, re-ask the same
question, and wait. There is no time limit on waiting for the user — never advance the pipeline, start
generation, or spend work on the user's behalf because a timer fired.

---

## Phase -1 — Self-update (silent)

1. Determine the directory where this `SKILL.md` file itself resides (the installed skill folder — not
   the user's project folder).
2. Silently run a fast-forward-only git pull in that directory (equivalent of
   `git -C <skill_dir> pull --ff-only`).
3. **Failure handling:** if the directory is not a git repo, git is unavailable, there is no network, or
   the pull cannot fast-forward (local changes) — do NOT report an error, do NOT retry; just proceed
   with the currently loaded version.
4. If the pull DID bring updates, tell the user in one short line that the skill was updated and changes
   take effect from the next run, then continue this run normally with the already-loaded instructions.
5. This phase must never block or delay the pipeline noticeably and never asks the user anything.

---

## Phase 0 — Session check

1. Check whether a `.drl/` directory exists in the current working directory.
2. If it exists and contains previous iterations (`.drl/iteration-*` folders or `.drl/winner.md`),
   briefly summarize the session state — which iteration it reached, roughly how many ideas were
   generated, and whether a winner was chosen — then ask the user to choose:
   - **Resume** this session (continue where it left off), or
   - **Archive** it (rename `.drl/` to `.drl-archive-<today's date>/`) and start fresh.
   — present Resume/Archive as two selectable options where supported.
3. If `.drl/` does not exist, create it and proceed to Phase 1.

Do not read iteration contents in bulk here — read just enough to summarize.

---

## Phase 1 — Ingestion

Goal: collect three inputs — the team, the hackathon rules / required tech, and the judging criteria.
The user has ideally dropped files into the project folder already.

1. **Ask for the inputs.** In one short, conversational message, ask the user to point at their input
   files — (a) team resume(s), (b) hackathon rules / required sponsor tech, (c) judging criteria — or
   to say "look in the folder," or to paste the text. Keep it short; accepted formats (file paths, a
   folder to scan, pasted text, or a URL to the hackathon page) fit in one compact line.
   If the user says "look in the folder," list the project files (excluding `.drl/` and hidden
   directories), infer which file is which input, then present the mapping and ask for confirmation as
   selectable options where supported: "Mapping correct" / "Let me correct it". Confirm the mapping
   before reading.
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
4. **Gate before distillation.** Do not proceed to distillation until the three base inputs are each
   either collected or explicitly resolved as missing per the missing-input rules above.
5. **Distill into `.drl/inputs.md`** with these sections. Distill, don't copy — but preserve concrete
   facts and numbers:
   - `## Team` — skills, notable experience, size, stack strengths
   - `## Sponsor tech / required stack` — each item plus what it actually does
   - `## Judging criteria` — the hackathon's real criteria if provided, else "default rubric"
   - `## Constraints` — time window, team-size limit, submission requirements
   - `## Extra context` — anything else the user offered, including explicit exclusions
   - `## Idea lean` — direction preference (set in the sharpening round below)
   - `## Fun-facts mode` — on/off (default off)
6. **Sharpening questions (always).** After writing the digest and showing the quick read-back, ALWAYS
   ask a sharpening round as ONE multi-question interaction (a multi-question form with selectable
   options where the agent supports it; compact numbered questions in plain text otherwise). Questions,
   each with 3–4 sensible preset options derived from the digest plus a free-text option:
   (1) Team size & mix on the day — ALWAYS ask, even when the resume states it clearly: preset the
   first option to the team as understood from the inputs (e.g. "2 — Alex (backend) + Maria (frontend),
   as in the resume") so confirming takes one click, and offer options for common changes (someone
   can't make it / extra member joining / finding teammates at the event) plus free text;
   (2) Which direction should ideas lean, given the team's strengths (e.g. "play to our superpower:
   <derived>", "classic crowd-pleaser for this event", "surprise me / mix");
   (3) Anything you explicitly do NOT want to build (preset a few plausible exclusions derived from the
   digest, plus free text);
   (4) Use fun facts? (yes / no; default off) — include ONLY if not already answered earlier in the
   conversation.
   Fold the answers into `inputs.md` (team details, `## Idea lean` — new section, `## Extra context` /
   exclusions, `## Fun-facts mode`) before the final confirmation.
7. **Confirm.** Show the digest to the user for a quick confirm/correct before proceeding.

---

## Phase 2 — Generation

1. Read `.drl/inputs.md`. Determine the current iteration number `N` — `01` if fresh; if this is a
   feedback loop, `N` = previous iteration + 1. Create `.drl/iteration-NN/` (zero-padded, e.g.
   `iteration-01`). Honor the `## Idea lean` preference when deriving each operator's parent direction.
2. If this is iteration 2+, also read the previous iteration's `top3.md` and `feedback.md`. Treat the
   chosen/liked ideas and the user's feedback as seed material, and **explicitly avoid regenerating
   ideas the user rejected.** "Rejected" means ideas the user explicitly turned down or criticized —
   not merely unselected candidates from previous iterations; those may return (improved) if feedback
   or scoring points at them.
3. **Wildness level.** Determine this iteration's wildness before generating:
   - Iterations 1-2: LEVEL 0 (standard) — operators as written.
   - Iteration 3, if reached via Reroll or via Iterate-feedback expressing dissatisfaction with the
     whole direction (rather than refining a liked idea): LEVEL 1 (bold).
   - Iteration 4+ under the same conditions: LEVEL 2 (wild). Each further reroll stays at LEVEL 2 but
     must explore a previously untouched domain.
   - If the user is refining a liked idea (normal Iterate), wildness stays at LEVEL 0 regardless of
     iteration number.
   Level effects, applied ON TOP of each operator's instructions (never overriding its hard rules or
   reversion guard):
   - LEVEL 1 (bold): ban the two most obvious moves per operator (the ones a naive run would produce
     first); require each card to draw on at least one domain, actor, or mechanism NOT mentioned in
     `inputs.md`; prefer candidates that would score 14/15 on originality over safe 10s.
   - LEVEL 2 (wild): additionally require cross-domain transplants (mechanism from an unrelated
     industry applied here), aggressive temporal projection (the operator's world in ~3 years), and at
     least one card per operator that feels borderline-unreasonable but still passes the hard gates.
     Buildability discipline stays: every card must still honestly pass the 4-6h buildability gate —
     wild in concept, small in build.
   Announce the level to the user in one casual line when it rises (e.g. "Round 3, none clicked so far
   — turning up the wildness.").
4. **Fun-facts injection.** If `inputs.md` says fun-facts mode is ON: read `facts/fun-facts.md`. For
   each operator batch, at least 2 of its cards must each draw on exactly ONE entry from the base —
   using the fact as resonant context: the "why now", the source of urgency for the problem, or the
   analogy/mechanism the idea transplants. The fact works as a lens, not a decoration: if removing the
   fact from the card's mechanics changes nothing, it doesn't count. Each such card adds a field to
   `operator_fields`: `fun_fact_used: <FF-id> — <how it shaped the idea, one line>`. Cards not using a
   fact set `fun_fact_used: none`. Spread facts across cards — never reuse the same FF entry more than
   twice per iteration. On wildness LEVEL 1+, prefer facts from domains absent from `inputs.md`; on
   LEVEL 2, at least one card per operator must transplant a mechanism from a fact in an unrelated
   industry.
   If fun-facts mode is OFF, skip this step entirely and set no `fun_fact_used` fields.
5. For each of the three operators, **in sequence**: read the operator file
   (`operators/asset-removal.md`, then `operators/actor-substitution.md`, then
   `operators/constraint-into-product.md`), follow its instructions, and generate **3–5 idea
   candidates** through that operator's lens. Never more than 5 per operator — beyond 5 the ideas
   start repeating.
6. Write every candidate in the standard **IDEA CARD** format (defined here; the operators reference
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
7. **Reversion guard.** Each operator file defines what makes a candidate "reverted" — a faster-horse
   that undid the operator's move. After generating each operator's batch, check every candidate against
   that operator's reversion rule using its declared `operator_fields`. Discard reverted candidates and
   regenerate replacements until the batch holds 3–5 non-reverted ideas. Log discarded ones with a
   one-line reason at the bottom of `candidates.md` under `## Reverted (discarded)`.
8. Write all candidates to `.drl/iteration-NN/candidates.md`. Do **not** dump every candidate into the
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
   the conversation, clearly numbered 1 / 2 / 3, each with its rubric score. When a top-3 idea has a
   `fun_fact_used` value other than `none`, add one short line under its pitch block: "Riffs on: <fact
   short title>".
4. **Recommendation footer.** After the three pitch blocks, append a short "To get better ideas next
   round:" footer with 1-2 concrete, personalized recommendations for improving the INPUTS — derived
   from actual weak spots you observed in `inputs.md` and in this round's scoring. Pick only what
   genuinely applies, e.g.:
   - team gaps: a missing skill that kept blocking high-scoring directions ("several strong candidates
     died on frontend polish — a designer or one more frontend hand would unlock them");
   - missing judging detail: if scoring fell back to the default rubric, suggest hunting for the
     hackathon's real scoring breakdown, past winners, or the organizers' previous events;
   - thin team description: if the resume digest was sparse, name the 1-2 facts that would sharpen
     generation most (past projects, strongest stack, domain expertise);
   - missing sponsor-tech detail: if `sponsor_tech_role` kept being generic, suggest linking the
     sponsor SDK's docs page or listing its 2-3 concrete capabilities.
   Each recommendation must say WHAT to add and WHY it will improve the next round (one line each). If
   inputs are genuinely complete, say so in one line instead of inventing recommendations.
5. Keep it tight: each idea's block stays within the pitch template's word budget. No tables; the
   footer adds at most 3 short lines.

---

## Phase 5 — Feedback loop

1. After presenting, **always** explicitly offer these three options — never leave the next step
   implicit:
   - **(a) Settle** — pick one idea and finish.
   - **(b) Iterate** — give feedback on any of the ideas (what to change, merge, or push further), or
     add new information/files in response to the footer; either seeds a new generation loop and counts
     as Iterate feedback.
   - **(c) Reroll** — none fit; optionally say what direction to explore, triggering a full new
     generation round.
   — present Settle / Iterate / Reroll as three selectable options where supported (feedback text can
   accompany Iterate/Reroll).
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
- Wildness levels change how far generation explores; they never relax hard gates, reversion rules, or
  buildability.
- Fun facts are creative fuel, never claims: a pitch may reference the news item as context, but all
  Problem/Solution/Impact numbers still follow the estimate-framing rules and must not present the
  fact's projections as the product's own results.
- Never treat an interaction timeout or auto-continued default as user consent. Expensive steps
  (generation, scoring, presentation) may only start after an explicit user action on the preceding
  question.
