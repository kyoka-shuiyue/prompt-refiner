# prompt-refiner

`prompt-refiner` is a Codex skill for turning rough user intent into clear, reusable, executable prompts or task briefs.

Its root purpose is not to "polish text". It exists to reduce ambiguity and turn an idea into work another agent can carry out without guessing. Before producing anything visible, the agent should internally clarify what the user is really trying to accomplish, what success looks like, which constraints must be preserved, what can be safely assumed, what must be asked, and whether the final prompt would let another agent start work directly.

When the user asks Codex to do the underlying task, the skill should run silently in the background and Codex should do the task. Only output the refined prompt when the user explicitly asks to optimize, rewrite, deepen, stress-test, compare, or produce a prompt/task brief.

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
  forward-test-report.md
```

## Design Goals

- Turn vague, fragmented, conflicting, or under-specified intent into an executable prompt or task brief.
- Treat prompt refinement as task clarification, not cosmetic rewriting.
- Internally self-grill instead of starting a long live interview by default.
- Prefer recommended defaults over pushing every decision back to the user.
- Ask visible questions only for true blockers.
- Preserve concrete user constraints instead of generalizing them away.
- Separate direct execution from prompt-output mode: if the user wants work done, do the work; if the user wants a prompt, output the prompt.
- Handle current information, high-stakes domains, hidden chain-of-thought requests, technical conflicts, and candidate prompt selection safely.
- Keep the skill folder lean: no bundled example files unless they are necessary for execution.

## Mental Model

For every request, the skill answers these questions internally:

- What is the user actually trying to accomplish?
- What would a successful answer or deliverable look like?
- Which user constraints are hard requirements?
- Which missing details can be safely assumed, and how should those assumptions be labeled?
- Which missing details are blockers that require a question?
- What should be explicitly excluded so the next agent does not overbuild?
- Does the resulting prompt give another agent enough context, scope, acceptance criteria, and verification guidance to start?

The output should feel less like edited prose and more like a compact operating brief.

## Eval Suite

The `evals/` folder contains a small manual eval set:

- `prompt-refiner-cases.jsonl`: test requests and expected behavior.
- `rubric.md`: scoring dimensions and failure markers.
- `forward-test-report.md`: results from fresh subagent forward-testing.

Run the eval by giving each case to an agent with this skill installed, then score the output against the rubric. For independent forward-testing, use fresh threads or subagents and pass only the skill path plus the user request, not your expected answer.

## License

MIT. See `LICENSE`.
