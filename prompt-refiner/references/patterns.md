# Refinement Patterns

Load this file only when a task needs handling beyond the core task-contract workflow. Activation, routing, overrides (`纯prompt`, `继续`, `不要问`), final-version, handoff-brief, and exit rules live only in `SKILL.md`.

## Materiality Test

Before adding a question, section, example, tool rule, workflow, module, path, or implementation step, ask:

1. Would omitting it change the goal or completion bar?
2. Would omitting it create a permission, safety, evidence, cost, or compatibility risk?
3. Is it required for reproducibility, an external contract, or an explicitly requested method?
4. Does it define when to finish, retry, fall back, ask, or stop?

If every answer is no, omit it and let the executing model choose the path.

## Creative Refinement

For an explicitly requested refinement of clear creative intent, preserve the user's idea, audience, intended effect, and explicit constraints. Add only requirements whose absence is likely to cause a material failure in this deliverable:

- Derive observable quality criteria from the existing goal; do not promote a suggested feature into a user requirement.
- When goals genuinely compete, state a brief priority grounded in the request. An exhibition may prioritize atmosphere and discovery; a utility may prioritize clarity and task completion.
- Leave composition, colors, typography, imagery, structure, motion, tools, and implementation to the executor unless the user constrained them. Describe what an interaction should accomplish before specifying controls.
- Tie the few consequential criteria to a proportionate check of the actual artifact. Let the executor choose how to check; never claim verification that did not occur.

Prefer the original brief plus a compact addition. Remove generic praise, repeated adjectives, decorative structure, and unnecessary implementation instructions.

## Known-Product Inspiration

When the user references an existing product, use it as a functional or stylistic reference. Do not copy protected assets, brand marks, exact copy, or exact UI.

## Current Information

When the task depends on current news, prices, schedules, laws, versions, product specifications, model capabilities, APIs, company facts, or other unstable information, require:

- fresh search or official and primary-source verification;
- a source date or freshness window;
- explicit handling of missing or uncertain facts;
- separation between retrieved facts and inference;
- a retrieval stop rule so search does not continue only to improve phrasing or add nonessential detail.

## High-Stakes Judgment

For investing, medical, legal, safety, betting, or other risky use:

- establish evidence before judgment;
- distinguish evidence, assumptions, recommendation, confidence, and uncertainty;
- state material risks and invalidation conditions;
- avoid implying professional certainty.

## Complex Product Or Developer Tool

Do not automatically expand a product or engineering request into a full specification. Add users, workflows, modules, stack, state, security, delivery phases, or named resources only when they materially affect the task contract.

For implementation plans, include enough detail to make dependencies, state transitions, failure behavior, privacy or security boundaries, and validation observable. Leave ordinary implementation choices to the executing agent.

## Technical Conflict Handling

When requirements conflict, do not silently drop one side. State:

```text
潜在冲突：[A] 与 [B] 冲突，因为 [原因]。
推荐默认：[策略及理由]。
边界或模式：[默认行为、例外条件和不能承诺的结果]。
```

Prefer a default, mode, phase, switch, or limitation that preserves the user's real goal.

## Hidden Thinking Requests

Replace requests for complete hidden reasoning with visible evidence:

- decision notes;
- assumptions;
- audit trail;
- tool timeline;
- input/output summaries;
- test or verification results.

Do not ask for or promise hidden chain-of-thought.

## Candidate Selection

When comparing prompt candidates:

1. Prefer the candidate with the clearest goal, completion bar, material boundaries, and stop rules.
2. Penalize invented requirements, repeated rules, unnecessary process detail, and decorative structure.
3. Merge only details that materially improve execution.
4. Return one adopted prompt when the user wants a usable result.
