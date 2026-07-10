---
name: prompt-refiner
description: "Self-grilling prompt and task refiner. Use to refine/rewrite/polish/optimize/clarify/stress-test/compare prompts, AI tasks, requirements, product ideas, or coding requests; triggers: 完善/优化/整理/改写/润色/打磨/重写/自问自答/追问/盘问/深挖/grill-me. Also use as a decision-first router for vague or non-trivial direct work. Distinguish prompt-only from execution, preserve supplied structure, default low-risk choices, ask only high-impact questions, honor 纯prompt/不要追问 overrides, and create plan.md before non-trivial direct execution."
---

# Prompt Refiner

Turn rough intent into either a reusable executable prompt or a decision-ready execution plan. Internally self-interview the request, absorb routine choices, surface only consequential controversies, and never expose hidden chain-of-thought.

## Routing Contract

Classify the request before producing anything:

- **Prompt track**: the user asks for a prompt, rewrite, critique, comparison, reusable instruction package, or says `纯prompt`. Return the refined prompt; do not execute it unless explicitly asked.
- **Execution track**: the user asks Codex to perform the underlying task, especially coding, file edits, local tools, UI, research, MCP/plugin/agent work, data work, or another multi-step deliverable.
- **Answer track**: the user asks a read-only factual or explanatory question. Apply the discipline silently and answer; do not create a prompt or `plan.md` merely to satisfy the skill.
- **Ambiguous track**: infer from the requested deliverable. Prefer execution only when the user clearly asks for an artifact or state change; otherwise return a prompt or concise clarification.

Explicit user overrides take precedence:

- `纯prompt` / `只要prompt`: return one copy-ready prompt block with no decision scaffolding or meta commentary. Preserve the full implementation framework.
- `不要问` / `直接继续` / `自问自答后执行`: self-grill silently, choose safe defaults, record them in the prompt or plan, and continue. Stop only for missing authority, safety boundaries, secrets, or information without which useful work is impossible.
- When editing a supplied rules block, template, or structured prompt, preserve its structure and replace or extend it in place unless the user asks for restructuring.

## Core Workflow

### 1. Self-Grill

Internally test intent, success criteria, audience, context, deliverable, depth, scope, non-goals, tools, platform/model/stack, source freshness, security, permissions, destructive actions, missing inputs, conflicts, failure modes, verification, and rollback.

Question budget is a pressure-test target, not output:

- tiny mechanical request: `4-8`
- simple prompt rewrite: `10-20`
- normal refinement: `20-35`
- complex product, coding, agent, MCP, plugin, data, UI, research, or execution task: `35-50`
- exceed `50` only when further questions still change the result

For each meaningful ambiguity, determine the recommended default, evidence from the request, downside if wrong, effect on the prompt/plan, and controversy level. For non-trivial work, expose only `8-15` distilled decisions when a visible summary is required; do not reveal the private question tree.

### 2. Resolve Or Ask

Use this filter:

1. Obvious from context: apply silently.
2. Unclear but low downside: choose the strongest default and write it into the prompt or plan.
3. Changes scope, architecture, style direction, cost, permissions, data/source requirements, safety, or acceptance criteria: ask.
4. No useful work is possible without it: block briefly.

Ask at most one round of `1-3` high-signal questions and include the recommended answer plus practical consequence. Merge overlapping questions. Do not ask routine preference questions that can be defaulted.

### 3. Produce And Stress-Test

Before delivery, verify that another agent can execute without hidden context; user constraints and original wording are preserved where important; assumptions are labeled; current facts require verification; conflicts and safety boundaries are visible; acceptance criteria are testable; and no major features or background were invented.

Revise only where executability, fidelity, or safety materially improves.

## Prompt Track

Default to one copy-ready prompt in the user's language. Use this skeleton only when it helps:

```text
任务：[要完成什么]
背景：[已知上下文]
目标：[成功结果]
要求：
1. ...
2. ...
约束：[范围、工具、数据、安全、风格、时间]
输出格式：[答案结构或交付物]
验收标准：[如何判断完成]
```

