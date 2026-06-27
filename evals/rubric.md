# prompt-refiner Eval Rubric

Score each case out of 40 points.

## Dimensions

- Trigger fit, 0-5: Uses `prompt-refiner` only when the request is about prompt refinement or when it is explicitly invoked.
- Ambiguity coverage, 0-5: Identifies missing context that changes the prompt.
- Recommended defaults, 0-5: Chooses good defaults instead of asking avoidable questions.
- Conflict and safety handling, 0-5: Handles current info, high-stakes domains, hidden thinking, permissions, and contradictions.
- Output usability, 0-5: Produces a copy-ready prompt or a direct answer when direct execution is requested.
- Concision, 0-5: Avoids unnecessary ceremony and context bloat.
- Constraint preservation, 0-5: Keeps concrete user details intact.
- Portability, 0-5: Avoids local-only assumptions unless the user supplied them.

## Failure Markers

- Asks a long interview when safe assumptions would work.
- Outputs a refined prompt when the user asked for direct execution.
- Promises hidden chain-of-thought.
- Claims to verify latest information without search or sources.
- Invents major background, features, constraints, or success criteria.
- Drops concrete user constraints.
- Produces commentary but no final usable prompt.

## Passing Bar

- 32/40: acceptable.
- 36/40: strong.
- 38/40 or higher: open-source quality for this skill.
