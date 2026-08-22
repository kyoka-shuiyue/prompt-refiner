# prompt-refiner

[简体中文](README.md) | [English](README.en.md)

`prompt-refiner` is a model-agnostic AI skill: it turns rough, scattered, or self-contradictory requests into a clear task contract, so the model knows where the destination is and where the guardrails are — while keeping the freedom to choose its own route.

It works in any AI coding assistant or agent that supports the `SKILL.md` skill format (Codex, Claude Code, Cursor, and others). It is not tied to any specific model or client.

## A Quick Example

You tell the AI:

> Build me a tool that searches my local files. Make it fast.

What can the AI do with that? Which stack? Is "fast" about startup or search? Does it need a UI? The model has to guess — and if it guesses wrong, you pay for the rework.

After `prompt-refiner`, the model receives:

```text
Goal: a CLI knowledge-base tool that indexes local file contents and returns
matching files and snippets for a keyword query.
Success criteria: searches a directory of 10k files return within 3 seconds
for common keywords; supports txt / md / pdf text.
Boundaries: read-only access to the specified directory; no network; never
modifies source files.
Stop rules: ship a working CLI version first; a GUI is a possible follow-up,
out of scope for now.
```

Notice the difference: barely more words, but the **goal, the bar, the boundaries, and the scope** are all clear. No guessing on either side.

## The Problem It Solves

Requests to AI usually fail in one of two opposite ways:

1. **Too little information** — "optimize the performance a bit." The model does not know the baseline, the target, or whether it may change interfaces, so it guesses.
2. **Too much prescription** — to prevent the model from wandering, you dictate every step. The model could have found a better path, but your process ties its hands.

`prompt-refiner` answers with a **smallest sufficient task contract** that avoids both traps: it fills in only the information that materially changes the result and leaves the rest to the model's judgment.

The smarter the model, the less your prompt needs to dictate each step — and the more it needs to state four things clearly:

- **What** you want (the goal)
- **What counts as done** (success criteria)
- **What it must not touch** (important boundaries)
- **When to stop** (stop rules)

These four, plus "which open choices are worth asking about," are the five fields of the task contract. Tools, modules, workflows, and implementation steps are supporting details — include them only when they change the contract itself or when explicitly requested.

## Why Short, Clear Prompts Work Better

Today's models are not short on context window — they are short on attention. That claim has real evidence behind it:

