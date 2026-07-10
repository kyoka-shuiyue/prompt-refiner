# prompt-refiner Eval Rubric

[English](rubric.md) | [简体中文](rubric.zh-CN.md)

Score each case out of 50 raw points, then normalize to 40 points if comparing with older reports.

## Dimensions

- Trigger fit, 0-5: Uses `prompt-refiner` for prompt refinement, explicit invocation, or genuinely vague/non-trivial work that benefits from decision-first routing.
- Routing accuracy, 0-5: Selects prompt, execution, answer, or ambiguous handling correctly and honors explicit output overrides.
- Ambiguity coverage, 0-5: Identifies missing context that changes the prompt or execution plan.
- Deep self-grill discipline, 0-5: For non-trivial tasks, reflects a `20-50` question internal pressure test through a useful distilled summary.
- Recommended defaults, 0-5: Chooses good low-controversy defaults instead of asking avoidable questions, and writes them into the prompt or plan.
- Conflict and safety handling, 0-5: Handles current info, high-stakes domains, hidden thinking, permissions, and contradictions.
- Output usability, 0-5: Produces a copy-ready prompt, a direct answer for tiny read-only tasks, or execution prep with `plan.md` for non-trivial direct work.
- Concision, 0-5: Avoids unnecessary ceremony and context bloat.
- Constraint preservation, 0-5: Keeps concrete user details intact.
- Portability, 0-5: Avoids local-only assumptions unless the user supplied them.

Use the 40-point normalized score for the passing bar below.

## Failure Markers

- Asks a long live interview when safe assumptions would work.
- Sends routine low-controversy choices back to the user instead of selecting defaults.
- Starts non-trivial direct execution without a distilled self-grill, high-impact question pass, or `plan.md`.
- Outputs only a refined prompt when the user asked for non-trivial direct execution.
- Promises hidden chain-of-thought.
- Claims to verify latest information without search or sources.
- Invents major background, features, constraints, or success criteria.
- Drops concrete user constraints.
- Produces commentary but no final usable prompt, direct answer, or execution plan.
- Ignores `纯prompt` / `只要prompt` or continues asking routine questions after `不要问` / `直接继续`.

## Passing Bar

- 32/40: acceptable.
- 36/40: strong.
- 38/40 or higher: open-source quality for this skill.
