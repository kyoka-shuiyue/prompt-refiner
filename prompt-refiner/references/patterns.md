# Refinement Patterns

Load this file only for prompt refinements that need extra handling beyond the core workflow.

## Contents

- Output overrides
- Self-grill decision record and funnel
- Current/high-stakes information
- Complex product and execution planning
- Conflicts, hidden thinking, and candidate selection

## Output Overrides

### Pure Prompt

When the user says `纯prompt`, `只要prompt`, or asks for a copy-ready prompt without commentary:

- return exactly one prompt block
- preserve the full task framework, constraints, acceptance criteria, and verification
- fold safe defaults into the prompt instead of explaining them outside it
- preserve the supplied rules/template structure when one exists
- omit self-grill summaries, recommendation scaffolding, and controversy lists unless a missing input makes the prompt unusable

### No Follow-Up

When the user says `不要问`, `直接继续`, or requests silent self-grilling:

- answer low- and medium-impact ambiguities with recommended defaults
- record consequential assumptions in the prompt or `plan.md`
- continue to the requested deliverable
- stop only for missing permission, dangerous scope expansion, secrets, or truly blocking information

No-follow-up changes how ambiguity is resolved; it does not authorize new external side effects or unsafe assumptions.

## Self-Grill Decision Record

For complex prompts, internally reduce ambiguity into this record:

```text
Question: [what is uncertain?]
Recommended default: [best answer]
Evidence: [what in the user request supports it]
Risk if wrong: [what changes if false]
Prompt impact: [what to write into the prompt]
Controversy level: [low | medium | high]
```

Expose only the decisions that materially change scope, cost, safety, architecture, or output quality.

## Deep Self-Grill Funnel

For non-trivial prompt refinement or execution prep, internally ask `20-50` self-grill questions. The goal is not to show every question; the goal is to pressure-test the prompt until routine decisions have been absorbed and only meaningful controversies remain. The visible summary should normally include `8-15` distilled decisions for substantial tasks.

Sort the answers into this funnel:

- `default into prompt`: low-controversy details where a strong default exists. Choose for the user and write the decision into the prompt or plan.
- `briefly note`: medium-impact assumptions that may matter later but do not require a user decision now.
- `ask user`: high-impact controversies that change scope, architecture, style, source requirements, permissions, cost, safety posture, or acceptance criteria.
- `block`: missing information without which no useful prompt or plan can be produced.

When too many questions qualify for `ask user`, merge them into at most `1-3` sharp questions. Each question should include the recommended answer and the practical consequence of choosing it.

## Current Information

When the prompt depends on latest news, prices, schedules, laws, versions, product specs, model capabilities, APIs, company facts, or other unstable facts, require:

- fresh search or official/primary-source verification
- source dates or freshness window
- explicit treatment of missing or uncertain facts
- separation between facts and inference

## High-Stakes Judgment

For investing, medical, legal, safety, betting, or other risky use:

- require facts before judgment
- distinguish evidence, assumptions, recommendations, confidence, and uncertainty
- include concise risk language
- avoid implying certainty or professional authority

## Complex Product Or Developer Tool

For apps, agents, MCP/tooling systems, plugins, SaaS, dashboards, data platforms, and desktop tools, expand vague requests into:

- role and task
- product/engineering goal
- target users and use context
- core workflows
- required modules
- configuration surfaces
- data model or state boundaries
- security and permission boundaries
- delivery scope and non-goals
- acceptance criteria and verification
- known risks and controversial choices

## Execution Track Plan Gate

When `prompt-refiner` is being used before direct task execution, do not jump straight to implementation for substantial work. First run the deep self-grill funnel, expose a concise decision summary, then ask up to three high-impact questions if the answer changes scope, architecture, style, data/source requirements, permissions, cost, or acceptance criteria.

After the question round is resolved, create `plan.md` before editing files or running a long tool chain. The plan should include:

- intent and success criteria
- defaults selected for the user, assumptions, and user-confirmed choices
- scope and non-goals
- implementation steps
- risks and rollback/safety notes
- verification steps

Then execute from the plan. Skip the file only for tiny one-step work, read-only answers, or explicit no-file instructions.

## Technical Conflict Handling

When requirements conflict, do not silently choose one side. Resolve with a default, mode, phase, switch, or limitation.

```text
潜在冲突：[A 要求] 与 [B 要求] 存在冲突，因为 [原因]。
推荐默认：采用 [策略]，因为 [理由]。
写入 prompt：
- 默认行为：[...]
- 可选模式/阶段：[...]
- 例外条件：[...]
- 不能承诺：[...]
```

Common conflicts:

- `single AI call` vs `tool/MCP use`
- `show full thinking` vs `do not expose hidden chain-of-thought`
- `high-permission local automation` vs `user safety`
- `copy a known product` vs `avoid infringement`
- `quick MVP` vs `production-ready system`

## Hidden Thinking Requests

Rewrite requests for "complete thinking process", "thinking chain", or "chain of thought" into visible evidence:

- decision notes
- assumptions
- audit trail
- tool timeline
- input/output summaries
- test or verification results

Do not ask for or promise hidden chain-of-thought.

## Candidate Selection

When comparing prompt candidates:

1. Prefer the candidate with clearer scope, constraints, safety boundaries, deliverables, and acceptance criteria.
2. Merge only details that improve executability without weakening safety or focus.
3. Produce one final adopted prompt when the user wants a usable result.
