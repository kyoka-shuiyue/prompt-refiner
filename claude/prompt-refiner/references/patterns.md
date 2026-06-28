# Refinement Patterns

Load this file when the user asks to deeply improve a prompt, provides multiple candidate prompts, or asks to distill examples into reusable behavior.

## General Patterns

- `parameterize missing target`: If the prompt is reusable, replace missing concrete targets with placeholders such as “用户指定的比赛/主题/产品”, instead of asking unnecessarily.
- `current information`: When the prompt asks for latest news, search, odds, market data, schedules, laws, or other changing facts, make search/verification mandatory. Include freshness windows, source-time requirements, and instructions to state missing or uncertain information.
- `prediction or judgment`: Require facts first, then judgment. Separate data-supported conclusions from intuition-based inference. Include confidence level and factors that could overturn the conclusion.
- `high uncertainty or risky use`: Add a concise risk note or disclaimer when outputs could affect betting, investing, medical, legal, safety, or other high-stakes decisions.
- `role expansion`: Upgrade vague roles into domain-specific analyst roles, but do not exaggerate credentials or invent authority.
- `output discipline`: Add sections that force the downstream AI to return facts, reasoning, final answer, confidence, and uncertainty in a predictable structure.

## Complex Product Or App

For product, app, desktop, SaaS, agent, plugin, or developer-tool requests, expand vague ideas into:

- role
- project goal
- product positioning
- target users
- core workflows
- feature modules
- configuration surfaces
- data model
- security boundaries
- architecture
- delivery scope
- MVP vs later phases
- acceptance criteria
- known risks and limitations

Example pattern:

A vague request like “做一个类似 Cherry Studio 的 AI 聊天桌面 EXE，支持 Skill、MCP、沙箱、参数微调和搜索” should become a detailed product-and-architecture prompt with desktop app positioning, strict single-call vs tool-enhanced modes, provider/model configuration, parameter controls, Skill/MCP management, sandbox verification, anonymous search, audit logs, local encrypted storage, security rules, deliverables, MVP phases, acceptance criteria, and a requirement to verify current third-party specs from official sources.

## Search And Prediction

For prompts that combine search with prediction, analysis, or judgment, include:

- target placeholder
- mandatory latest-information checklist
- source freshness requirement
- instruction to cite missing or uncertain information
- fact-vs-inference separation
- final prediction or judgment
- confidence level
- risk factors that could overturn the conclusion
- disclaimer for betting, investing, medical, legal, or safety-sensitive contexts

Example pattern:

A vague request like “你是足球预测专家，搜索最新消息，凭直觉给比分” should become a structured reusable prompt with analyst role, target match placeholder, latest-information checklist, fact-vs-intuition separation, predicted result, score, confidence, risk factors, and a non-betting disclaimer.

## Technical Conflict Handling

When requirements conflict, do not silently choose one side. Preserve intent by making the tension explicit and resolving it with a mode, switch, stage, or limitation.

Template:

```text
潜在冲突：
[A 要求] 与 [B 要求] 存在冲突，因为 [原因]。

推荐处理：
默认采用 [推荐默认策略]。

写入 prompt 的约束：
- 提供 [模式/开关/阶段] 来分别满足两侧需求。
- 明确默认行为。
- 明确例外条件。
- 明确不能承诺的能力。
```

Common conflict examples:

- `single AI call` vs `MCP/tool use`: split into strict single-call mode and tool-enhanced mode.
- `show full thinking` vs `do not expose hidden chain-of-thought`: show execution evidence, not hidden reasoning.
- `high-permission local tools` vs `user safety`: add permission gates, audit logs, and safe defaults.
- `copy a known product` vs `avoid infringement`: use the product only as a style/function reference, not as copied assets or exact UI.

## Hidden Thinking Requests

When the user asks to show “完整思考过程”, “思维链”, or “chain of thought”, rewrite it as visible execution evidence:

- tool timeline
- inputs and outputs
- summaries
- logs
- decision notes
- audit records
- user-readable process explanation

Do not ask for or promise hidden chain-of-thought disclosure.

## Official Source Requirement

When the prompt depends on third-party projects, APIs, protocols, model capabilities, MCP behavior, search providers, laws, prices, or version-sensitive facts, require checking latest official documentation or primary sources.

## Candidate Selection

When multiple refined prompt candidates are provided:

1. Choose the candidate with the clearest scope, safety boundaries, conflict handling, deliverables, and acceptance criteria.
2. Explain the choice briefly if the user asks which one to adopt.
3. Merge useful details from other candidates only when they improve executability without weakening safety.
4. Produce one final adopted prompt if the user wants a usable result.
