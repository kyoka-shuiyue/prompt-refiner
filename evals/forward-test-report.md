# prompt-refiner Forward-Test Report

[English](forward-test-report.md) | [简体中文](forward-test-report.zh-CN.md)

## Summary

Date: `2026-07-29`

The model-agnostic task-contract candidate outperformed the previous repository version on six representative cases.

| Evaluation | Previous version | Candidate | Result |
| --- | ---: | ---: | --- |
| Independent runs with the 14-point rubric | `71/84` | `78/84` | Candidate `+7` |
| Anonymized comparison reviewer | `60/84` | `84/84` | Candidate preferred |

The independent runs recorded no critical failure for either version. The anonymized reviewer recorded one critical failure for the previous version because a clear implementation request received process scaffolding and a plan instead of a completed execution result. The candidate received no critical failure.

## Versions

- Previous version: repository commit `11010eb55038bdd6f6ff33d7281843347e0f0c8a`.
- Candidate: the working-tree revision represented by the commit containing this report.
- Rubric: [rubric.md](rubric.md), seven dimensions scored `0-2`, maximum `14` per case.

## Cases

The test used these six entries from [prompt-refiner-cases.jsonl](prompt-refiner-cases.jsonl):

1. `clear-prompt-rewrite`
2. `vague-product-request`
3. `current-information-research`
4. `ambiguous-code-change`
5. `scoped-local-implementation`
6. `destructive-cleanup`

They cover a clear prompt, safely defaultable product ambiguity, fresh-evidence requirements, inspect-before-ask behavior, authorized localized implementation, and destructive-operation safeguards.

## Method

Two isolated evaluators each received one skill version, the same six user requests, and the same rubric. The previous-version evaluator read only the committed baseline. The candidate evaluator read only the working-tree skill. Neither received the expected output fields from the JSONL cases.

A third evaluator then compared anonymized response sets as Candidate A and Candidate B without being told which version produced them. Candidate A was the new version; Candidate B was the previous version.

The underlying research, code edits, tests, and deletion requests were not executed. Execution cases were evaluated as pre-action contracts and decisions. Evaluators were instructed not to fabricate inspection evidence or verification results.

## Independent Results

| Case | Previous | Candidate | Difference |
| --- | ---: | ---: | ---: |
| Clear prompt rewrite | `14` | `14` | `0` |
| Vague product request | `12` | `14` | `+2` |
| Current-information research | `14` | `14` | `0` |
| Ambiguous code change | `9` | `12` | `+3` |
| Scoped local implementation | `12` | `12` | `0` |
| Destructive cleanup | `10` | `12` | `+2` |
| **Total** | **`71/84`** | **`78/84`** | **`+7`** |

Candidate dimension totals:

| Goal | Success | Boundaries | Stop | Clarification | Autonomy | Density |
| ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| `12/12` | `9/12` | `12/12` | `12/12` | `12/12` | `12/12` | `9/12` |

The candidate average was `13.0/14`, above the release threshold of `11`, with no critical failures.

## Anonymized Comparison

The comparison reviewer scored the candidate `84/84` and the previous version `60/84`. It preferred the candidate in every behavior that differed materially:

- The candidate inspected before asking on ambiguous code work.
- The candidate kept localized implementation free of fixed summaries and mandatory planning ceremony.
- The candidate required an evidence-backed dry-run proposal before deletion confirmation.
- The candidate avoided inventing product features or prescribing a platform choice.
- The candidate preserved completion, rollback, evidence, and uncertainty boundaries while leaving implementation choices open.

The reviewer classified the previous version's localized implementation response as a critical failure because it stopped at a visible summary and promised plan instead of completing the requested work. This strict judgment reinforces the execution-track rule: a plan is not the final deliverable when implementation was authorized.

## Qualitative Findings

### Improvements

- **Path autonomy**: methods, tools, modules, and architecture remain conditional rather than mandatory.
- **Information density**: visible text contains decisions that change execution, not a quota-driven summary.
- **Clarification discipline**: safe read-only evidence gathering precedes questions when possible.
- **Execution routing**: clear localized changes proceed without a planning artifact unless recovery complexity justifies one.
- **Destructive safety**: exact targets and evidence appear in the dry-run before approval to delete.
- **Portability**: the skill no longer assumes a specific model, client, planning path, or local installation layout.

### Preserved Strengths

- Fresh primary evidence for unstable facts.
- Fact/inference separation and missing-evidence handling.
- Explicit authorization and side-effect boundaries.
- Conservative deletion, rollback, and validation behavior.
- Replacement of hidden chain-of-thought with auditable evidence and decisions.

## Limitations

- Six of the 13 repository cases were run in this comparison.
- The tests evaluate generated contracts and action decisions, not downstream task quality.
- No production system, repository feature, web search, or deletion was executed.
- Model sampling can vary; the scores are directional evidence, not a deterministic benchmark.
- The anonymized reviewer scored concise behavior summaries rather than raw tool transcripts and explicitly noted that claimed execution evidence could not be independently verified.

## Decision

Adopt the candidate. It preserves the previous version's safety and evidence strengths while removing fixed-depth self-grilling, mandatory visible summaries, blanket planning, and platform-specific assumptions. The result is better aligned with increasingly capable models: define the goal, success criteria, important boundaries, and stop rules, then let the model choose an effective path.
