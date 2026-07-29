# Refinement Patterns

Load this file only when a task needs handling beyond the core task-contract workflow.

## Output Overrides

### Pure Prompt

When the user says `纯prompt`, `只要prompt`, or requests a copy-ready prompt without commentary:

- return exactly one prompt block;
- preserve the supplied structure when useful;
- fold safe defaults into the prompt;
- include only sections that change behavior;
- omit decision scaffolding unless a missing input makes the prompt unusable.

### No Follow-Up

When the user says `不要问`, `直接继续`, or requests silent self-grilling:

- resolve low-risk ambiguity with sensible defaults;
- record consequential assumptions only when useful for verification or recovery;
- continue to the requested deliverable;
- stop only for missing permission, dangerous expansion, secrets, or truly blocking information.

No-follow-up changes how ambiguity is resolved. It does not authorize new external side effects or unsafe assumptions.

## Materiality Test

Before adding a question, section, example, tool rule, workflow, module, path, or implementation step, ask:

1. Would omitting it change the goal or completion bar?
2. Would omitting it create a permission, safety, evidence, cost, or compatibility risk?
3. Is it required for reproducibility, an external contract, or an explicitly requested method?
4. Does it define when to finish, retry, fall back, ask, or stop?

If every answer is no, omit it and let the executing model choose the path.

## Current Information

When the task depends on current news, prices, schedules, laws, versions, product specifications, model capabilities, APIs, company facts, or other unstable information, require:

- fresh search or official and primary-source verification;
- a source date or freshness window;
- explicit handling of missing or uncertain facts;
- separation between retrieved facts and inference;
- a retrieval stop rule so search does not continue only to improve wording or add nonessential detail.

## High-Stakes Judgment

For investing, medical, legal, safety, betting, or other risky uses:

- establish evidence before judgment;
- distinguish evidence, assumptions, recommendation, confidence, and uncertainty;
- state material risks and invalidation conditions;
- avoid implying professional certainty or guaranteed outcomes.

## Complex Product Or Engineering Work

Do not automatically expand a product or engineering request into a full specification. Add users, workflows, modules, stack, state, security, delivery phases, or named resources only when they materially affect the task contract.

For implementation plans, include enough detail to make dependencies, state transitions, failure behavior, privacy or security boundaries, and validation observable. Leave ordinary implementation choices to the executing model.

## Persistent Plan Gate

For genuinely complex, multi-phase direct execution that benefits from recovery state, confirm the task contract before implementation. Ask up to three high-impact questions only when safe defaults could materially change the result. Then persist:

- goal and success criteria;
- important boundaries and confirmed assumptions;
- scope and non-goals;
- a recovery-oriented roadmap;
- stop, fallback, safety, and rollback rules;
- verification.

Do not turn the plan into a speculative implementation specification. Skip persistent planning for clear localized changes, tiny actions, read-only answers, prompt-only work, or explicit no-file instructions. Use the host agent's native planning mechanism and paths rather than assuming a particular platform.

## Technical Conflicts

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
