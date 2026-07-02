# dynamic-resonance-lite

## What it is

A skill pack that turns your team's resume(s) and a hackathon's rules into scored, pitch-ready hackathon ideas. It reads your team's skills, the required sponsor tech, and the judging criteria, generates candidate ideas using three TRIZ-based idea operators, ranks them with a 100-point judging rubric, and runs in a loop until you settle on an idea you want to build.

## Install

Clone the pack into your agent's skills directory.

Claude Code:

```sh
git clone https://github.com/DynamicResonance/dynamic-resonance-lite ~/.claude/skills/dynamic-resonance-lite
```

For Cursor or other agents, clone the folder into that agent's respective skills directory. The pack uses only universal `SKILL.md` features (`name` + `description` frontmatter plus a markdown body), so it works across any agent that supports skills.

## Usage

1. Open your hackathon project folder.
2. Drop your input files into it — team resume(s), the hackathon page/rules, and judging criteria. Any format works.
3. Run `/dynamic-resonance-lite`.
4. Point the skill at your files when asked.

If no judging criteria are provided, the built-in rubric is used.

## Session state

The skill writes all working state to a `.drl/` directory in the current project — the inputs digest, the generated ideas for each iteration, and the feedback history. Each hackathon project keeps its own isolated session, so you can stop and resume anytime.

Add `.drl/` to your project's `.gitignore`.

## Pipeline

1. **Ingestion** — read the team resume(s), hackathon rules, and judging criteria; build an inputs digest of team skills, required sponsor tech, and scoring criteria.
2. **Generation** — produce 9–15 idea candidates by applying three TRIZ operators: asset removal, actor substitution, and constraint-into-product.
3. **Scoring** — rank every candidate against the 100-point win-hackathon rubric (hard gates + weighted criteria).
4. **Presentation** — present the top 3 as investor-style pitches.
5. **Feedback loop** — take your feedback, regenerate and re-score, and repeat until you settle on an idea.
