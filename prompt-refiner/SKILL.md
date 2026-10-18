---
name: prompt-refiner
description: 澄清语言混乱、意图不明、前后矛盾或交付目标存在实质歧义的请求；重大歧义时给出选项询问，目标需补全时制定匹配任务规模的高要求、明确且可验收目标。也用于用户明确要求创建、改写、优化、点评或比较 Prompt／提示词／系统提示词。明确任务、自由创作、仅缺输入或环境未知时默认跳过；不因长度、复杂度或未写验收标准而触发。目标与关键边界明确、重大冲突解除后结束精炼，转入对应执行流程。
metadata:
  version: "1.3.0"
---

# Prompt Refiner

Turn rough intent into a clear task contract or an execution-ready decision. Improve the destination and guardrails, not the amount of process text.

This skill owns intent clarification, important boundaries, and completion criteria. Planning, progress tracking, implementation, and artifact validation belong to the underlying task's workflow; define relevant verification requirements without taking over their execution or requiring a plan file.

## Core Principle

A useful refinement makes five things clear:

1. **Goal**: the user-visible outcome.
2. **Success criteria**: what must be true before the work is complete.
3. **Important boundaries**: scope, permissions, safety, evidence, cost, compatibility, and facts that must be preserved.
4. **Stop rules**: when to finish, retry, fall back, ask, abstain, or stop.
5. **Material clarifications**: missing choices whose answers would change one of the first four fields.

Context, tools, output format, workflows, modules, data models, and implementation steps are supporting details. Add them only when they materially change the task contract or are explicitly requested. Do not expand a request merely to make it look complete.

## Activation Gate

This is the single definition of when the skill applies; later sections assume it.

**Activate** for:

- language disorder that obscures meaning;
- unclear intent, or unresolved contradictions in intent;
- a materially unclear deliverable: the essential artifact or required behavior cannot be reliably inferred, or plausible interpretations would materially change the result;
- an explicit request to create, rewrite, optimize, critique, or compare a prompt, or an explicit invocation of this skill.

**Do not activate** for detailed and explicit requests, clear simple requests, or clear creative requests with ample freedom. Length, complexity, an open solution space, the word `prompt` alone, missing acceptance criteria, or unspecified style, layout, technique, or optional features are not triggers. Correct obvious transcription errors from context without activating.

**Missing input is not unclear intent.** If the task is clear but the source text, file, data, or prerequisite is missing, locate it within the authorized context or request the smallest missing input directly. For example, "翻译这篇文章" without an article needs the original text, not an intent interview. Inspect an unknown environment through the task's normal workflow. Activate only if resolving such a gap reveals a material choice about the goal, scope, or acceptable result.

## Routing Contract

Classify the request before producing anything:

- **Prompt track**: the user asks to create, rewrite, critique, or compare a prompt or reusable instruction package, or says `纯prompt`. Return the refined prompt; do not execute it unless explicitly asked. Direct prose, document, data, or code transformations are execution requests, not prompt-track requests.
- **Execution track**: the user asks the agent to perform the underlying task. Resolve material ambiguity, then resume the underlying workflow within the confirmed authorization; refinement prepares the work and does not replace the deliverable.
- **Answer track**: the user asks a read-only factual or explanatory question. Apply the contract check silently and answer; do not create a prompt or planning files.
- **Ambiguous track**: infer minor gaps when low risk. If the basic intent cannot be reliably identified or remains contradictory after checking context, gather relevant read-only evidence first, then ask with concrete interpretations (see Resolve Or Ask). Stay inspect-only for actions that depend on the unresolved choice.

State which track you chose only when it prevents a likely misunderstanding, for example: "我先按讨论方案处理，暂不改文件。"

Explicit user overrides take precedence:

- `纯prompt` / `只要prompt`: return one copy-ready prompt block without decision scaffolding, unless a missing input would make the prompt unusable. Fold resolved defaults into it.
- `继续` / `直接继续`: resume the agreed goal within the existing authorization. This does not waive clarification for a newly discovered material ambiguity or contradiction. Earlier explicit no-question preferences or delegated choices still apply within their original scope.
- `不要问` / `你决定` / `自问自答后执行`: choose safe defaults for the delegated decisions and continue within the authorized scope. Stop only for missing authority, dangerous scope expansion, secrets, or information without which useful work is impossible. This never authorizes new external side effects.
- When editing a supplied rules block, template, or structured prompt, preserve its structure and replace or extend it in place unless restructuring is requested.
- When the user explicitly invokes `grill-me`, do only the initial routing, then yield the question loop to that skill without duplicating its questions.

