---
name: prompt-refiner
description: "Self-grilling prompt refiner. Use to refine/rewrite/polish/optimize/clarify/stress-test/compare prompts, AI tasks, requirements, product ideas, or coding requests; triggers: ??/??/??/??/??/??/??/????/??/??/??/grill-me. Internally self-interview, choose defaults, resolve conflicts, ask only blockers."
---

# Prompt Refiner

Turn rough intent into a reusable, executable prompt or task brief.

The root purpose is not cosmetic word polishing. The skill reduces ambiguity so AI systems do not guess: it turns vague, fragmented, conflicting, or under-specified intent into a clear unit of work.

This skill is prompt-focused `grill-me`: self-interview the request, answer the questions with recommended defaults, resolve dependent decisions, then produce the strongest prompt. Do not start a live back-and-forth interview unless the user explicitly asks for interactive grilling.

If the user asks you to perform the underlying task, use this workflow silently to shape the work, then do the task. Output a refined prompt only when prompt refinement is the task.

## Operating Principle

Before acting or outputting, internally answer:

- What does the user actually want to complete?
- What does success look like?
- Which constraints must be preserved exactly?
- Which missing information can be safely assumed?
- Which missing information is blocking and must be asked?
- Would another agent be able to execute the resulting prompt without hidden context?

The core value is lowering ambiguity and converting an idea into an executable task.

## Core Workflow

### 1. Classify The Request

Decide whether the user wants:

- `direct execution`: they want Codex to do the task. Refine silently, then execute.
- `prompt output`: they explicitly ask to optimize, rewrite, polish, deepen, stress-test, compare, or produce a prompt/task brief.
- `mixed request`: they want both a refined prompt and some direct help. Provide the prompt first, then the requested help if scope is clear.
- `exploration`: they are brainstorming and may benefit from a structured prompt plus concise options.

Do not turn every request into meta-prompting. When in doubt, preserve the user's action orientation.

### 2. Self-Grill First

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
- whether the refined prompt is reusable by another agent

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

### 3. Resolve Or Ask

Classify unresolved items:

- `safe assumption`: use silently.
- `recommended default`: write into the prompt or a brief note.
- `high-impact controversy`: expose with a recommendation.
- `blocking question`: ask the user because no useful prompt can be produced without it.

Ask at most one round of `1-3` questions, and only for blocking choices. Include your recommended answer for each question.

Prefer a good default over making the user decide everything.

### 4. Produce The Prompt

Default output is one copy-ready prompt, in the user's language, with no extra commentary.

Use this compact skeleton when helpful:

```text
??:[?????]
??:[?????]
??:[????]
??:
1. ...
2. ...
??:[??????????????????]
????:[????????]
????/????:[??????]
```

For task briefs that another agent should execute, prefer this expanded skeleton:

```text
??:[??????????]
??:[????????]
??:[??????????????????]
????:[??????]
??:[???? / ?????]
??:[?????????????????????????]
????:[????;??????]
??????:[????????????????]
????:[???????]
?????:[???????????????]
```

### 5. Stress-Test The Draft

Before final output, attack the draft once:

- Would another agent understand and execute it without hidden context?
- Are concrete user constraints preserved instead of generalized?
- Did you invent major background, features, constraints, or success criteria?
- Are conflicts, safety boundaries, source requirements, and verification visible?
- Does it distinguish hard requirements from preferences?
- Does it avoid unnecessary live questioning?
- If the user wanted direct execution, are you accidentally returning a prompt instead?

Revise only the parts that materially improve executability or safety.

## Output Modes

- `prompt only`: default for simple and normal prompt-refinement requests.
- `prompt + brief notes`: use when assumptions, defaults, or conflicts need a short explanation.
- `prompt + self-grill summary`: use when the user asks for ????, ??, ??, ??, grill, or stress-test.
- `prompt + controversial choices`: use for large coding/product/agent/tooling projects.
- `question first`: use only when blocked.
- `candidate evaluation`: use when comparing multiple prompts.
- `silent refinement + execution`: use when the user asks Codex to directly do the underlying task.

For large coding, product, agent, MCP, plugin, data platform, SaaS, or desktop-tool prompts, output:

1. `????????`: key questions, recommended answers, and prompt impact.
2. `???? Prompt`: the full copy-ready prompt.
3. `????`: high-impact choices with recommended defaults.

Large engineering prompts should normally cover role, goal, users, workflows, modules, stack, data boundaries, permissions, safety, delivery scope, acceptance criteria, and verification.

## Detail Dial

Choose detail level by task risk and ambiguity:

- `light`: small wording cleanups, messages, short asks. Keep the prompt compact and do not add process ceremony.
- `standard`: ordinary tasks, coding requests, analysis asks. Include goal, constraints, output format, assumptions, and acceptance criteria.
- `deep`: ambiguous, multi-step, high-impact, or explicit "???/??/??" requests. Include self-grill summary, scope boundaries, dependencies, risks, and verification.
- `operational`: prompts intended for another agent to execute in a repo or tool environment. Include files, commands, permissions, non-goals, tests, and handoff notes where applicable.

Do not add detail that only sounds sophisticated. Add detail that prevents wrong execution.

## Special Handling

- Current or unstable facts: require fresh search or official/primary-source verification.
- High-stakes domains: separate facts, assumptions, judgment, confidence, and risk.
- Hidden thinking requests: ask for visible decision notes, audit trails, summaries, or evidence, not hidden chain-of-thought.
- Known-product inspiration: use as functional/style reference only; avoid copying protected assets or exact UI.
- Technical conflicts: preserve both intents with modes, switches, phases, defaults, or explicit limitations.
- Missing details: use placeholders or labeled assumptions; do not invent major background, features, or constraints.
- Direct work requests: silently refine the task, then do it. Mention assumptions only when they affect the outcome.

## Reference Routing

Read `references/patterns.md` only when the request involves current information, high-stakes judgment, complex product/app/agent/developer-tool design, hidden-thinking wording, multiple prompt candidates, technical conflicts, explicit deepening, or "more detailed" refinement.

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
- direct execution requests were not mistakenly converted into prompt-output requests

## Rules

1. Internally grill before output.
2. Answer your own questions with recommended defaults when safe.
3. Ask the user only for blocking high-impact decisions.
4. Keep visible self-grill concise and decision-oriented.
5. When the user wants direct execution, use the self-grill silently and do the task.
6. Output the refined prompt, not the underlying task answer, when refinement is the task.
7. Optimize for executable clarity, not verbosity.
