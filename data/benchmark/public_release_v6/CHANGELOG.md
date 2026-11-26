# Changelog

Versions follow MAJOR.MINOR.PATCH. A change to any gold answer or to the schema is MAJOR.

## 6.0.0 (2026-11-22)

No question, date, answer or row id changes (`data/benchmark/earlier_test2.jsonl` is byte-identical to 5.1.1). MAJOR because
a metadata field is renamed.

- **Renamed field.** `review.high_risk_lawyer_check` → `review.high_risk_ai_author_check`. No lawyer took part
  in that step: two blind AI labels were reconciled and verified by one author who saw them. Now in `SCHEMA.md`.
- **Blind readings, round 3 (random).** 30 of the 94 lineages unread after round 2, one random row each (seed
  `20261121:random30`), read independently by two annotators; answers locked by sha256 before the store's
  answer was shown; agreement with the store computed by machine (`basis: machine_compare`), not by the
  readers. 0 of 30 store errors (Wilson 95%: 0–11.4%). Annotators: in-force status 30/30, wording 29/30; the
  one disagreement was a reader error (`adjudication: annotator_error`, checked against the issued text of
  Circular 03/2018/TT-BYT). Files: `annotation/blind_reading/round3_random_*.tsv`. 135 of 199 lineages now
  have a blind reading.
- **`blind_gold.as_of_read`.** Every blind-reading record names the date the reader was asked. Records are
  per lineage and copied to all its rows; only the row at `as_of_read` was read.
- **Selection funnel.** `annotation/excluded_lineages.jsonl`: the 175 lineages labelled but not released
  (374 → 199), with the step that removed each and the reason codes of its final label. By step:
  `label_review` 156, `label_unresolved` 10, `author_flagged_recheck` 5, `relocation_check` 3,
  `author_decision` 1. `check_dataset.py` checks 175 + 199 = 374 and that no excluded lineage is released.
- **Corpus interval corrections.** `corpus/interval_corrections.jsonl`: the six corpus versions whose interval
  is wrong (listed in `corpus/README.md` since 5.0.0), in machine-readable form. The corpus files are unchanged.
- **`check_dataset.py`** reports blind readings per round and reader, with the comparison basis.
- **Known source defects (not changed).** The store keeps the portal's wording verbatim. A machine scan for
  immediately repeated word sequences (6+ words) over all contexts and answers found two defects in the
  portal text: Circular 03/2018/TT-BYT, Art. 13(2)(c), repeats "và nắm vững các nguyên tắc, tiêu chuẩn GPP"
  (the issued text has it once); and one version of Art. 20 of the lineage `…138bc0b82b6b` lacks the line
  break between the article title and its body. The other hit of the scan is ordinary legal wording.

## 5.1.1 (2026-11-21)

- **Datasheet.** `docs/DATASHEET.md` (Datasheets for Datasets). Documentation only; no data change.

## 5.1.0 (2026-11-21)

- **Scorer.** `scripts/evaluate.py` (standard library only): accuracy, wrong_version, refusal, F1,
  pair_consistency, recall@k, tv_recall@k, MRR, per date mode and group, with lineage-resampled intervals.
  On three of the authors' saved system runs it matches their internal scorer row by row on accuracy,
  wrong_version, F1, refusal and tv_recall; recall@k and MRR differ on at most 3 of 361 rows (see the
  script header). No data change.

## 5.0.0 (2026-11-21)

