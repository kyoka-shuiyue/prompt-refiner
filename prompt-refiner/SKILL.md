---
name: prompt-refiner
description: "Self-grilling prompt refiner. Use to refine/rewrite/polish/optimize/clarify/stress-test/compare prompts, AI tasks, requirements, product ideas, or coding requests; triggers: 完善/优化/整理/改写/润色/打磨/重写/自问自答/追问/盘问/深挖/grill-me. Internally self-interview, choose defaults, resolve conflicts, ask only blockers."
---

# Prompt Refiner

Turn rough intent into a reusable, executable prompt.

This skill is prompt-focused `grill-me`: self-interview the request, answer the questions with recommended defaults, resolve dependent decisions, then produce the strongest prompt. Do not start a live back-and-forth interview unless the user explicitly asks for interactive grilling.

If the user asks you to perform the underlying task, use this workflow silently to shape the work, then do the task. Output a refined prompt only when prompt refinement is the task.

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

Question budget:

- simple request: `4-8`
- normal request: `8-14`
- complex request: `14-24`
- high ambiguity or explicit deep grilling: continue until more questions stop changing the prompt

For each meaningful ambiguity, decide:

- recommended default
- evidence from the user request
- risk if wrong
- prompt impact

Do not expose hidden chain-of-thought. If showing the self-grill, show a concise decision summary only.

### 2. Resolve Or Ask

Classify unresolved items:

- `safe assumption`: use silently.
- `recommended default`: write into the prompt or a brief note.
- `high-impact controversy`: expose with a recommendation.
- `blocking question`: ask the user because no useful prompt can be produced without it.

Ask at most one round of `1-3` questions, and only for blocking choices. Include your recommended answer for each question.

Prefer a good default over making the user decide everything.

### 3. Produce The Prompt

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

### 4. Stress-Test The Draft

Before final output, attack the draft once:

- Would another agent understand and execute it without hidden context?
- Are concrete user constraints preserved instead of generalized?
- Did you invent major background, features, constraints, or success criteria?
- Are conflicts, safety boundaries, source requirements, and verification visible?
- If the user wanted direct execution, are you accidentally returning a prompt instead?

Revise only the parts that materially improve executability or safety.

## Output Modes

- `prompt only`: default for simple and normal requests.
- `prompt + brief notes`: use when assumptions or conflicts need a short explanation.
- `prompt + self-grill summary`: use when the user asks for 自问自答, 追问, 盘问, 深挖, grill, or stress-test.
- `prompt + controversial choices`: use for large coding/product/agent/tooling projects.
- `question first`: use only when blocked.
- `candidate evaluation`: use when comparing multiple prompts.

For large coding, product, agent, MCP, plugin, data platform, SaaS, or desktop-tool prompts, output:

1. `自问自答压测摘要`: key questions, recommended answers, and prompt impact.
2. `完善后的 Prompt`: the full copy-ready prompt.
3. `争议选择`: high-impact choices with recommended defaults.

Large engineering prompts should normally cover role, goal, users, workflows, modules, stack, data boundaries, permissions, safety, delivery scope, acceptance criteria, and verification.

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
- meaningful ambiguities were self-grilled
- major assumptions are safe, labeled, or exposed
- no invented major feature or context slipped in
- output language matches the user
- concrete constraints from the user are preserved
- result is reusable and executable
- visible questions are limited to true blockers
- complex engineering prompts include verification and controversy handling

## Rules

1. Internally grill before output.
2. Answer your own questions with recommended defaults when safe.
3. Ask the user only for blocking high-impact decisions.
4. Keep visible self-grill concise and decision-oriented.
5. When the user wants direct execution, use the self-grill silently and do the task.
6. Output the refined prompt, not the underlying task answer, when refinement is the task.
