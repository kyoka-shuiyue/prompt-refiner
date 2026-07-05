---
name: prompt-refiner
description: "Self-grilling prompt refiner. Use to refine/rewrite/polish/optimize/clarify/stress-test/compare prompts, AI tasks, requirements, product ideas, or coding requests; triggers: 完善/优化/整理/改写/润色/打磨/重写/自问自答/追问/盘问/深挖/grill-me. Internally self-interview deeply, default low-controversy choices into the prompt, expose only high-impact controversies, and create plan.md for non-trivial direct work."
---

# Prompt Refiner

Turn rough intent into a reusable, executable prompt.

This skill is prompt-focused `grill-me`: self-interview the request deeply, answer low-controversy questions with recommended defaults, resolve dependent decisions, then produce the strongest prompt or execution plan. Do not start a live back-and-forth interview for trivia, but do ask a short question round when a high-impact choice would materially change the result.

If the user asks you to perform the underlying task, do not jump straight to implementation for non-trivial work. Use the execution track: visible self-grill summary, one short question round when needed, write `plan.md`, then start work. For tiny, reversible, operationally clear tasks, use the workflow silently and execute.

## Core Workflow

### 1. Self-Grill First

Before output, internally ask and answer the important ambiguity questions.

Cover:

- task vs non-task
- desired outcome and acceptance criteria
- audience and use context
- output format and depth
- tool, platform, model, stack, file, or environment constraints
- required scope and excluded scope
- hard constraints vs preferences
- missing inputs and safe placeholders
- current-information or source requirements
- security, privacy, permissions, and destructive-action boundaries
- conflicts, tradeoffs, risks, and failure modes

Internal question budget:

- tiny or purely mechanical request: `4-8`
- simple prompt rewrite: `10-20`
- normal prompt/task refinement: `20-35`
- complex coding, product, agent, MCP, plugin, data, UI, research, or execution request: `35-50`
- explicit deep grilling, vague high-stakes work, or multi-system design: `50+` only if the extra questions keep changing the prompt or plan

For non-trivial work, default to at least `20` internal self-grill questions before deciding that the request is clear. Do not expose the full question list; expose a distilled decision summary that is substantial enough to be useful.

For each meaningful ambiguity, decide:

- recommended default
- evidence from the user request
- risk if wrong
- prompt impact
- controversy level: low, medium, or high

Do not expose hidden chain-of-thought. If showing the self-grill for non-trivial work, normally show `8-15` distilled decision bullets, not the full private reasoning trace.

### 2. Resolve Or Ask

Classify unresolved items:

- `safe assumption`: use silently.
- `recommended default`: choose it for the user, write it into the prompt or plan, and label it as an assumption only when helpful.
- `medium-impact ambiguity`: choose the strongest default when the user gave enough context; expose only if several defaults are genuinely plausible and consequential.
- `high-impact controversy`: expose as a user question with your recommended answer.
- `blocking question`: ask the user because no useful prompt can be produced without it.

Ask at most one round of `1-3` questions for blocking choices or high-impact controversies. Include your recommended answer for each question.

Prefer a good default over making the user decide everything. The user's time should be spent only on leverage points; the agent should absorb routine decisions.

Use this decision filter:

1. If the answer is obvious from the request, silently apply it.
2. If the answer is not obvious but the downside is small, choose the recommended default and write it into the prompt or plan.
3. If the answer changes architecture, scope, cost, permissions, data/source requirements, style direction, acceptance criteria, or safety posture, ask the user.
4. If several questions are worth asking, collapse them into the smallest set of `1-3` high-signal questions.

### 3. Choose The Track

Use `prompt track` when the user asks for a prompt, rewrite, critique, comparison, or reusable instruction package.

Use `execution track` when the user expects you to perform the underlying task, especially coding, file edits, local tools, web/UI work, research, MCP/plugin/agent work, data work, or any multi-step deliverable.

For execution track:

1. Run a deep internal self-grill first: normally `20-50` questions for non-trivial work. Show a substantial but distilled `自问自答压测摘要`, normally `8-15` decision bullets, with only the important decisions, recommended defaults, and prompt/plan impact. This is a visible decision record, not hidden chain-of-thought.
2. Ask `1-3` `争议/待确认问题` before implementation when answers could materially change scope, architecture, style, data/source requirements, permissions, cost, or acceptance criteria.
3. Write the low-controversy defaults into the prompt or `plan.md` so future agents inherit the decisions instead of re-asking.
4. After the question round is resolved, write a `plan.md` file before editing files or running a long tool chain. In repo threads, place it at the relevant repo/task root unless local conventions say otherwise. In projectless threads, place it under `work/plan.md` unless the user requested a user-facing deliverable.
5. Start executing the plan after `plan.md` exists.

Skip `plan.md` only for tiny one-step tasks, read-only answers, or when the user explicitly says not to create files.

### 4. Produce The Prompt

Default output is one copy-ready prompt, in the user's language, with no extra commentary.

Use this compact skeleton when helpful:

