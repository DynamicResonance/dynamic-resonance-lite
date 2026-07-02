---
name: win-hackathon-rubric
description: 100-point scoring rubric for hackathon idea candidates — hard gates plus 10 weighted criteria. Read by the dynamic-resonance-lite orchestrator during Scoring, not standalone.
---

# How to apply this rubric inside dynamic-resonance-lite

1. **Precedence.** The rubric's hard-gate table is explicitly an EXAMPLE set ("to be replaced with actual hackathon rules"). If `.drl/inputs.md` contains the hackathon's real rules/criteria, real rules REPLACE the example gates wherever they overlap and are ADDED as new gates where the rubric has none. Rubric gates that don't conflict with real rules stay active as sanity defaults (safety/ethics ALWAYS stays active). If `inputs.md` says "default rubric," apply the gates as written.
2. **Gating procedure.** Check every candidate against every active gate first. A single gate failure = rejected before scoring, with a one-line reason referencing the gate.
3. **Scoring procedure.** Score survivors on the 10 weighted criteria. For each criterion pick Strong pass / Minimum pass / Fail per the table and assign points accordingly (Strong pass = full weight, Minimum pass = roughly half weight, Fail = 0; interpolate with judgment). Sum to a /100 score.
4. **Real judging criteria.** When the hackathon provided its own judging criteria: map each real criterion onto the nearest rubric criterion and let the REAL criterion's emphasis adjust the weight up or down; where a real criterion has no rubric counterpart, score it qualitatively and note it in `scores.md`.
5. **Calibration.** Use the "Quantified thresholds" and "Score interpretation" tables to calibrate — e.g. a candidate whose demo needs >90 seconds to understand cannot get a Strong pass on Killer demo moment.
6. **Sponsor-centrality exception.** If `inputs.md` marks the hackathon as tech-agnostic, criterion 1 (Sponsor / track alignment) redistributes its 15 points proportionally across criteria 2–5, and the sponsor-compliance gate is skipped.

---
# ORIGINAL RUBRIC (verbatim — do not edit)
---

# **Rubric: Win Hackathon**

## **Core thesis**

A hackathon-winning wedge is not the most fundable startup. It is the **most impressive 4–6 hour proof of a future product**.

It should show:

“This did not exist yesterday, it uses the sponsor’s new tech in a non-trivial way, it works live, and it makes judges feel the future arrived early.”

# **Hard gates \[example, to be replaced with actual hackathon rules \]**

A generated candidate should be rejected before scoring if it fails any of these.

| Hard gate | Example Requirement | Example fail |
| ----- | ----- | ----- |
| **Sponsor/tool compliance** | Uses required sponsor technology in the critical path, not as decoration | Calls Gemini once for summary but core product works without it |
| **Team size compliance** | Fits event rule, e.g. max team size of 4  | Requires 6 specialists |
| **Buildability** | Can produce a credible demo within hackathon time (e.g. in 4–6 hours) with available skills | Needs FDA approval, enterprise integration, or custom hardware |
| **Demoability** | Has one end-to-end flow that can be shown in specified time (e.g. 1-3 minutes) | Backend-only concept with no visible output |
| **Novelty threshold** | Not a trivial chatbot, wrapper, CRUD app, or generic dashboard | “Chat with your PDF” |
| **Submission completeness** | Can produce demo, short pitch, architecture explanation, and repo/artifact | Requires extensive explanation to understand |
| **Safety / ethics** | Does not require unsafe claims, private data misuse, medical/legal overclaiming, or deceptive behavior | “AI doctor diagnoses stroke live” without guardrails |

# **Autoresearch Success Criteria: Hackathon-winning wedge**

| Criterion | Weight | Strong pass | Minimum pass | Fail |
| ----- | ----- | ----- | ----- | ----- |
| **1\. Sponsor / track alignment** | 15 | Sponsor tech is central to the product; impossible or much weaker without it | Uses at least one required sponsor tool in a meaningful workflow | Sponsor tool is bolted on or only mentioned |
| **2\. Killer demo moment** | 15 | Judges can understand the magic in ≤30 seconds; demo has a clear “wow” reveal | One working end-to-end flow in ≤3 minutes | Needs long explanation; no visible payoff |
| **3\. Originality / “car, not faster horse”** | 14 | Feels like a new behavior, workflow, or system made possible by recent tech | Fresh recombination of known tools into a useful workflow | Generic chatbot, dashboard, summarizer, or wrapper |
| **4\. Technical depth** | 14 | Shows real systems work: agents, memory, evals, retrieval, orchestration, latency, scaling, local-first, multimodal, or control loops | More than a single model call; has architecture worth explaining | Simple prompt → response app |
| **5\. Meaningful AI use** | 12 | AI performs reasoning, planning, perception, synthesis, tool use, or autonomous action | AI meaningfully improves outcome versus rules/manual workflow | AI is cosmetic text generation |
| **6\. Buildability in 4–6 hours** | 10 | Small enough to finish, impressive enough to win; uses existing APIs/components wisely | Can be built as a thin but working MVP | Too complex, fragile, or integration-heavy |
| **7\. Problem clarity / user value** | 8 | Pain is instantly legible; user benefit obvious without market education | Problem is understandable after short pitch | Problem is vague, abstract, or fake |
| **8\. Execution polish** | 6 | Clean UX, stable demo, good error handling, crisp visual output | Basic UI/CLI/API works reliably enough | Broken, confusing, or visually illegible |
| **9\. Story / pitch clarity** | 4 | 60-second narrative: problem → why now → demo → why it matters | Basic explanation works | Judges cannot repeat what it does |
| **10\. Expansion imagination** | 2 | Obvious path from hack to real product or company | Some plausible next step | Pure toy with no continuation |

Total: **100 points**

---

# **Score interpretation**

| Score | Autoresearch output |
| ----- | ----- |
| **90–100** | Likely hackathon winner / finalist |
| **80–89** | Strong contender; optimize demo and pitch |
| **70–79** | Good project but needs more wow, depth, or clarity |
| **55–69** | Buildable but unlikely to win |
| **\<55** | Reject or mutate heavily |

---

# **Quantified thresholds**

| Evidence / property | Weak | Minimum pass | Strong pass |
| ----- | ----- | ----- | ----- |
| Time to understand demo | \>90 sec | ≤60 sec | ≤30 sec |
| End-to-end demo length | \>5 min | ≤3 min | ≤90 sec hero flow |
| Build time | \>1 day | 4–6 hours | First working version in 2–3 hours |
| Number of core user flows | 4+ | 1–2 | 1 killer flow |
| Required integrations | 5+ fragile integrations | 1–3 APIs/tools | 1–2 APIs with strong payoff |
| AI centrality | Optional | Important | Product impossible without AI |
| Sponsor centrality | Mentioned | Used meaningfully | Critical path |
| Technical architecture depth | Single prompt | 2+ components | Agent loop / retrieval / eval / multimodal / local-first / orchestration |
| Demo robustness | Breaks often | Works on happy path | Has fallback / canned data / retry |
| Pitch length | \>3 min | ≤90 sec | ≤60 sec |
| Judge memorability | Generic | Clear concept | One-sentence viral description |

---

# **One-line Success Criteria for Autoresearch**

**A hackathon-winning wedge is a sponsor-aligned, technically non-trivial, highly demoable project that creates one memorable “future is here” moment within a 4–6 hour build window.**

