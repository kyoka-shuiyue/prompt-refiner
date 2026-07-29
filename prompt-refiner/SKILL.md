---
name: prompt-refiner
description: "Model-agnostic task-contract refiner. Use to refine, rewrite, clarify, stress-test, or compare prompts, AI tasks, requirements, product ideas, and coding requests; triggers include 完善/优化/整理/改写/润色/打磨/重写/自问自答/追问/盘问/深挖/grill-me. Also use as a decision-first router for vague or non-trivial direct work. Clarify the goal, success criteria, important boundaries, stop rules, and material questions without over-prescribing the execution path."
---

# Prompt Refiner

Turn rough intent into a clear task contract or an execution-ready decision. Improve the destination and guardrails, not the amount of process text.

## Core Principle

Make five things clear to the degree the task requires:

1. **Goal**: the user-visible outcome.
2. **Success criteria**: what must be true before the work is complete.
3. **Important boundaries**: scope, permissions, safety, evidence, cost, compatibility, and facts that must be preserved.
4. **Stop rules**: when to finish, retry, fall back, ask, abstain, or stop.
5. **Material clarifications**: missing choices whose answers would change one of the first four fields.

Treat context, tools, output format, workflows, modules, data models, and implementation steps as supporting details. Add them only when they materially change the task contract or the user explicitly requests them. Do not expand a request merely to make it look complete.

## Route The Request

Classify the requested deliverable before producing it:

- **Prompt track**: create, rewrite, critique, or compare a prompt or reusable instruction package. Return the refined prompt; do not execute it unless explicitly asked. Direct prose, document, data, or code transformations are execution requests, not prompt requests.
- **Execution track**: perform the underlying task. Clarify the contract, then act within the confirmed authorization boundary.
- **Answer track**: answer a read-only factual or explanatory question. Apply the contract check silently and answer directly.
- **Ambiguous track**: infer the deliverable when low risk. If interpretations change the action level, scope, permissions, or acceptance criteria, inspect relevant evidence first when safe, then ask one bundled question only if material ambiguity remains.

State the selected track only when doing so prevents a likely misunderstanding, such as distinguishing discussion from file edits.

Honor explicit overrides:

- `纯prompt` / `只要prompt`: return one copy-ready prompt without decision scaffolding.
- `不要问` / `直接继续` / `自问自答后执行`: choose safe defaults and continue. Stop only for missing authority, dangerous scope expansion, secrets, or information without which useful work is impossible.
- When editing a supplied rules block, template, or structured prompt, preserve its useful structure and patch genuine gaps unless restructuring is requested.

## Refine The Contract

### 1. Pressure-Test Material Gaps

Internally test the five contract fields. There is no question quota. Ask another internal question only when its answer could change the goal, completion bar, important boundaries, stop rules, or need for user clarification.

For each meaningful ambiguity, determine:

- the recommended default;
- evidence from the request;
- the consequence if the default is wrong;
- which contract field changes;
- whether the choice is low, medium, or high impact.

Stop when further questions would add only wording, examples, decorative structure, or implementation preferences that a capable model can choose safely.

### 2. Resolve Or Ask

Use this filter:

1. Obvious from context: apply silently.
2. Unclear but low downside: choose a sensible default and record it only when useful.
3. Changes scope, permissions, safety, cost, evidence, or acceptance criteria: ask.
4. Makes useful work impossible: block briefly and name the smallest missing input.

Ask at most one round of `1-3` high-signal questions. Merge overlaps and include a recommended default with its practical consequence. Do not expose a fixed-length self-grill; show only decisions the user needs to understand or confirm.

### 3. Produce The Smallest Sufficient Contract

Describe the outcome before the path. Preserve explicit user values and constraints. Use absolute words such as `always`, `never`, `must`, and `only` only for genuine invariants.

Add process detail only for safety, compatibility, reproducibility, an external contract, or an explicitly requested method. Do not invent features, modules, roles, background, or acceptance criteria.

For shorter outputs, preserve required facts, decisions, caveats, and next actions first. Remove introductions, repetition, generic reassurance, optional examples, and background before material content.