- **Store corrections (missed amendments).** Every released provision was checked against the documents that
  vbpl.vn lists as amending, repealing or replacing its document; candidates were read in the official text.
  Five provisions kept a version beyond its real end. Seven rows are re-dated, so their ids change; answers
  and labels are unchanged because each moved row now sits in the middle of its version's true interval:
  - `…7ba9e014…` (Decree 20/2008/NĐ-CP, Art. 20(2)(b)): amended by Decree 92/2017/NĐ-CP from 2017-09-25;
    row 2020-04-04 → 2015-08-13 (found by a blind reader).
  - `…a16089ba…` (Circular 23/2014/TT-NHNN, Art. 4(1)): the change took effect on 2017-03-01 (Circular
    32/2016/TT-NHNN), not 2019-03-01 (Circular 02/2019/TT-NHNN re-enacted the same wording); boundary rows
    2019-02-28/2019-03-01 → 2017-02-28/2017-03-01, row 2017-04-29 → 2015-12-23 (found by a blind reader).
  - `…e9f1fcba…` (Decree 33/2010/NĐ-CP, Art. 7(1)(đ), control): Article 7 replaced by Decree 53/2012/NĐ-CP
    from 2012-08-10; row 2015-12-30 → 2011-07-13.
  - `…b69cb980…` (Decree 05/2015/NĐ-CP, Art. 14(3)(b)): the decree ceased to be in force on 2021-02-01
    (Decree 145/2020/NĐ-CP, Art. 114(2)); row 2022-05-12 → 2020-01-08.
  - `…e23c54c0…` (Decree 135/2004/NĐ-CP, Art. 61(3)): Chapter IV repealed by Decree 221/2013/NĐ-CP from
    2014-02-15; row 2017-10-20 → 2012-11-26.
  Affected rows carry `flags: store_corrected_missed_amendment` and `store_correction` (basis and old end).
  The corpus snapshot is unchanged; its intervals for these versions are listed in `corpus/README.md`.
- **Answers.** Eight answers cut with "…" in the middle of a sentence completed verbatim, per version.
- **High-risk augmentation.** 29 rows (23 repealed, 6 multi_versions) labelled again by two blind AI agents
  and verified by one author: `review.high_risk_lawyer_check`, `annotation/high_risk_augmentation.jsonl`.
- **Blind readings.** The lawyer sheet now fills `agrees_with_store` (in-force status only, as in `review`).
- **Validation.** `check_dataset.py` also checks that every answerable row's `as_of` lies inside its
  version's validity interval, and prints flag counts.

## 4.0.0 (2026-11-19)

- **Group names.** Descriptive names replace the internal codes: `T1` → `control`, `T2` → `two_versions`,
  `T3` → `multi_versions`, `T6` → `repealed`; subgroups `T4` → `near_identical`, `T6a` → `repealed_by_name`,
  `T6b` → `repealed_via_parent`. Row ids and `twin_of` change accordingly (`vilextime:repealed:…`).
- **Date modes.** `as_of_strategy` values `ranh_gioi` → `boundary`, `giua_khoang` → `mid_interval`.
- **Readable field values.** `qsource` → `source_batch` (`round_1/2/3`, `control`); `post_review_ai_checks`
  steps `H7` → `author_flagged_recheck`, `R2` → `relocation_check`; `repeal_mode` `ancestor` → `via_parent`;
  the redundant internal `review.batch` field was dropped.
- Annotation files keep the internal codes; join them by `lineage_id`.

## 3.0.0 (2026-11-19), first public release

Derived from the authors' internal test set 2.0.0. Changes relative to that version:

- **Answers.** T1 answers: leading enumeration marker removed from 82 of 86 answers. One T2 row corrected
  after the lawyer's blind reading: Circular 11/2021/TT-BXD was repealed by Circular 36/2026/TT-BXD from
  2026-07-01, so the row dated 2026-08-06 is now unanswerable (`flags: gold_corrected_by_blind_gold`).
- **Labelling.** Every lineage now has one Claude and one Codex label: 28 lineages first labelled by two
  agents of the same family were relabelled on the missing side (superseded labels kept in `annotation/`).
- **Provenance.** `review` rewritten per lineage from the labelling records: agents and models (Codex
  models from the CLI logs), AI resolution, author review, post-review AI checks, blind readings.
- **Schema.** `domain` and `domain_source` removed (69% of rows were "other"); `issuer_code` added.
- **New components.** `corpus/` (retrieval corpus) and `annotation/` (guidelines, worksheets, raw labels,
  adjudication, blind readings, prompts).
