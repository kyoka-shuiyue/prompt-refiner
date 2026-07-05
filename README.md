# prompt-refiner

`prompt-refiner` is a Codex skill for turning rough user intent into clear, executable prompts.

It is intentionally prompt-focused `grill-me`: the agent internally self-interviews the request, answers ambiguity with recommended defaults, resolves conflicts, and outputs the strongest usable prompt. For non-trivial direct execution, it runs a deeper execution track first: distilled self-grill summary, only the highest-impact user questions, `plan.md`, then implementation.

## Install

Copy the skill folder into your Codex skills directory:

```bash
cp -R prompt-refiner ~/.codex/skills/
```

On Windows PowerShell:

```powershell
Copy-Item -Recurse .\prompt-refiner "$env:USERPROFILE\.codex\skills\prompt-refiner"
```

Restart Codex if your environment does not hot-reload skills.

## Contents

```text
prompt-refiner/
  SKILL.md
  agents/
    openai.yaml
  references/
    patterns.md
evals/
  prompt-refiner-cases.jsonl
  rubric.md
```

## Design Goals

- Trigger on prompt refinement, rewriting, polishing, stress-testing, comparison, and `grill-me` style prompt improvement.
- Internally self-grill deeply instead of starting a long live interview by default.
- For non-trivial work, use about `20-50` internal questions and show about `8-15` distilled decision bullets.
- Prefer recommended defaults over pushing every decision back to the user; write low-controversy choices into the prompt or plan.
- Ask visible questions only for true blockers or distilled high-impact controversies.
- For non-trivial direct execution, create `plan.md` before implementation unless the user forbids files.
- Preserve concrete user constraints instead of generalizing them away.
- Handle current information, high-stakes domains, hidden chain-of-thought requests, technical conflicts, and candidate prompt selection safely.
- Keep the skill folder lean: no bundled example files unless they are necessary for execution.

## Eval Suite

The `evals/` folder contains a small manual eval set:

- `prompt-refiner-cases.jsonl`: test requests and expected behavior.
- `rubric.md`: scoring dimensions and failure markers.
- `forward-test-report.md`: results from fresh subagent forward-testing.

Run the eval by giving each case to an agent with this skill installed, then score the output against the rubric. For independent forward-testing, use fresh threads or subagents and pass only the skill path plus the user request, not your expected answer.

## License

MIT. See `LICENSE`.