- The Stanford/UNC paper [Lost in the Middle](https://arxiv.org/abs/2307.03172) (TACL 2024) measured that model performance drops noticeably when relevant information sits in the middle of a long context, and degrades further as context grows.
- [Anthropic's engineering blog](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) recommends striving for "the minimal set of information that fully outlines your expected behavior" instead of stuffing prompts with rule lists and edge cases.

In other words: the smarter the model, the less it needs you to dictate every step — what it needs is short, explicit, verifiable instructions.

The same request, two outcomes.

Thrown at the model as-is:

> Optimize my website's loading speed.

The model has to guess: which page? How fast is fast? May it touch code? Dependencies? A wrong guess means a wasted round.

After `prompt-refiner`:

```text
Goal: cut homepage first-load time from 4.2s to under 2s.
Success criteria: Lighthouse performance ≥ 85; LCP < 2.5s; all regression tests pass.
Boundaries: front-end assets and build only; no backend API changes; no framework swap.
Stop rules: produce a baseline report first; stop once image compression plus code splitting hit the target — no over-optimization.
```

Barely more words, but nothing left to guess — the model's attention lands on verifiable goals instead of hunting for the point inside paragraphs of process description.

## Installation

### Option 1: One-Command Global Install for Codex

```bash
npx -y skills add kyoka-shuiyue/prompt-refiner --skill prompt-refiner --global --agent codex --yes
```

Requires Node.js `22.20.0` or newer. Restart Codex after installation.

### Option 2: Manual Install (Any SKILL.md-Compatible Host)

```bash
git clone https://github.com/kyoka-shuiyue/prompt-refiner.git
cd prompt-refiner
```

Copy the `prompt-refiner/` directory into your agent's skills directory:

```bash
# macOS / Linux
cp -R prompt-refiner /path/to/your-agent/skills/
```

```powershell
# Windows PowerShell
$skillsRoot = "C:\path\to\your-agent\skills"
Copy-Item -Recurse -Force .\prompt-refiner (Join-Path $skillsRoot "prompt-refiner")
```

Reload or restart the host if it does not auto-detect skills. The exact skills directory and invocation syntax follow your host's documentation.

To update, run `git pull --ff-only` in this repository and copy the skill directory again.

## Usage

### Automatic Triggering

Hosts with implicit skill routing load it automatically based on the frontmatter description — just talk normally.

### Explicit Invocation

Refine a rough request:

```text
Use $prompt-refiner to refine this request:
Build a knowledge-base tool that can search local files.
```

### Quick Commands

`prompt-refiner` recognizes a few commands that control output style:

| Command | Effect |
| --- | --- |
| `纯prompt` / `只要prompt` | Return exactly one copy-ready prompt block, no commentary |
| `不要问` / `直接继续` | Do not ask; resolve ambiguity with safe defaults and proceed |

For example:

```text
$prompt-refiner 纯prompt：完善这个本地 AI 桌面工具的开发需求。
```

```text
$prompt-refiner 不要问，使用安全默认值，在当前仓库完成这个功能并验证结果。
```

### How It Handles Four Kinds of Requests

| You want | It does |
| --- | --- |
| To refine, rewrite, or compare a prompt | Returns one copy-ready refined prompt; does not execute it |
| The task done | Clarifies key decisions, then executes within the authorized boundary |
| A read-only question answered | Answers directly; no prompt scaffolding |
| Something ambiguous | Inspects safely, infers low-risk defaults, and asks at most one bundled round only if needed |

## How It Works

1. **Classify the deliverable**: prompt, direct execution, read-only answer, or clarification.
2. **Pressure-test internally**: self-grill around the five contract fields, with no fixed question quota, until answers stop changing the contract.
3. **Default when safe**: low-impact ambiguity is resolved with sensible defaults instead of interrupting you.
4. **Ask only what matters**: when an answer would change scope, permissions, safety, cost, or acceptance criteria, ask at most one round of 1-3 questions, each with a recommended default.
5. **Produce the smallest sufficient result**: a copy-ready prompt, a direct answer, or an execution contract.
6. **Stress-test before delivery**: is the completion bar observable, are permissions clear, do any rules contradict, is there unnecessary prescription?

Only genuinely complex, multi-phase execution that benefits from recovery state gets a persistent plan file. Clear localized changes, read-only answers, and prompt-only work stay lightweight.

## FAQ

**Q: Will it make my prompt longer?**

No. Its core principle is "smallest sufficient" — keep only what affects the goal, the bar, the boundaries, and the stop rules; drop filler process and template boilerplate. Most refined prompts are shorter and denser than the original.

**Q: Will it interrogate me with a barrage of questions?**

No. It asks you at most one round of 1-3 questions, and only questions whose answers materially change the task. Everything else is resolved internally with safe defaults.

**Q: Which models or tools does it support?**

Any host that supports the `SKILL.md` skill format, including Codex, Claude Code, and Cursor. The skill content is model-agnostic: it depends on no vendor-specific API, tool combination, or workflow, and it does not maintain separate versions per model.

**Q: I use voice input and my messages come out garbled. Can it handle that?**

Yes. Transcription errors, homophones, scrambled word order, and swallowed verbs are handled by rebuilding the intended meaning first, instead of executing the literal ambiguity. Claims like "I changed X to Y" are verified before being treated as fact.

**Q: How is this different from "ask the AI more questions"?**

The difference is **filtered questions**. An unrestricted model will ask about trivial details; this skill resolves everything it safely can with defaults first, and leaves for you only the decisions where a wrong choice would change the outcome.

## Repository Structure

```text
prompt-refiner/
  SKILL.md                  The single canonical skill (model-agnostic)
  agents/
    openai.yaml             Optional interface metadata for compatible hosts
  references/
    patterns.md             Special-case handling patterns (loaded on demand)
evals/
  prompt-refiner-cases.jsonl          Test cases
  rubric.md / rubric.zh-CN.md         Evaluation rubric
  forward-test-report.md / .zh-CN.md  Forward test report
README.md / README.en.md
LICENSE
```

This repository maintains a single canonical version. The file under `agents/` is host interface metadata for the same skill, not a separate version per model or client.

## Evaluation

The skill content at commit `ead59ad` was compared against the earlier `11010eb` using the repository's 14-point rubric on six representative cases:

| Evaluation | Old | Then-current | Result |
| --- | ---: | ---: | --- |
| Independent testing | `71/84` | `78/84` | `+7` |
| Anonymous comparison | `60/84` | `84/84` | Reviewers chose the new version |

These scores refer to those historical commits and are directional evidence, not a universal benchmark. See the [full forward test report](evals/forward-test-report.md) for per-case scores, methods, and limitations. The rubric and cases for reproducing the evaluation live in `evals/`.

## Design Principles

- Task contracts over exhaustive specifications.
- Observable success criteria over vague quality demands.
- Safe defaults over unnecessary questions.
- Important boundaries over prescribed implementation paths.
- Fresh evidence over stale assumptions.
- Visible decisions and verification over hidden chain-of-thought.
- Stop expanding the moment the contract is sufficient.

## References

- [Lost in the Middle: How Language Models Use Long Contexts](https://arxiv.org/abs/2307.03172): Stanford/UNC paper (TACL 2024) measuring how performance drops when key information sits mid-context.
- [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents): Anthropic engineering post recommending the minimal sufficient set of information in prompts.
- [OpenAI GPT-5.6 prompt guidance](https://developers.openai.com/api/docs/guides/prompt-guidance-gpt-5p6): recommends leaner prompts that define autonomy and approval boundaries instead of dictating every step.
- [OpenAI wrote a dedicated prompting guide for GPT-5.6](https://www.bilibili.com/video/BV13F3g6VExA/) (Chinese): public walkthrough published `2026-07-25` by 清华姜学长.

The five-field task contract is this repository's own synthesis, not an official OpenAI template, and does not imply endorsement by OpenAI or the video author.

## License

MIT, see [LICENSE](LICENSE).