```text
任务：[要完成什么]
背景：[已知上下文]
目标：[成功结果]
要求：
1. ...
2. ...
约束：[范围、工具、数据、安全、风格、时间等]
输出格式：[答案结构或交付物]
质量标准/验收标准：[如何判断好坏]
```

### 5. Stress-Test The Draft

Before final output, attack the draft once:

- Would another agent understand and execute it without hidden context?
- Are concrete user constraints preserved instead of generalized?
- Did you invent major background, features, constraints, or success criteria?
- Are conflicts, safety boundaries, source requirements, and verification visible?
- If the user wanted direct execution, did you complete the execution track instead of only returning a prompt?

Revise only the parts that materially improve executability or safety.

## Output Modes

- `prompt only`: default for simple and normal requests.
- `prompt + brief notes`: use when assumptions or conflicts need a short explanation.
- `prompt + self-grill summary`: use when the user asks for 自问自答, 追问, 盘问, 深挖, grill, or stress-test.
- `prompt + controversial choices`: use for large coding/product/agent/tooling projects.
- `execution prep`: use for non-trivial direct-execution requests; show self-grill summary, ask high-impact questions, then create `plan.md` after answers.
- `plan.md + execution`: use after the question round is resolved; save the plan before implementing.
- `question first`: use only when blocked.
- `candidate evaluation`: use when comparing multiple prompts.

For large coding, product, agent, MCP, plugin, data platform, SaaS, or desktop-tool prompts, output:

1. `自问自答压测摘要`: key questions, recommended answers, and prompt impact.
2. `完善后的 Prompt`: the full copy-ready prompt.
3. `争议选择`: high-impact choices with recommended defaults.

Large engineering prompts should normally cover role, goal, users, workflows, modules, stack, data boundaries, permissions, safety, delivery scope, acceptance criteria, and verification.

For large engineering prompts, the visible `自问自答压测摘要` should be the distilled result of `20-50` internal questions, not merely a few shallow bullets. It should normally contain `8-15` meaningful decision bullets, enough to prove the request was stress-tested while still hiding private chain-of-thought.

## Special Handling

- Current or unstable facts: require fresh search or official/primary-source verification.
- High-stakes domains: separate facts, assumptions, judgment, confidence, and risk.
- Hidden thinking requests: ask for visible decision notes, audit trails, summaries, or evidence, not hidden chain-of-thought.
- Known-product inspiration: use as functional/style reference only; avoid copying protected assets or exact UI.
- Technical conflicts: preserve both intents with modes, switches, phases, defaults, or explicit limitations.
- Missing details: use placeholders or labeled assumptions; do not invent major background, features, or constraints.

## Reference Routing

Read `references/patterns.md` only when the request involves current information, high-stakes judgment, complex product/app/agent/developer-tool design, hidden-thinking wording, multiple prompt candidates, or technical conflicts.

Do not load examples by default. Prefer generating task-specific examples from the current request instead of relying on stored example files.

## Final Check

Before answering, verify:

- intent is preserved
- meaningful ambiguities were self-grilled deeply enough; non-trivial tasks normally used `20-50` internal questions
- low-controversy decisions were selected for the user and written into the prompt or plan
- major assumptions are safe, labeled, or exposed
- no invented major feature or context slipped in
- output language matches the user
- concrete constraints from the user are preserved
- result is reusable and executable
- visible questions are limited to true blockers or high-impact controversies
- complex engineering prompts include verification and controversy handling

## Rules

1. Internally grill before output.
2. For non-trivial requests, internally self-grill with `20-50` questions before treating the request as clear.
3. Answer low-controversy questions with recommended defaults and write those defaults into the prompt or plan.
4. Ask the user only for blocking decisions or distilled high-impact controversies before non-trivial execution.
5. Keep visible self-grill concise and decision-oriented.
6. When the user wants direct execution, use the execution track for non-trivial work and the silent workflow only for tiny, obvious tasks.
7. Output the refined prompt, not the underlying task answer, when refinement is the task.
8. For non-trivial local execution, create `plan.md` before implementation unless the user forbids files.

<!-- SKILLOPT-SLEEP:LEARNED START -->
## Learned preferences & procedures

_This block is maintained by SkillOpt-Sleep. Edits here are proposed offline, validated against your past tasks, and adopted only after you approve them. Hand-edits outside this block are never touched._

- For any refined prompt involving latest/current information, news, schedules, markets, sports, product facts, regulations, or explicit search/browse/verify wording, include the exact phrase '必须搜索' in the prompt's requirements or constraints.
- For any refined prompt involving sports predictions, match outcomes, scores, odds, gambling-adjacent analysis, or betting-adjacent decisions, include the exact phrase '不构成任何投注建议' as a safety constraint or disclaimer.
- OVERRIDE hidden-thinking handling: when a user asks for chain of thought, hidden reasoning, step-by-step inner thinking, or full thought process, refuse hidden chain-of-thought and require a section titled exactly '可见决策摘要' for concise, auditable decision notes.
<!-- SKILLOPT-SLEEP:LEARNED END -->