## Core Workflow

Pick the branch that matches why the skill activated:

- **Unclear or contradictory intent**: run steps 1–4.
- **Clear direction, materially unclear deliverable**: run steps 1–4; step 3 defines the delivery goal.
- **Explicitly requested refinement of clear creative intent**: use Creative Refinement below; steps 1–4 only for real uncertainty or complexity.
- **Already sufficient**: proceed with the requested deliverable. Invoking the skill does not require expansion.

### 1. Pressure-Test The Contract

Internally test the five contract fields. There is no question quota. Ask another internal question only when its answer could change the goal, completion bar, boundaries, stop rules, or need for user clarification.

For each meaningful ambiguity, determine the recommended default, the evidence for it, the consequence if it is wrong, which contract field changes, and whether the impact is low, medium, or high.

Stop when further questions would only add wording, examples, decorative structure, or implementation preferences the executor can choose safely.

### 2. Resolve Or Ask

Use this filter:

1. Obvious from context: apply silently.
2. Unclear but low downside: choose a sensible default and record it only when useful.
3. An unresolved choice would materially change scope, permissions, safety, cost, evidence requirements, or what the user would accept, and cannot be safely inferred: ask. Deriving a necessary quality criterion from an explicit goal does not by itself require confirmation.
4. No useful work is possible without it: block briefly and name the smallest missing input.

When you ask, default to one compact round on the most consequential choice. Offer `2-3` concrete interpretations or priority options with their practical differences, recommend one only when the request supports it, and allow a free-text correction. Ask for the intended outcome or which conflicting goal wins, not for a rewritten request. Merge overlapping questions.

If the reply leaves material ambiguity or introduces a new contradiction, ask the smallest necessary follow-up; one round is a default, not a cap. A latest correction that clearly replaces an earlier instruction is already resolved. Do not repeat answered choices or ask about routine details to fill a template. Show the user only decisions they need to understand or confirm, not a self-grill summary.

### 3. Produce The Smallest Sufficient Contract

Describe the outcome before the path. Preserve explicit user values and constraints. Use decision rules for judgment calls; reserve `always`, `never`, `must`, and `only` for genuine invariants.

**Defining an unclear delivery goal.** Derive a high-standard, explicit goal from the user's purpose and context: the artifact and intended use, essential content or behavior, relevant quality requirements, scope boundaries, and observable acceptance checks. Replace vague aspirations such as "high quality" with concrete completion conditions. Scale detail to the task: a few sentences for a simple task, detailed requirements and acceptance evidence for a complex deliverable. Label consequential defaults as your proposal so the user can spot a wrong interpretation. If defining the goal means choosing between materially different intentions, ask first (step 2); otherwise carry the goal forward without a separate approval. Explicit goals do not need rewriting.

High standards mean a complete, usable, verifiable result within scope. They do not justify invented facts, product scope, business commitments, modules, roles, extra features, fixed word quotas, or prescribed creative methods. Keep optional features and implementation preferences replaceable. Add process detail only when required for safety, compatibility, reproducibility, an external contract, or an explicitly requested method.

For shorter outputs, keep required facts, decisions, caveats, and next actions; cut introductions, repetition, reassurance, optional examples, and background first.

For editing or rewriting, preserve the requested artifact, facts, structure, genre, and length before improving clarity. Treat an unqualified request to preserve length as approximate.

### 4. Stress-Test Before Delivery

Check that another capable model could complete the task without hidden context, while still choosing its own efficient path:

- the completion bar is observable;
- permissions and side-effect boundaries are clear;
- current facts require fresh evidence when appropriate;
- stop, fallback, and missing-evidence behavior are defined where needed;
- supplied structure and wording are preserved when material;
- no rule contradicts another, and no detail remains only because a template had a slot for it;
- you are still answering the latest request, have not silently switched between discussion, execution, and prompt-only work, and the next action would not surprise a reasonable user.

### Creative Refinement

For an explicitly requested refinement of clear creative intent, preserve the user's idea, audience, intended effect, and explicit constraints. Add only requirements whose absence is likely to cause a material failure in this deliverable:

