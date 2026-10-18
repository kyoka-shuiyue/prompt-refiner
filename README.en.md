# prompt-refiner

[简体中文](README.md) | [English](README.en.md)

`prompt-refiner` helps clarify your thinking: identify the intended outcome, resolve ambiguities and conflicts that change the result, and express the necessary goal, boundaries, and completion criteria.

**Current version: 1.3.0. The focus is clarifying existing intent while preserving scope and creative freedom.** Clear tasks proceed directly. Only gaps that materially affect the result warrant an addition or a question. The skill also supports creating, rewriting, critiquing, and comparing prompts.

The package contains `SKILL.md` and supporting files for compatible skill hosts. Discovery and invocation depend on the host; the core instructions do not bind to a particular model.

## What Changed

- **Narrower activation:** length, complexity, an open solution space, or missing acceptance criteria no longer trigger refinement by themselves. Clear creative requests proceed directly.
- **Less process:** removed the skill's mandatory plan-file gate, repeated command explanations, internal mode labels, and recurring phase checks. Planning, implementation, and validation belong to the underlying workflow.
- **Bounded goal completion:** derive observable completion criteria from the existing purpose without inventing features, modules, business commitments, fixed metrics, or methods.
- **Explicit exit:** finish refinement once the outcome and necessary boundaries are identifiable and material conflicts are resolved. Missing files, unknown environments, and technical failures alone do not restart intent clarification.
- **Corrected questions and commands:** offer 2–3 concrete interpretations for material ambiguity. Prefer one compact round, with minimal follow-up if a real conflict remains. “Continue directly” resumes the agreed task; “don't ask / you decide” delegates choices within existing authorization.

See [CHANGELOG.md](CHANGELOG.md) for release details in Chinese and English.

## When to Use It

| Situation | Behavior |
| --- | --- |
| Disordered language obscures meaning | Rebuild intent from context; correct obvious transcription errors directly |
| Intent is unclear or contradictory, or interpretations change the deliverable | Inspect available evidence, then offer concrete interpretations |
| Direction is clear but the actual deliverable cannot be identified | Derive the necessary goal and completion criteria from the existing purpose |
| Explicit request to create, rewrite, critique, or compare a prompt | Return the prompt or critique; execute its task only if asked |
| Clear task, simple request, or clear creative brief | Proceed through the underlying workflow |
| Only a source, file, dataset, or environment detail is missing | Locate or request the smallest necessary input without restarting an intent interview |

For example:

> Condense this document to one page, but preserve every paragraph word for word.

Clarify whether to retain the original and add a one-page summary, or allow edits to the original. Those produce materially different results.

By comparison, “Create an event poster with a seaside atmosphere; you decide the rest” provides direction and creative freedom. It does not require questions about colors, fonts, or layout, or authorize inventing event details.

## What It Retains

When refinement is needed, clarify only what affects the result:

1. The intended outcome.
2. What counts as complete.
3. Important scope, fact, and permission boundaries.
4. When to finish, ask, retry, or stop.
5. Which unresolved choices need the user's decision.

These are checks, not a form to fill on every request. Clear requirements need no rewrite. Leave ordinary implementation choices to the executor, and scale any additions to the task.

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

The six-case comparison in `evals/` covers historical commits `ead59ad` and `11010eb`; see the [historical report](evals/forward-test-report.md). **Those scores are not behavioral results for 1.3.0.** This release synchronizes the local skill files and checks structure and references. A new comparative behavior evaluation has not been run, and no model-specific efficiency gain is claimed.

## License

MIT; see [LICENSE](LICENSE).
