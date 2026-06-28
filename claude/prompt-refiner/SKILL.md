---
name: prompt-refiner
description: Self-grilling prompt refiner — the prompt-focused form of grill-me. Turns a vague, scattered, or half-formed prompt, requirement, product idea, or coding ask into one strong, copy-pasteable prompt another AI can execute without guessing. Runs an internal grill-me style self-interrogation first — probing hidden assumptions, surfacing conflicts, picking smart defaults — then returns the single strongest prompt, asking only about the few high-impact controversies. Use when the user wants to refine, rewrite, optimize, polish, sharpen, clean up, restructure, compare, or stress-test a prompt, requirement, spec, or rough idea; Chinese triggers 完善/优化/整理/改写/润色/打磨/重写/精炼/重组/压测/对比/自问自答/追问/盘问/深挖/帮我把需求写成prompt/grill-me. Do NOT trigger for a plain build, fix, or answer task whose wording needs no sharpening (e.g. 做个贪吃蛇游戏 → just build it; 优化这段循环的性能 → just optimize the code); trigger only when the prompt, requirement, or spec is itself the thing to improve.
---

> Portability: this skill needs no special tools. If your harness exposes a real `grill-me` capability, the internal pass maps to it; otherwise just interrogate the request yourself.

# Prompt Refiner

Self-grill first, then hand back the strongest prompt. This skill already contains the effect of `grill-me` — never make the user invoke `grill-me` separately.

## Core decision: refine the prompt, or do the task?

- **Refinement IS the task** (user says refine / rewrite / optimize / polish / sharpen a prompt, or hands you scattered requirements to clean up) → run the internal pass, then output a reusable prompt.
- **User wants the underlying task done** (build X, fix Y, answer Z) → run the same internal grill *silently*, then execute the task directly. Do **not** emit a refined-prompt artifact, and do **not** end with a "want me to build it?" confirmation — just do it.
- **Already operationally clear** → light rewrite or just proceed; skip the full pass.

## 1. Internal grill-me pass (silent)

Interrogate the request the way `grill-me` interrogates a plan — but to yourself. For each open point: ask it, propose the best answer from the user's wording, rate your confidence, decide keep-internal vs expose.

Cover: the **real goal behind the stated request** (and the premise worth challenging) · success outcome · audience · output format · platform/stack/env · scope boundaries · hard constraints vs soft preferences · conflicts · unrealistic assumptions · missing prerequisites · decision-changing tradeoffs · second-order effects the user may not have considered.

Don't take the request at face value. The strongest refinement often comes from questioning *whether the user is even aiming at the right thing* — surface that, don't just tidy their wording.

Pass count: ~5–8 simple · ~12 normal · ~16–20 moderate · ~24–30 high-ambiguity · hard cap 50. Stop early once more questions no longer change the output.

## 2. Classify each open item

- **safe assumption** → keep internal, state it explicitly in the prompt
- **recoverable** → keep internal, pick a sensible default
- **high-impact controversy** → expose to the user

High-impact = prototype vs production · plan vs implementation · mandatory vs preferred stack · internal vs end-customer audience · speed vs completeness · hard vs negotiable constraints.

## 3. Ask only the decisive questions

Only if a high-impact controversy survives: **one** round, **1–3** questions, each with a recommended default. Never dump the grill tree on the user. Low-risk → assume and proceed.

**Question quality bar — a question earns a slot only if it:**
- **challenges a premise or forces a real tradeoff** the user hasn't faced — take a position, don't fence-sit;
- **goes after the real goal or a second-order effect** (what does success actually buy them, what would kill this) before any surface parameter;
- **is personalized** — it pivots on *this* user's situation, stakes, or constraints. First mine context you already have (the conversation, open files, prior turns, known preferences) and personalize from it — never ask what you can infer. Only when the decisive context is genuinely absent, ask for the single fact that most changes the answer, and say *why* it changes the answer — don't invent a default in its place;
- **would materially change the output** if answered differently.

Reject form-filling (platform, format, length, tech preference, vague "what's your audience") — default those silently and state the assumption in the prompt. A good grill makes the user think; a bad one makes them fill in a form.

## 4. Second pass — stress-test the draft

Before returning anything, re-check the drafted prompt against the original request:

- **Dropped constraint?** Every hard constraint the user gave must survive.
- **Invented scope?** Remove any feature, background, or requirement the user didn't imply.
- **Silent conflict?** Surface it and resolve via a mode / switch / stage (see patterns.md).
- **Missing current-info requirement?** If the answer depends on facts that change (news, prices, schedules, laws, APIs), make search + source-freshness mandatory.
- **High-risk use?** Investing, medical, legal, safety, betting → add a concise disclaimer.
- **Hidden chain-of-thought requested?** Convert to visible execution evidence; never expose or promise hidden CoT.

If any check fails, **fix the prompt** — don't just flag it.

## 5. Output

Default: just the refined prompt — copy-pasteable, in the user's language. Pick the lightest shape that fits:

- **one-liner** for simple asks
- **labelled block** (任务 / 目标 / 要求 / 约束 / 输出格式) as the normal default
- **fuller block** (背景 / 必做范围 / 约束与偏好 / 输出要求 / 质量标准) only for multi-module or multi-constraint work

Never inflate it into a PRD. Generate any illustrative examples on the fly from the current request — this skill ships no static example file.

In refine mode, close with **exactly one** compact revision entry, e.g. "要加 / 删 / 改什么,告诉我下一版。" In execute mode, no revision entry.

**Modes:** `prompt-only` (default) · `prompt + brief notes` (when the user asks what changed) · `candidate-evaluation` (user gave several versions → pick the strongest, say why, merge only safety-preserving improvements) · `question-first` (a high-impact controversy would materially change the result).

**Large engineering / product prompt** (app, agent, MCP, plugin, SaaS, data platform, desktop tool, or any multi-module build) → output three labelled parts: `自问自答压测摘要` (key questions + chosen defaults + prompt impact), `完善后的 Prompt` (the full copy-ready prompt — normally covering role, goal, users, workflows, modules, stack, data/permission boundaries, safety, delivery scope, acceptance criteria, verification), and `争议选择` (high-impact choices with recommended defaults). Confirm direction before the user acts on it.

## Deeper patterns

Read [patterns.md](references/patterns.md) **only** when the request involves: current-information search / prediction, high-stakes disclaimers, complex product/app/agent/MCP/skill/dev-tool specs, audit/logging requirements, hidden-thinking wording, multiple candidate prompts, or technical conflicts.

## Rules

1. Preserve the user's intent; never invent major features, background, or constraints.
2. Structural clarity beats smooth-but-empty wording.
3. Match the user's language.
4. Internal grilling is the default; visible grilling is selective — expose only the most controversial assumptions.
5. Output a refined prompt only when refinement is the task; otherwise self-grill and execute.
