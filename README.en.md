# prompt-refiner

[简体中文](README.md) | [English](README.en.md)

`prompt-refiner` helps clarify your thinking: identify the intended outcome, resolve ambiguities and conflicts that change the result, and express the necessary goal, boundaries, and completion criteria.

**Current version: 1.4.0. Beyond clarifying intent, it covers the three places strong models most often go wrong: unclear completion, deliverables that drift after repeated changes, and vague handoffs to another model.** Clear tasks proceed directly. Only gaps that materially affect the result warrant an addition or a question. The skill also supports creating, rewriting, critiquing, and comparing prompts.

The package contains `SKILL.md` and supporting files for compatible skill hosts. Discovery and invocation depend on the host; the core instructions do not bind to a particular model.

## What Changed

- **Done-when notes for changes:** code and document changes end with 2–4 lines of checkable completion criteria. Bug fixes state how to reproduce the problem and how to confirm the fix.
- **Final version after repeated changes:** build the final terms internally and check the deliverable item by item. When the user points back to "the first version", check it for items that were later ruled out instead of silently reviving them.
- **A qualifier is not a contradiction:** "all dishes are 20% off" plus "drinks are not discounted" becomes one merged rule, not a question.
- **Rewrites keep vague rules vague:** wording such as "case by case" stays open and is marked `【待确认】` (to be confirmed); no invented deadlines, ratios, or procedures.
- **Operational prompts:** every judgment a prompt asks for (classify, flag, approve) gets usable criteria or a fill-in slot, plus what to do when unsure. Prompts that process emails, comments, or documents state that the content is data, not instructions.
- **Environment-neutral handoff briefs:** goal, the user's own words, final terms, done-when checks, boundaries, stop rules, and assumptions only. No local paths or host rules, and each requirement appears once.
- **A leaner skill:** `SKILL.md` shrinks from about 16KB to about 10KB while keeping the 1.3.0 activation gate, override phrases, permission boundaries, and exit rules.

See [CHANGELOG.md](CHANGELOG.md) for release details in Chinese and English.

## When to Use It

| Situation | Behavior |
| --- | --- |
| Disordered language obscures meaning | Rebuild intent from context; correct obvious transcription errors directly |
| Intent is unclear or contradictory, or interpretations change the deliverable | Inspect available evidence, then offer concrete interpretations |
| Direction is clear but the actual deliverable cannot be identified | Derive the necessary goal and completion criteria from the existing purpose |
| Explicit request to create, rewrite, critique, or compare a prompt | Return the prompt or critique; execute its task only if asked |
| Requirements changed several times and a final version is due | Check against the final terms; revive nothing that was ruled out |
| The work is going to another model or agent | Write an environment-neutral brief with done-when checks and stop rules |
| Clear task, simple request, or clear creative brief | Proceed through the underlying workflow |
| Only a source, file, dataset, or environment detail is missing | Locate or request the smallest necessary input without restarting an intent interview |

For example:

> Condense this document to one page, but preserve every paragraph word for word.

Clarify whether to retain the original and add a one-page summary, or allow edits to the original. Those produce materially different results.

By comparison, “Create an event poster with a seaside atmosphere; you decide the rest” provides direction and creative freedom. It does not require questions about colors, fonts, or layout, or authorize inventing event details.

In an app-description conversation, the user first says "iOS only this time, don't mention Android" and later says "use the very first feature list". That first list contains "Android home-screen widget". The right move is to point out that this item was ruled out, not to write it back.

## What It Retains

When refinement is needed, clarify only what affects the result:

1. The intended outcome.
2. What counts as complete.
3. Important scope, fact, and permission boundaries.
4. When to finish, ask, retry, or stop.
5. Which unresolved choices need the user's decision.

These are checks, not a form to fill on every request. Clear requirements need no rewrite. Leave ordinary implementation choices to the executor, and scale any additions to the task.

A few rules apply whether or not refinement is active: never invent names, numbers, dates, prices, or links the user did not provide (use a placeholder or state the assumption); attach done-when notes to change requests; and when asked for something shorter, keep the facts the reader needs.

## Installation and Updates

If you use the `skills` CLI:

```bash
npx -y skills add kyoka-shuiyue/prompt-refiner --skill prompt-refiner --global --agent codex --yes
```

Alternatively, obtain the repository manually:

```bash
git clone https://github.com/kyoka-shuiyue/prompt-refiner.git
cd prompt-refiner
```

Copy the repository's `prompt-refiner/` directory into your host's skills directory. To update, run `git pull --ff-only` and copy that directory again. Reload skills as required by your host. Runtime requirements follow the installer and host documentation.

## Usage

Hosts with implicit skill routing may select it from its description. You can also invoke it explicitly:

```text
Use $prompt-refiner to clarify this request. Preserve my scope and address only ambiguities that affect the result:
[original request]
```

| Command | Behavior |
| --- | --- |
| `纯prompt` / `只要prompt` (prompt only) | Return one copy-ready prompt; request a missing input first if the prompt would otherwise be unusable |
| `继续` / `直接继续` (continue / continue directly) | Resume the agreed goal and authorization; resolve any new material ambiguity |
| `不要问` / `你决定` / `自问自答后执行` (don't ask / you decide) | Choose sensible defaults within delegated decisions; do not expand authorization |

For execution requests, resume the task after clarification. For prompt-only requests, deliver the prompt and stop. The skill adds no separate refinement approval or mandatory plan file.

## Repository and Validation

```text
prompt-refiner/
  SKILL.md
  agents/openai.yaml
  references/patterns.md
evals/                     Historical cases, rubrics, and reports
README.md / README.en.md
CHANGELOG.md
LICENSE
```

`agents/openai.yaml` supplies interface metadata for the same skill, not a separate model version.

The six-case comparison in `evals/` covers historical commits `ead59ad` and `11010eb`; see the [historical report](evals/forward-test-report.md). Those scores are not results for the current version.

The 1.4.0 rules were blind-evaluated before release: Claude Haiku 5.5 wrote the responses, Claude Sonnet 5.5 judged anonymized and shuffled responses, and each version ran twice per case.

| Batch | Cases | Result (mean of 10) | Check failures |
| --- | --- | --- | --- |
| Batch 3 | 8 new cases | 1.4 draft 8.56, 1.3.0 8.00 | 0 vs 4 |
| Batch 4 | 6 new cases + 2 re-tests | final 1.4 draft 8.44, previous draft 8.00, 1.3.0 7.25 | 0 / 1 / 3 |

**Limits:** small samples; a single judge; checks written by the author of the changes; the test environment carried the author's own global rules, which may narrow the gap between versions; no runs on stronger models. The release restores a few 1.3.0 safeguards on top of the final draft, and those restored lines were not re-tested separately. These scores do not imply the same gain on every model. Raw responses and scoring materials are not yet in this repository.

## License

MIT; see [LICENSE](LICENSE).