For large coding, product, agent, MCP, plugin, data platform, SaaS, or desktop-tool prompts, normally return:

1. `自问自答压测摘要`: `8-15` distilled decisions, recommended defaults, and prompt impact.
2. `完善后的 Prompt`: the full copy-ready prompt.
3. `争议选择`: only unresolved high-impact choices, each with a recommended default.

If the user requested `纯prompt`, omit sections 1 and 3 and fold resolved defaults into the prompt itself.

Large engineering prompts should cover role, goal, users, workflows, modules, stack, data/state boundaries, permissions, safety, delivery scope, non-goals, acceptance criteria, and verification. Do not execute the prompt unless the user also requests execution.

## Execution Track

For non-trivial direct work:

1. Run the deep self-grill.
2. Show a concise `自问自答压测摘要` of `8-15` decisions unless the user requested silent/no-scaffolding operation.
3. Ask `1-3` high-impact questions only when the user has not disabled follow-ups and safe defaults would materially risk divergence.
4. Write low-controversy defaults and confirmed choices to `plan.md`.
5. Save `plan.md` before editing files or starting a long tool chain, then execute it.

In repositories, place `plan.md` at the relevant task/repo root unless local conventions say otherwise. In projectless threads, use `work/plan.md`. Include goal, acceptance criteria, scope/non-goals, decisions, steps, risks, safety/rollback, and verification.

Skip `plan.md` for tiny one-step operations, read-only answers, or explicit no-file instructions.

## Special Handling

- **Current or unstable facts**: require fresh search or official/primary-source verification, source dates, missing-data handling, and fact/inference separation.
- **High-stakes domains**: separate facts, assumptions, judgment, confidence, risks, and invalidation conditions; do not imply professional certainty.
- **Hidden thinking requests**: replace hidden chain-of-thought with concise decision notes, evidence, audit trails, tool timelines, or verification results.
- **Known-product inspiration**: use functional or stylistic reference without copying protected assets or exact UI.
- **Technical conflicts**: resolve with explicit defaults, modes, phases, switches, or limitations rather than silently dropping one side.
- **Missing details**: use labeled assumptions or placeholders; do not invent major context, features, or constraints.

Read [references/patterns.md](references/patterns.md) when the task involves current information, high-stakes judgment, complex product/agent/developer-tool design, direct execution preparation, hidden-thinking wording, prompt comparison, technical conflicts, `纯prompt`, or no-follow-up operation.

## Final Check

- Correct track selected; prompt refinement did not accidentally execute the task.
- Non-trivial work used a deep enough self-grill; visible notes are decisions, not hidden reasoning.
- Low-risk defaults are written into the deliverable; visible questions are true leverage points.
- Supplied structures and concrete constraints remain intact.
- Output language and requested format match the user.
- Current facts, high-stakes claims, permissions, and verification are handled explicitly.
- Direct non-trivial execution has `plan.md` unless an allowed exception applies.

<!-- SKILLOPT-SLEEP:LEARNED START -->
## Learned preferences & procedures

_This block is maintained by SkillOpt-Sleep. Edits here are proposed offline, validated against your past tasks, and adopted only after you approve them. Hand-edits outside this block are never touched._

- For any refined prompt involving latest/current information, news, schedules, markets, sports, product facts, regulations, or explicit search/browse/verify wording, include the exact phrase '必须搜索' in the prompt's requirements or constraints.
- For any refined prompt involving sports predictions, match outcomes, scores, odds, gambling-adjacent analysis, or betting-adjacent decisions, include the exact phrase '不构成任何投注建议' as a safety constraint or disclaimer.
- OVERRIDE hidden-thinking handling: when a user asks for chain of thought, hidden reasoning, step-by-step inner thinking, or full thought process, refuse hidden chain-of-thought and require a section titled exactly '可见决策摘要' for concise, auditable decision notes.
<!-- SKILLOPT-SLEEP:LEARNED END -->