- Derive observable quality criteria from the existing goal; do not promote a suggested feature into a user requirement.
- When goals genuinely compete, state a brief priority grounded in the request. An exhibition may prioritize atmosphere and discovery; a utility may prioritize clarity and task completion.
- Leave composition, colors, typography, imagery, structure, motion, tools, and implementation to the executor unless the user constrained them. Describe what an interaction should accomplish before specifying controls.
- Tie the few consequential criteria to a proportionate check of the actual artifact, such as completing the main journey on the requested devices. Let the executor choose how to check; never claim verification that did not occur.

Prefer the original brief plus a compact addition; the prompt template below is optional. Remove generic praise, repeated adjectives, decorative structure, and unnecessary implementation instructions.

## Ambiguous Change Requests

Answer, explain, review, diagnose, or plan requests are read-only; change, build, implement, or fix requests authorize in-scope local edits with proportionate validation. External writes, destructive actions, purchases, secrets, or material scope expansion still need confirmation unless already narrowly authorized. Beyond that general rule, this skill adds:

- When the requested change or expected result is materially ambiguous, stay at inspection: read relevant material, diagnose when safely possible, state the plausible interpretations, recommend the narrowest next step, and ask one bundled question only if evidence does not resolve it.
- Generic words such as `测试`, `验证`, `优化`, or `性能` do not authorize pressure, load, stress, or soak testing.
- Do not over-question clear implementation requests.

## Prompt Track

Default to one copy-ready prompt in the user's language, using only sections that improve execution. A compact default:

```text
目标：[最终要得到什么]
成功标准：[完成时必须满足什么]
重要边界：[范围、事实、权限、安全或兼容性约束]
停止条件：[何时完成、重试、降级、询问或停止]
```

Resolved clarifications are folded into these fields. Add context, evidence, tools, output format, or method sections only when they change behavior. If the supplied prompt already has a useful structure, patch genuine gaps instead of replacing it.

When comparison or clarification is useful, return a brief judgment containing only material decisions, one copy-ready refined prompt, and any unresolved high-impact choices. For `纯prompt`, return only the prompt.

## Handoff, Exit, And Re-entry

End refinement when the artifact or outcome is identifiable, important boundaries and necessary completion criteria are clear enough for the task, and material intent conflicts are resolved. Safely replaceable defaults may stay open; missing inputs can still block execution without keeping refinement active.

- **Prompt track**: deliver the prompt or critique and stop.
- **Execution track**: state consequential assumptions when they help the user spot drift, then carry the goal, boundaries, and completion criteria into the underlying workflow and continue authorized work in the same turn. That workflow decides planning, progress, implementation, and validation. Use `planning-with-files` only when it is actually available and its own triggers apply. Do not create a plan file or ask for a separate refinement approval.

Re-enter only for an explicit new refinement request, or a material change, ambiguity, or conflict in scope, authorization, or acceptable results that context cannot safely resolve. Phase transitions, missing files, unknown environments, and technical failures are not reasons to reopen. Reuse confirmed choices; ordinary task questions and verification are unaffected.

## Special Handling

- **Correction signals**: stop the old route, restate the corrected goal, name the wrong assumption briefly, and continue only from the corrected contract.
- **Current or unstable facts**: require fresh search or primary-source verification, freshness dates, missing-data handling, and separation of fact from inference.
- **High-stakes judgment**: separate evidence, assumptions, recommendation, confidence, risks, and invalidation conditions.
- **Hidden thinking**: replace requests for hidden chain-of-thought with concise decision notes, evidence, audit trails, tool timelines, or verification results.
- **Known-product inspiration**: use functional or stylistic reference without copying protected assets or exact UI.
- **Technical conflicts**: expose the conflict and resolve it with a default, mode, phase, switch, or limitation.
- **Destructive or irreversible operations**: require a dry-run preview, exact targets, a backup or rollback path, and conservative defaults. List instead of acting when uncertain.

Read [references/patterns.md](references/patterns.md) only when one of these cases, a complex product or developer-tool request, a materiality doubt, or a prompt-candidate comparison needs more detail.

## Final Check

- The track matches the requested deliverable.
- The five contract fields are clear to the degree the task needs.
- Questions cover only material choices; severe ambiguity got concrete options unless questions were waived.
- Nothing prescribes methods a capable model can choose safely.
- The output is no longer than needed to preserve the contract.
