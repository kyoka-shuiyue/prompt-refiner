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

## Intent-To-Task Conversion

When the user's intent is vague or fragmented, turn it into a task brief by identifying:

- `Outcome`: the concrete thing to produce or decide.
- `Operator`: who or what should execute it, such as Codex, another agent, a human team, or a specific model.
- `Inputs`: known materials, links, files, constraints, examples, and preferences.
- `Unknowns`: missing information separated into safe assumptions and blockers.
- `Boundaries`: what is in scope, out of scope, forbidden, or deferred.
- `Verification`: how the user or next agent can tell the work is done.

Prefer action verbs over abstract intentions. For example, rewrite "make this better" into "revise the prompt so another agent can implement X while preserving Y and verifying Z".

## Direct Execution Vs Prompt Output

Use this distinction aggressively:

- If the user asks "help me build/fix/write/analyze", silently refine and perform the task.
- If the user asks "optimize this prompt/rewrite requirements/self-grill/deepen", output the refined prompt or task brief.
- If the user provides a prompt and asks "can this be better?", output a refined prompt plus brief notes.
- If the user asks for both, provide the refined prompt first, then any requested execution plan or next step.

Failure mode to avoid: answering a direct work request with only a meta-prompt.

## Detail Expansion

Add detail only when it improves execution. Useful detail includes:

- specific deliverable shape
- relevant context the next agent otherwise lacks
- hard constraints and non-goals
- acceptance criteria
- validation steps
- source requirements
- safety and permission boundaries
- tradeoffs and recommended defaults
- placeholders for unknown inputs

Avoid detail that merely restates the same idea, adds invented features, or forces the user to decide low-impact preferences.

## Missing Information

Classify missing information before asking:

```text
Safe assumption:
- Missing item:
- Assumption:
- Why safe:
- Where to label it:

Blocker:
- Missing item:
- Why it blocks:
- Recommended question:
- Recommended default if the user asks you to proceed:
```

Common safe assumptions:

- preserve the user's original language
- keep output copy-ready
- avoid hidden chain-of-thought
- ask only blockers
- use placeholders for unknown file paths, product names, dates, or audiences

Common blockers:

- no source material is provided for a rewrite
- destructive local action is requested without target confirmation
- high-stakes recommendation lacks the domain facts needed for a responsible answer
- multiple mutually exclusive deliverables are requested and cannot be phased

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
????:[A ??] ? [B ??] ????,?? [??]?
????:?? [??],?? [??]?
?? prompt:
- ????:[...]
- ????/??:[...]
- ????:[...]
- ????:[...]
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