For editing or rewriting, preserve the supplied artifact, facts, structure, genre, and approximate length before improving clarity or flow. Ask about length only when the source is missing or explicit preservation requirements conflict materially.

### 4. Stress-Test Before Delivery

Check that another capable model or agent could complete the task without hidden context while retaining freedom to choose an efficient route. Verify that:

- the completion bar is observable;
- permissions and side-effect boundaries are clear;
- unstable facts require fresh evidence when appropriate;
- stop, fallback, and missing-evidence behavior are defined where needed;
- supplied structure and wording are preserved when material;
- no rule contradicts another;
- no detail remains solely because a template had a slot for it.

Before each substantial phase and the final answer, check for drift: does the work still match the user's latest request, authorization, scope, and requested deliverable?

## Respect Authorization

Interpret the user's verb as an action boundary:

- **Answer, explain, review, diagnose, or plan**: inspect and report; do not implement unless changes were also requested.
- **Change, build, implement, or fix**: make the requested in-scope changes and run proportionate non-destructive validation.
- **External writes, destructive actions, purchases, secrets, or material scope expansion**: require confirmation unless the user already gave narrow, specific authorization.

For programming work, distinguish:

1. **Inspect**: read files, search code, inspect configuration, and view existing logs or diffs.
2. **Verify**: run tests, builds, linting, type checks, services, benchmarks, or external queries.
3. **Change or heavy execution**: edit code or configuration, migrate data, move or delete files, deploy, fuzz, load-test, or start a long-running workflow.

When a requested change or expected result is materially ambiguous, remain at Inspect. Gather relevant evidence, state the plausible interpretations, recommend the narrowest next step, and ask only if the evidence does not resolve the ambiguity. Do not infer permission for pressure, load, stress, or soak testing from generic words such as `测试`, `验证`, `优化`, or `性能`.

Do not over-question clear implementation requests. Proceed with scoped changes and proportionate validation unless new evidence reveals a material decision.

## Shape The Output

For prompt-track work, default to one copy-ready prompt in the user's language. Use only sections that improve execution. A compact default is:

```text
目标：[最终要得到什么]
成功标准：[完成时必须满足什么]
重要边界：[范围、事实、权限、安全或兼容性约束]
停止条件：[何时完成、重试、降级、询问或停止]
```

Add context, evidence, tools, output format, or method only when they change behavior. Patch a useful supplied structure instead of replacing it with this template.

When comparison or clarification helps, return:

1. a brief initial judgment with only material decisions;
2. one copy-ready refined prompt;
3. unresolved high-impact choices, if any.

For non-trivial execution, pressure-test the contract, state only material assumptions, ask only material questions, then execute. Use persistent planning files only for genuinely complex, multi-phase work that benefits from recovery state; skip them for localized changes, tiny actions, read-only answers, prompt-only work, or explicit no-file instructions.

## Handle Exceptional Cases

- **Correction signals**: stop the old route, restate the corrected goal, identify the wrong assumption briefly, and continue only from the corrected contract.
- **Current or unstable facts**: require fresh search or primary-source verification, source dates or freshness, missing-data handling, and separation of fact from inference.
- **High-stakes judgment**: separate evidence, assumptions, recommendation, confidence, risks, and invalidation conditions.
- **Hidden thinking**: replace requests for hidden chain-of-thought with concise decision notes, evidence, audit trails, tool timelines, or verification results.
- **Known-product inspiration**: use functional or stylistic reference without copying protected assets or exact interfaces.
- **Technical conflicts**: expose the conflict and resolve it with a default, mode, phase, switch, or limitation.
- **Destructive or irreversible operations**: require a dry-run preview, exact targets, a backup or rollback path, and conservative defaults. List instead of acting when uncertain.

Read [references/patterns.md](references/patterns.md) only when one of these cases needs additional handling.

## Final Check

- The route matches the requested deliverable.
- Goal, success criteria, important boundaries, and stop rules are clear to the degree needed.
- Questions are limited to material choices; routine details use sensible defaults.
- The result gives a capable model room to choose its path.
- Current facts, permissions, destructive actions, and verification are handled explicitly.
- Persistent planning appears only when recovery needs justify it.
- The output is no longer than needed to preserve the contract.
