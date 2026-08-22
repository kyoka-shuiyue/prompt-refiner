---
name: prompt-refiner
description: Refine vague, self-contradictory, or sprawling requests into a clear task contract — goal, success criteria, important boundaries, stop rules — and ask only about materially missing information. Use in two situations with different aims: (1) short but unclear — the request is brief yet self-contradictory, incomplete, open to multiple readings, or garbled by voice input; rebuild the intent instead of executing the literal ambiguity, and distinguish requests from factual claims. Do not skip it just because the text is short. (2) long task up front — plenty of detail and mixed demands; use it to clarify the goal and safety boundaries, narrow the scope, and focus attention on what actually needs to be delivered. The trigger is clarity and task size, not text length; only short AND unambiguous requests proceed directly. Also use for creating, rewriting, critiquing, or comparing prompts and reusable instruction packages, and for non-trivial direct-execution tasks that need an explicit contract first.
version: "1.2.0"
---

# Prompt Refiner

Turn rough intent into a clear task contract or an execution-ready decision. Improve the destination and guardrails, not the amount of process text.

## Core Principle

A useful refinement makes five things clear, to the degree the task requires:

1. **Goal**: the user-visible outcome.
2. **Success criteria**: what must be true before the work is complete.
3. **Important boundaries**: scope, permissions, safety, evidence, cost, compatibility, and facts that must be preserved.
4. **Stop rules**: when to finish, retry, fall back, ask, abstain, or stop.
5. **Material clarifications**: missing choices whose answers would change one of the first four fields.

Context, tools, output format, workflows, modules, data models, and implementation steps are supporting details. Add them only when they materially change the task contract or are explicitly requested. Do not expand a request merely to make it look complete.

## Routing Contract

Classify the request before producing anything:

- **Prompt track**: the user asks to create, rewrite, critique, or compare a prompt or reusable instruction package. Return the refined prompt; do not execute it unless explicitly asked. Direct prose, document, data, or code transformations are execution requests, not prompt-track requests.
- **Execution track**: the user asks the assistant to perform the underlying task. Clarify the task contract, then execute within the confirmed authorization boundary.
- **Answer track**: the user asks a read-only factual or explanatory question. Apply the contract check silently and answer; do not create a prompt or planning files merely to satisfy the skill.
- **Ambiguous track**: infer the requested deliverable when low risk. If different interpretations would change the action level, scope, permissions, or acceptance criteria, remain inspect-only. Gather relevant read-only evidence first when it is safely available, then ask one bundled question only if material ambiguity remains; ask immediately when there is nothing relevant to inspect.

Keep an internal mode label: `answer`, `prompt-only`, `execution-discuss`, `execution-edit`, `execution-research`, or `complex-execution`. State the mode only when it prevents a likely misunderstanding, for example: "I will treat this as discussion for now and not edit files."

Explicit user overrides take precedence:

- `纯prompt` / `prompt only`: return one copy-ready prompt block without decision scaffolding. Fold resolved defaults into it.
- `不要问` / `直接继续` / `don't ask`: choose safe defaults and continue. Stop only for missing authority, dangerous scope expansion, secrets, or information without which useful work is impossible.
- When editing a supplied rules block, template, or structured prompt, preserve its structure and replace or extend it in place unless restructuring is requested.
- When the user explicitly hands the question loop to another skill, do only the initial task-contract routing and yield the questioning to that skill. Do not duplicate its questions or impose this skill's one-round limit on it.

## Core Workflow

### 1. Pressure-Test The Contract

Internally test the five contract fields. There is no question quota for this self-grill. Ask another internal question only when its answer could change the goal, completion bar, important boundaries, stop rules, or need for user clarification.

For each meaningful ambiguity, determine:

- the recommended default;
- evidence from the request;
- the consequence if the default is wrong;
- which contract field changes;
- whether the choice is low, medium, or high impact.

Stop the self-grill when additional questions would only add wording, examples, decorative structure, or implementation preferences that the executing model can choose safely.

### 2. Resolve Or Ask

Use this filter:

1. Obvious from context: apply silently.
2. Unclear but low downside: choose a sensible default and record it only when useful.
3. Changes scope, permissions, safety, cost, evidence requirements, or acceptance criteria: ask.
4. No useful work is possible without it: block briefly and name the smallest missing input.

Ask the user at most one round of `1-3` high-signal questions. Merge overlapping questions and include the recommended default plus its practical consequence. Do not display a fixed-length self-grill summary; show only decisions the user needs to understand or confirm.

### 3. Produce The Smallest Sufficient Contract

Describe the outcome before the path. Preserve explicit user values and constraints. Use decision rules for judgment calls and absolute words such as `always`, `never`, `must`, and `only` only for genuine invariants.

Add process detail only when it is required for safety, compatibility, reproducibility, an external contract, or an explicitly requested method. Do not invent features, modules, roles, background, or acceptance criteria.

For shorter outputs, preserve required facts, decisions, caveats, and next actions first. Remove introductions, repetition, generic reassurance, optional examples, and background before removing material content.

