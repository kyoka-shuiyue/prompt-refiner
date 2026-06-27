# Refinement Patterns

Load this file only for prompt refinements that need extra handling beyond the core workflow.

## Self-Grill Decision Record

For complex prompts, internally reduce ambiguity into this record:

```text
Question: [what is uncertain?]
Recommended default: [best answer]
Evidence: [what in the user request supports it]
Risk if wrong: [what changes if false]
Prompt impact: [what to write into the prompt]
```

Expose only the decisions that materially change scope, cost, safety, architecture, or output quality.

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
