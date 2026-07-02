---
name: pitch-format
description: Presentation template for top-3 hackathon ideas — a Founder Institute one-sentence Madlib pitch plus Problem / Solution / How it Works / Impact blocks. Read by the dynamic-resonance-lite orchestrator during Presentation, not standalone.
---

# Pitch format

This is the first thing a reader or judge sees. It follows the Founder Institute one-sentence pitch structure, then four short sections. The whole block must be readable in under 60 seconds.

## Block structure (one per idea)

### <N>. <Idea name> — <score>/100

**Pitch:** exactly one sentence in this Madlib:
"<Team/project name> is developing <a defined offering> to help <a defined audience> <solve a problem> with <secret sauce>."
Rules for the sentence:
- No evaluative or superlative adjectives — never "first", "only", "huge", "best", "revolutionary" (they signal inexperience). Plain functional/classifier words ("multi-step", "continuous", "open-source") are fine.
- <a defined audience> must be narrow and specific. "Women" or "small businesses" are far too broad; "idea-stage and pre-seed entrepreneurs" is the right granularity. Test: the audience phrase should name a role AND a context ("compliance officers at regulated startups", not "operations teams"). If you can add a qualifier that meaningfully shrinks who it applies to, the phrase was too broad.
- <a defined offering> is a concrete product category, not a vision statement.
- <secret sauce> is the mechanism that makes it work — for a hackathon, usually the non-trivial use of the sponsor tech or the technical move from the idea card.

**Problem:** 2–3 sentences WITH NUMBERS — who suffers, how often, what it costs (time/money). Numbers must be plausible and framed as estimates ("~", "est.") unless they came from the user's inputs; never cite invented sources.

**Solution:** 2–3 sentences WITH NUMBERS — what changes for the user, quantified (time saved, error reduction, coverage). Same estimate-framing rules.

**How it works:** THE HACKATHON BUILD PLAN — 3–5 short bullets describing exactly what gets built in the 4–6 hour window, derived from the idea card's `build_sketch` and `mechanics`, with the sponsor tech's role in the critical path explicitly named per bullet where it participates. This section is what the team actually executes.

**Impact:** 1–2 sentences WITH NUMBERS — the honest "so what" at demo scale and one line of expansion imagination (rubric criterion 10). No business-plan theater; hackathons don't require a revenue model, they require consequence.

## Length discipline
- Whole block per idea: target 150–190 words, hard ceiling 210 (word counts exclude the block heading and bold section labels). The three blocks together must fit in one conversational message comfortably.
- Sections are prose or tight bullets exactly as specified above; no tables, no headers beyond the block structure.
- If a block exceeds the ceiling, cut Problem/Solution color first, never the How-it-works bullets.

## Consistency rules
- Everything stated must trace back to the idea card and `inputs.md` — no new capabilities invented at pitch time.
- The `demo_moment` from the card must be visible in either Solution or How it works — the judge should be able to spot the "wow" without extra explanation.
- Keep terminology identical to the card (same product name, same actor names) so `scores.md`, `candidates.md` and `top3.md` cross-reference cleanly.
