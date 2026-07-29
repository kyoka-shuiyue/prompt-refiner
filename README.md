# prompt-refiner

[English](README.md) | [简体中文](README.zh-CN.md)

`prompt-refiner` is a model-agnostic skill for turning rough intent into a clear task contract. It helps capable AI models understand the destination and guardrails while leaving them room to choose the best route.

It is not a prompt-lengthener or a fixed questionnaire. It clarifies only what materially affects the work:

1. **Goal**: the user-visible outcome.
2. **Success criteria**: what must be true when the task is complete.
3. **Important boundaries**: scope, facts, permissions, safety, evidence, cost, and compatibility.
4. **Stop rules**: when to finish, retry, fall back, ask, abstain, or stop.
5. **Material clarifications**: unresolved choices that would change the first four items.

Everything else, including tools, modules, workflow, output structure, and implementation steps, is added only when it changes the task contract or the user explicitly asks for it.

## Why This Approach

As models become more capable, prompts benefit less from prescribing every step and more from defining the right outcome, evidence bar, boundaries, and stopping behavior. `prompt-refiner` therefore aims for the smallest sufficient contract:

- clarify the destination without choosing every road;
- use safe defaults for low-impact ambiguity;
- ask only questions whose answers materially change the task;
- preserve the user's facts, structure, intent, and authorization boundary;
- require fresh evidence for unstable claims;
- make completion and failure behavior observable;
- avoid invented requirements and decorative process.

There is no fixed self-question count, visible-summary quota, or mandatory planning artifact. The amount of refinement scales with the decision risk and complexity of the request.

## Routing Behavior

| User intent | Result |
| --- | --- |
| Refine, rewrite, compare, or stress-test a prompt | Return a copy-ready refined prompt |
| Perform the underlying task | Clarify material decisions, then execute within authorization |
| Ask a read-only question | Answer directly without prompt ceremony |
| Give an ambiguous request | Inspect safely, infer low-risk defaults, or ask one bundled clarification |
| Say `纯prompt` or `只要prompt` | Return exactly one prompt block |
| Say `不要问` or `直接继续` | Resolve safe ambiguity internally and continue |

## Install

Clone the repository:

```bash
git clone https://github.com/kyoka-shuiyue/prompt-refiner.git
cd prompt-refiner
```

Copy the canonical `prompt-refiner/` folder into the skill directory required by your AI agent or skill host:

```bash
cp -R prompt-refiner /path/to/your-agent/skills/
```

Windows PowerShell:

```powershell
$skillsRoot = "C:\path\to\your-agent\skills"
Copy-Item -Recurse -Force .\prompt-refiner (Join-Path $skillsRoot "prompt-refiner")
```

Restart or reload the host if it does not detect skills automatically. The host must support `SKILL.md`-style skills; consult its documentation for the exact discovery directory and invocation syntax.

To update, run `git pull --ff-only` in this repository and copy the canonical folder again.

## Usage

Explicit invocation:

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
$prompt-refiner 不要问，使用安全默认值，在当前仓库完成这个功能并验证结果。
```

Hosts that support implicit skill routing may also load the skill automatically from its frontmatter description.

## How It Works

1. Identify whether the user wants a prompt, direct execution, a read-only answer, or clarification.
2. Pressure-test the five task-contract fields without a fixed question quota.
3. Resolve low-impact ambiguity with sensible defaults.
4. Ask at most one round of `1-3` questions only when the answer changes scope, permissions, safety, evidence, cost, or acceptance criteria.
5. Produce the smallest sufficient prompt, answer, or execution contract.
6. Stress-test for observable completion, authorization, evidence freshness, stop behavior, contradictions, and unnecessary prescription.

Persistent planning is reserved for genuinely complex, multi-phase execution that benefits from recovery state. Clear localized work, read-only answers, and prompt-only tasks stay lightweight.

## Repository Layout

```text
prompt-refiner/
  SKILL.md                  Canonical model-agnostic skill
  agents/
    openai.yaml             Optional interface metadata for compatible hosts
  references/
    patterns.md             Special-case refinement patterns
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

The repository contains one canonical skill. Files under `agents/` are host-facing metadata for that same skill, not separate model or client versions.

## Evaluation

The manual suite covers clear rewrites, vague product requests, current information, ambiguous code changes, scoped implementation, destructive operations, high-stakes judgment, hidden-thinking requests, technical conflicts, output overrides, structure preservation, and authorization boundaries.

To evaluate a revision:

1. Load the skill under test in a fresh agent context.
2. Run each request in [evals/prompt-refiner-cases.jsonl](evals/prompt-refiner-cases.jsonl) without revealing its expectations.
3. Score the result with [evals/rubric.md](evals/rubric.md).
4. Record the exact revision, method, limitations, and results in the forward-test report.

The rubric rewards goal clarity, success criteria, important boundaries, stop rules, clarification discipline, path autonomy, and information density. It does not reward verbosity or hidden reasoning.

## Design Principles

- Task contracts over exhaustive specifications.
- Observable success over vague quality claims.
- Safe defaults over avoidable questions.
- Important boundaries over prescribed implementation paths.
- Fresh evidence over stale assumptions.
- Visible decisions and verification over hidden chain-of-thought.
- Stop when the contract is sufficient.

## License

MIT. See [LICENSE](LICENSE).