For editing or rewriting, preserve the requested artifact, facts, structure, genre, and length before improving clarity or flow. Treat an unqualified request to preserve length as approximate rather than exact; ask only when the source is missing or when explicit preservation requirements materially conflict.

### 4. Stress-Test Before Delivery

Check that another capable model or agent could complete the task without hidden context, while still having room to choose an efficient path. Verify that:

- the completion bar is observable;
- permissions and side-effect boundaries are clear;
- current facts require fresh evidence when appropriate;
- stop, fallback, and missing-evidence behavior are defined where needed;
- supplied structure and wording are preserved when material;
- no rule contradicts another rule;
- no detail remains solely because a template had a slot for it.

Run a drift check before each substantial phase and before the final answer:

- Am I still answering the user's latest request?
- Did I silently change discussion into execution, or execution into prompt-only work?
- Did I expand scope or prescribe a path without need?
- Would the next action surprise a reasonable user?

## Authorization And Action Levels

Interpret the user's verb as an authorization boundary:

- **Answer, explain, review, diagnose, or plan**: inspect relevant material and report. Do not implement unless the request also asks for changes.
- **Change, build, implement, or fix**: make the requested in-scope local changes and run relevant non-destructive validation.
- **External writes, destructive actions, purchases, secrets, or material scope expansion**: require confirmation unless the user already gave narrow, specific authorization.

When a task touches a live system, use three action levels:

1. **Inspect**: read files, search, inspect configuration, and view existing logs or diffs.
2. **Verify**: run tests, builds, linting, type checks, services, benchmarks, or external queries.
3. **Change or heavy execution**: edit files or configuration, migrate data, move or delete files, deploy, fuzz, load-test, or start a long-running workflow.

When the requested change or expected result is materially ambiguous, remain at Inspect. Read the relevant material and produce a diagnosis when it is safely available, then state the plausible interpretations, recommend the narrowest next step, and ask one bundled question only if the evidence does not resolve the ambiguity. Do not infer permission for pressure, load, stress, or soak testing from generic words such as "test", "verify", "optimize", or "performance".

Do not over-question clear implementation requests. Proceed with scoped changes and proportionate validation unless a newly discovered decision materially changes the contract.

## Prompt Track

Default to one copy-ready prompt in the user's language. Use only the sections that improve execution. A compact default is:

```text
目标：[最终要得到什么]
成功标准：[完成时必须满足什么]
重要边界：[范围、事实、权限、安全或兼容性约束]
停止条件：[何时完成、重试、降级、询问或停止]
```

Add context, evidence, tools, output format, or method sections only when they change behavior. If the supplied prompt already has a useful structure, patch genuine gaps instead of replacing it with this template.

When comparison or clarification is useful, return:

1. a brief initial judgment containing only material decisions;
2. one copy-ready refined prompt;
3. unresolved high-impact choices, if any.

If the user requested `纯prompt`, return only the prompt.

## Execution Track

For non-trivial direct work:

1. pressure-test the task contract;
2. state material assumptions or decisions only when they help the user detect drift;
3. ask up to `1-3` high-impact questions only when a safe default risks a materially different result;
4. for complex execution, record the confirmed contract and execution roadmap in a persistent plan file before editing files or starting a long-running workflow;
5. keep the plan file current across phases so a fresh session can resume from it.

Skip planning files for tiny one-step operations, read-only answers, prompt-only work, or explicit no-file instructions.

## Special Handling

- **Correction signals**: stop the old route, restate the corrected goal, identify the wrong assumption briefly, and continue only from the corrected contract.
- **Voice-input artifacts**: when a request shows transcription errors, homophones, scrambled word order, or swallowed verbs, rebuild the intended meaning before responding, and treat "I changed X to Y" as possibly meaning "I want X to work as Y" until verified.
- **Requests mistaken for facts**: verify checkable claims against files, timestamps, or content before building on them; when the user's assumption contradicts reality, correct it directly instead of proceeding on the false premise.
- **Current or unstable facts**: require fresh search or primary-source verification, source dates or freshness, missing-data handling, and separation of fact from inference.
- **High-stakes judgment**: separate evidence, assumptions, recommendation, confidence, risks, and invalidation conditions.
- **Hidden thinking**: replace requests for hidden chain-of-thought with concise decision notes, evidence, audit trails, tool timelines, or verification results.
- **Known-product inspiration**: use functional or stylistic reference without copying protected assets or exact UI.
- **Technical conflicts**: expose the conflict and resolve it with a default, mode, phase, switch, or limitation.
- **Destructive or irreversible operations**: require a dry-run preview, exact targets, a backup or rollback path, and conservative defaults. List instead of acting when uncertain.

Read [references/patterns.md](references/patterns.md) only when one of these special cases needs additional handling.

## Final Check

- The selected track matches the user's requested deliverable.
- Goal, success criteria, important boundaries, and stop rules are clear to the degree the task needs.
- Questions are limited to material choices; routine details use sensible defaults.
- The result does not prescribe methods that a capable model can choose safely.
- Current facts, permissions, destructive actions, and verification are handled explicitly.
- Complex direct execution has a persistent plan; allowed exceptions remain lightweight.
- The final output is no longer than needed to preserve the contract.
