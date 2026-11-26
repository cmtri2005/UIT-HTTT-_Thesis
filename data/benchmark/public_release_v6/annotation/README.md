# Annotation materials

Everything needed to inspect or re-run the labelling of the 199 released lineages. All files are
restricted to the released lineages and joined to the dataset by `lineage_id`.

```
annotation/
├── guidelines/        what an item is and how to label it (read these first)
├── worksheets/        what each labeller was shown
├── labels/            what the AI labellers answered, and how disagreements were settled
├── blind_reading/     independent human answers, used to check the labels
└── prompts/           exact instructions and documents given to the AI agents (plain text, verbatim)
```

## How labelling worked, in order

| Step | Who | Input | Output |
|---|---|---|---|
| 1. Worksheet | script | dataset row + excerpts of the acting documents | `worksheets/worksheets.jsonl` |
| 2. Label | AI-A (Claude) and AI-B (Codex), independently | worksheet, guideline, source documents on the web | `labels/ai_labels_raw.jsonl` |
| 3. Settle disagreements | AI adjudicator agent, or the author | both labels | `labels/adjudication.jsonl` |
| 4. Review | one author, every lineage | labels + adjudication | accepted lineages (`review` in the dataset) |
| 5. Re-check | AI adjudicator agent | lineages marked unsure (H7); all repealed provisions (R2) | `labels/post_review_ai_checks.jsonl` |
| 6. Blind reading | human readers who saw neither the store nor the AI labels | question + date only | `blind_reading/*.tsv` |

## Files

| Path | Content |
|---|---|
| `guidelines/dataset_definition.md` | What an item is: task, scope, rules R1–R7, groups, admission conditions, exclusion codes |
| `guidelines/annotation_guideline_v4.0.md` | How to label: the four questions Q1–Q4, reason codes, worked examples, and a step-by-step lookup for readers without legal training |
| `worksheets/worksheets.jsonl` | One record per lineage: question, dates, answers, acting-document excerpts (`official`), links |
| `labels/ai_labels_raw.jsonl` | Raw labels before adjudication: 398 used (199 × 2) + 56 superseded. Fields: `run`, `agent`, `status`, `labeler` (`ai-a`/`ai-b`), Q1–Q4 with reason codes, `verdict`, verbatim `quote`, `search_log`, `note`, `evidence` |
| `labels/adjudication.jsonl` | One record per disagreement: disputed fields, reasoning, evidence, proposed label |
| `labels/post_review_ai_checks.jsonl` | H7 and R2 re-checks after the author's review |
| `blind_reading/round{1,2}_<reader>.tsv`, `round3_random_<reader>.tsv` | Reader's answer (`in_force`, `content`, `applicable_document`); `store_*` = the store's answer, revealed only after the reader's file was locked by sha256; `agrees_with_store` = rounds 1–2: the reader's own judgement (lawyer sheet: `in_force` only); round 3: computed by machine (same `in_force`, and `content` contained in `store_content` after normalisation). Round 3 is a random sample: 30 of the 94 lineages unread after round 2, one random row each (seed `20261121:random30`), read by both annotators |
| `excluded_lineages.jsonl` | The 175 lineages labelled for the final set but not released (374 labelled → 199 released): `group`, `source_batch`, `question`, `removed_at` (`label_review`, `label_unresolved`, `author_flagged_recheck`, `relocation_check`, `author_decision`), `decided_by`, `reason_codes` (closed list of `guidelines/dataset_definition.md` §7). Candidates removed before labelling (for example repealed provisions whose content may have moved) are not in this file |
| `prompts/labelling/<run>/*.txt` | Prompt per chunk and side (`*_ai-a.txt`, `*_ai-b.txt`); `codex_b_wrapper.txt` |
| `prompts/adjudication/<run>/*.txt` | Adjudication prompts |
| `prompts/agents/*.txt` | Agent definitions and skills loaded by the labelling and adjudication agents |
| `prompts/context/*.txt` | The Vietnamese definition and guideline exactly as the agents read them (working versions with internal notes; the English documents in `guidelines/` are the reference) |

## Agents per labelling run

Every released lineage is labelled by **one Claude agent (AI-A) and one Codex agent (AI-B)**.

| Run (`run` field) | Lineages | AI-A | AI-B |
|---|---:|---|---|
| `round3_mixed` | 47 | Claude Code, claude-opus-5-5 | Codex CLI, gpt-6.1-sol |
| `lo2` | 70 | Claude Code, claude-opus-5-5 | Codex, model not recorded (agent self-report: GPT-6 family) |
| `supp18` | 11 | Claude Code, claude-opus-5-5 | Codex CLI, gpt-6-luna |
| `t1` | 43 | Claude Code, claude-sonnet-5-5 | Codex CLI, gpt-6-luna |
| `uniform_20261119` | 19 | Claude Code, claude-opus-5-5 (new) | Codex native subagent, model not recorded (kept from `round2_codex`) |
| `uniform_20261119` | 9 | Claude Code, claude-opus-5-5 (kept from `silver01`) | Codex CLI, gpt-6-luna (new) |

Codex models are taken from the Codex CLI logs (`model:` line). `gpt-6.1-sol` was refused for the
ChatGPT account used from 8 November, so later runs used `gpt-6-luna`.

**Relabelling of 19 November 2026.** Runs `silver01` (both agents Claude) and `round2_codex` (both agents
Codex) gave same-family pairs for 28 released lineages. The missing side was relabelled blind with the
same worksheet records, guideline and prompt template (`prompts/labelling/uniform_20261119/`). The new
pairs agreed on 26 lineages; the 2 disagreements repeat the objection already settled in
`labels/adjudication.jsonl` for those lineages, and the new label agrees with that settlement.
`status = used` marks the labels of the current pair; `status = superseded …` the earlier same-family
labels, kept for audit.

## Codes

- Group codes in these files are the internal ones: `T1` control, `T2` two_versions (`T4` near_identical),
  `T3` multi_versions, `T6` repealed. Join to the dataset by `lineage_id`, not by row id.

- Q1 `dung`/`sai` (correct/wrong) · Q2, Q3 `co`/`khong` (yes/no) · Q4 `khong`/`co`/`khong_chac`
  (no/yes/unsure) · `verdict` `dat`/`khong_dat` (accept/reject; `dat` only when Q1–Q4 are all good).
- Reason codes: guideline Section 2.

## Notes

- Everything under `prompts/` is kept verbatim as evidence of what the agents received, including the
  initials `CMT` (the reviewing author) and file paths of the project repository.
- Free-text notes and timing columns of human readers are not released; reason codes are.
