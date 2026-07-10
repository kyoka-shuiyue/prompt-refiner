# prompt-refiner

[English](README.md) | [简体中文](README.zh-CN.md)

`prompt-refiner` is a Codex skill that turns rough intent into either a reusable prompt or a decision-ready execution plan.

It is more than a wording polisher. The skill self-grills ambiguous requests, preserves concrete constraints, selects safe defaults, exposes only consequential decisions, and routes the result to the correct behavior: return a prompt, prepare direct execution, answer a read-only question, or ask a concise clarification.

## Why Use It

Rough requests often hide the decisions that determine whether an AI task succeeds: audience, scope, non-goals, source freshness, permissions, output format, verification, and rollback. `prompt-refiner` makes those decisions explicit without forcing the user through a long interview.

Core behavior:

- Pressure-test normal requests with roughly `20-35` internal questions and complex work with `35-50`.
- Show only a concise `8-15` item decision summary when visible reasoning is useful.
- Choose strong defaults for routine ambiguity and ask at most `1-3` high-impact questions.
- Preserve supplied templates, rules, wording, and hard constraints.
- Create `plan.md` before non-trivial direct execution.
- Require fresh verification for unstable facts and separate facts from inference.
- Replace requests for hidden chain-of-thought with auditable decision notes and evidence.

## Routing Model

| User intent | Result |
| --- | --- |
| Refine, rewrite, compare, or stress-test a prompt | Return a copy-ready refined prompt |
| Perform a non-trivial task | Clarify decisions, create `plan.md`, then execute |
| Ask a stable read-only question | Answer directly without prompt ceremony |
| Give an ambiguous request | Infer the safest useful track or ask one concise clarification |
| Say `纯prompt` or `只要prompt` | Return exactly one prompt block |
| Say `不要问` or `直接继续` | Resolve safe ambiguity internally and continue |

## Install

Clone the repository:

```bash
git clone https://github.com/kyoka-shuiyue/prompt-refiner.git
cd prompt-refiner
```

Copy the skill folder into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills/prompt-refiner
cp -R prompt-refiner/. ~/.codex/skills/prompt-refiner/
```

Windows PowerShell:

```powershell
$target = "$env:USERPROFILE\.codex\skills\prompt-refiner"
New-Item -ItemType Directory -Force $target | Out-Null
Copy-Item -Recurse -Force .\prompt-refiner\* $target
```

Restart Codex if the current environment does not hot-reload skills.

## Update

Pull the latest repository changes, then copy the skill folder again:

```bash
git pull --ff-only
mkdir -p ~/.codex/skills/prompt-refiner
cp -R prompt-refiner/. ~/.codex/skills/prompt-refiner/
```

Windows PowerShell:

```powershell
git pull --ff-only
$target = "$env:USERPROFILE\.codex\skills\prompt-refiner"
New-Item -ItemType Directory -Force $target | Out-Null
Copy-Item -Recurse -Force .\prompt-refiner\* $target
```

## Usage

Invoke the skill explicitly when needed:

```text
Use $prompt-refiner to improve this request:
Build a local knowledge-base tool that can search my files.
```

Prompt-only output:

```text
$prompt-refiner 纯prompt：完善这个本地 AI 桌面工具的开发需求。
```

Direct execution with safe defaults:

```text
$prompt-refiner 不要问，先自问自答，再在当前仓库完成这个功能。
```

The frontmatter description is intentionally broad enough for automatic routing, so Codex may also load the skill for vague or non-trivial work even without an explicit invocation.

## Workflow

1. Classify the request as prompt output, execution, direct answer, or ambiguous.
2. Self-grill intent, success criteria, scope, constraints, risks, and verification.
3. Resolve low-risk ambiguity with recommended defaults.
4. Surface only high-impact controversies or blockers.
5. Produce and stress-test the prompt, answer, or execution plan.
6. For non-trivial direct work, save `plan.md` before implementation.

## Repository Layout

```text
prompt-refiner/
  SKILL.md                  Core routing and refinement behavior
  agents/
    openai.yaml             Codex interface metadata
  references/
    patterns.md             Advanced handling patterns
evals/
  prompt-refiner-cases.jsonl
  rubric.md
  rubric.zh-CN.md
  forward-test-report.md
  forward-test-report.zh-CN.md
README.md
README.zh-CN.md
LICENSE
```

## Evaluation

The manual eval suite covers prompt rewriting, complex engineering prompts, direct execution, prompt-only overrides, no-follow-up behavior, current information, high-stakes requests, technical conflicts, candidate comparison, and hidden-thinking requests.

To evaluate a build:

1. Install the skill under test.
2. Run each request in `evals/prompt-refiner-cases.jsonl` in a fresh thread or agent context.
3. Score the response with [evals/rubric.md](evals/rubric.md).
4. Record independent results in the forward-test report.

Avoid giving the evaluating agent the expected answer. Provide only the skill path and the test request.

## Customization

The `SKILLOPT-SLEEP:LEARNED` block at the end of `SKILL.md` is designed for validated local preferences. Keep general behavior above the block and machine-managed preferences inside it. If you maintain a public fork, review learned entries before publishing because they may reflect personal workflows.

## Design Principles

- Executable clarity over decorative verbosity.
- Safe defaults over avoidable questioning.
- User constraints over invented requirements.
- Visible evidence over hidden chain-of-thought.
- Fresh primary sources over stale assumptions.
- Testable acceptance criteria over vague quality claims.

## License

MIT. See [LICENSE](LICENSE).
