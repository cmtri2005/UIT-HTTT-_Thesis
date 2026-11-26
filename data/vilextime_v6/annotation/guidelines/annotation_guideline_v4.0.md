# ViLexTime annotation guideline, version 4.0

How to decide whether a ViLexTime question is acceptable. It implements `dataset_definition.md`; where they
differ, the definition prevails. Rules marked ⚖ were reviewed by a practising lawyer. This is the English
edition of the guideline given to the labelling agents (Vietnamese original, verbatim:
`annotation/prompts/context/annotation_guideline_v4.0_vi.txt`); Section 0 is added for readers without legal
training and introduces no new rule.

**Group codes.** The guideline uses the internal codes found in the worksheets and labels: T1 = `control`,
T2 = `two_versions` (T4 = its `near_identical` slice), T3 = `multi_versions`, T6 = `repealed`. T5 (retroactive
or signed-but-not-yet-effective) rules are kept for completeness; no T5 item is in the release.

## 0. Before you start: how to look up a provision (for non-lawyers)

You need only a web browser. Every row links to the official text on the national legal database
(vbpl.vn, field `source_url`).

1. **Open the document** from `source_url`. Find the Article/Clause/Point named in `citation`.
2. **Check the document's status and dates.** The properties tab shows the issue date, the effective date
   and whether the document is still in force. Note any partial effect ("this Decree takes effect from …
   except Clause 2 …").
3. **Find every document that acts on it.** The relationship/diagram tab lists documents that amend,
   supplement, replace or repeal this one. Open each one dated before the `as_of` you are checking.
4. **In each acting document, find two sentences:** the command that touches your provision ("Amend
   Clause 2 Article 28 as follows …", "Repeal Point c …", "Repeal Clause 1 …" for a parent provision) and
   that document's own effective-date article. The change applies from that effective date, which may be
   earlier than the signing date (retroactive effect) or later (signed but not yet in force).
5. **Write down the wording in force on `as_of`.** Read list items together with their lead-in sentence
   (fines in sanction decrees are usually in the lead-in of the clause).
6. **For a repealed provision (T6)**, search for where the content went (Step 7 below): the same repealing
   document, sibling clauses, documents effective on the same day, higher-level documents.
7. **Paste one verbatim sentence** from the acting document you read. A label without evidence is not saved.

If after these steps you are still unsure, choose an answer anyway and write `[unsure] …` in the note.

## 1. What you label

**One question**, not one row. The screen shows: the question; each reference date and the answer at that
date (differences between versions highlighted, the short answer in bold); excerpts of the acting
documents; links to the originals.

There are three groups:

| Group | A good question is one that… |
|---|---|
| **T2** | gives **two different answers** at the two dates. "Near-identical" tag: the two versions are ≥ 90% similar and differ in one number or phrase |
| **T3** | gives **at least three different answers** across the dates |
| **T6** | has an answer at the earlier date; at the later date the provision **has been repealed** (by name or through its parent), so the correct answer is "no longer regulated" |

Each date has an `as_of` value and a reference answer: the content in force on that date.
The machine builds the answers; a language model drafts the question.

> A lawyer reads the question, knows each `as_of`, and looks up the right document. Would that person
> answer **exactly the answer at each date**, and in **only one way**?

Three ground rules:

1. **`as_of` is the date of legal effect** ⚖. The answer is the version in force on `as_of` according to
   the validity intervals set by the documents themselves, including retroactive effect. The facts are
   taken to arise on `as_of`. A transitional provision that keeps the old version only for ongoing matters
   does not change the answer.
2. **The question asks about content, not location.** The answer is the norm that governs the asked
   content on `as_of`. Changing only the fine amount is still the same norm.
3. **The lead-in is part of the answer.** A list item must be read with the lead-in of its parent on
   `as_of` (column `lead_in`).

## 2. Filling in the form

Answer all four questions. The label `dat` (accept) / `khong_dat` (reject) is derived automatically:
`dat` when Q1 = Correct, Q2 = Yes, Q3 = Yes, Q4 = No. When you choose a "bad" answer, also choose **one
reason** from the list under that question.

**Q1. Is the answer at each date correct according to the documents?**
Open the acting document; read the amending or repealing sentence and its effective-date article. Check
both the full passage and the bold short answer.
- Correct
- Wrong → `gold_wrong` (wrong version, wrong date, corrupted characters) · `reference_only` (the answer
  only says "as provided in …") · `other` (the short answer misses part of what is asked; write what)

**Q2. Does the question target the highlighted change?**
Read the question, hide the answer, ask: at the two dates, would the person have to say two different
things? T6: the question must ask about content, not only "is it still in force".
- Yes
- No → `group_mismatch` · `cosmetic_only` (only formatting changed) · `rename_only` (only an agency's
  name changed due to restructuring, same duties) · `auto_reference_update`

**Q3. Is the question clear?** Clear means: only one provision can answer it, with one answer; it does not
reveal the date, document number, Article, Clause or item letter, and does not contain the answer.
- Yes
- No → `question_ambiguous` · `unanswerable_as_asked` · `question_leaks`

**Q4. Does some other rule currently replace this answer?**
T2, T3: a higher-level or later document regulates the matter differently without amending this
provision; a date after the 2025 restructuring while the answer still names the old agency or level; a
transitional rule. T6: was the repealed content moved elsewhere (seven criteria, Step 7)?
- No
- Yes → `superseded_by_other_norm` · `restructuring_2025` · `transitional_rule` · `relocated` (give the new location)
- Unsure → `relocation_unclear` (give the suspected location)

**Evidence (required).** Paste **one verbatim sentence** from the acting document you read. For T6 with
Q4 = No, write where you searched ("same repealing document; sibling clauses; …").

## 3. The eight checks behind Q1–Q4

**T1 control questions** (added 11 November 2026). Q1 and Q3 as for T2. Q2 means "is the question really
time-independent": the provision has one version from its effective date to `as_of`; nothing amended,
replaced or repealed it or its parent before `as_of`; the document is in force on `as_of`. Q4 as for T2.

### Q1
**Step 1. The answer is the version in force on `as_of`.**
- Wrong version, or wording missing/extra compared with the original → `gold_wrong`.
- The portal's "original" already shows amended wording, so the earlier answer is wrong → `gold_wrong`.
- The acting document has split effect ("except Clauses 2 and 3", "provisions on … take effect from the
  signing date"): read its effective-date article. If the change at this date falls under the exception
  → `gold_wrong`, give the correct date.
- Characters corrupted by extraction → `gold_wrong`; do not fix the wording by hand.
- Retroactive amendment (date between the effective date and the signing date): the answer is the
  **new** version. Signed but not yet effective: the answer is the **old** version.
- T6: the repealing document must be in force. Repeal by name (T6a) or of the parent (T6b); if the parent's
  repeal keeps this provision ("except point a") → `gold_wrong`.
- A transitional provision keeps the old version for the very relationship the question states →
  `transitional_rule` (recorded under Q4).

**Step 5. The answer is legal content.** An answer that only refers elsewhere ("as provided in Chapter IV")
→ `reference_only`, even if it matches the wording.

**Step 8. The short answer is correct and complete.** Wrong or missing part of what is asked → `other`;
quote the correct span **from the answer**, do not write new text. If the fault is really the question's,
use the code of Step 3 or Step 6.

### Q2
**Step 2. The question touches the change.**
- T2: one question, two dates; same answer at both → `group_mismatch`. Near-identical pairs usually differ
  in one number or phrase; the question must ask exactly that.
- T3: at least three different answers; only two → `group_mismatch`. Name-only or formatting-only changes
  do not count as a new state.
- T6: a question that **only** asks yes/no → `group_mismatch`.
- **Not** a change ⚖: formatting only → `cosmetic_only`; renaming of an agency or unit after
  restructuring with the same duties (Resolution 190/2025/QH15, Article 3(1)) → `rename_only`; a reference
  update the document already applied automatically → `auto_reference_update`.
- **Is** a change: transfer to a functionally different agency; replacing a list of administrative units
  that is not one-to-one (e.g. "commune, ward, township" → "commune, ward, special zone").

### Q3
**Step 3. One provision, one answer.** A general question that several clauses answer →
`question_ambiguous`. Asking "when" with no time limit in the answer, or "who" with no actor →
`unanswerable_as_asked`.

**Step 4. No leaks.** No date, document number, Article, Clause or item letter, no answer text. A question
that already states the old version's phrase that the new version changed → Q3 = No at both dates.

### Q4
**Step 6. No overriding norm, no restructuring problem.**
- ⚖ A higher-level document, or a later document of the same issuer, regulates the same matter differently
  without amending this provision → `superseded_by_other_norm`.
- ⚖ `as_of` after the restructuring date (ministries 1 March 2025; districts and provinces 1 July 2025)
  while the answer still names the old agency, level or place → `restructuring_2025`.

**Step 7 (T6). Where did the repealed content go?** ⚖ Always do this step. Search in order: (a) the same
repealing document ("Supplement …", "Amend Article … as follows"); (b) parent and sibling clauses;
(c) other documents effective on the same day or referred to; (d) higher-level documents (then use
`superseded_by_other_norm`). Compare old and new on seven criteria, only for the part the question asks:

| # | Criterion | Met when |
|---|---|---|
| 1 | Conduct | essentially the same |
| 2 | Subject | the same (place names changed only by merger still count) |
| 3 | Conditions | the same |
| 4 | Structure | only position or time limit changed |
| 5 | Sanction / legal consequence | same type of sanction; the amount may change |
| 6 | Authority and rank | same issuer (or successor), legal rank not lower |
| 7 | Transition trace | a trace links the two: same "Repeal … Supplement …" batch, a "replace" sentence, a transitional clause |

All seven met → `relocated` (give the new location). Unsure on any, or no trace for 7 →
`relocation_unclear`. No successor, or criteria 1–6 clearly not met → the step passes. A gap (old clause
ends on date A, new one starts on a later date B) means an empty answer is correct between A and B, after
checking transitional rules, other documents, retroactivity, and that A and B really differ.

**Three common mistakes.** Accepting at first glance (in the calibration batch, all 12 such labels were
overturned by the lawyer); skipping Step 7 for T6; comparing the question with the whole provision instead
of the change.

## 4. Examples (checked against the original documents on 20 October 2026)

| Group | Row | Label | Why |
|---|---|---|---|
| T2 | Circular 12/2017/TT-BGTVT, Art. 28(2)(b), "which test papers and records must be archived", 30/11/2019 vs 01/12/2019 | `dat` | Circular 38/2019 added "simulated traffic situation test on software": the question targets the change |
| T2 | Circular 52/2022/TT-BGTVT, Art. 4(2), "issued under regulations of which ministry", 30/06/2025 vs 01/07/2025 | `khong_dat`: `rename_only` | Only "Ministry of Transport" → "Ministry of Construction"; by Resolution 190/2025/QH15 the name changed in law from 01/03/2025, so both dates have the same legal answer |
| T3 | Circular 12/2017/TT-BGTVT, Art. 33(13), "when is a driving licence valid" | `dat` | Three answers: before 01/12/2019; to 31/05/2024; from 01/06/2024 (adds VNeID) |
| T6a | Decree 67/2013/NĐ-CP, Art. 26(3)(c), minimum 3 m² for tobacco retail, 10/08/2021 | `dat` | Repealed by Decree 106/2017, Art. 2(4); no successor rule |
| T6b | Decree 23/2016/NĐ-CP, Art. 23(1)(a), qualifications at crematoria, 09/06/2026 | `dat` | Decree 14/2026, Art. 138(2) repealed the parent clause; no successor |
| T6 | Decree 37/2017/NĐ-CP, Art. 8(2), fire safety at seaports, after 24/10/2018 | `khong_dat`: `relocated` | Decree 147/2018 rewrote Article 7 with the same requirement |
| T6 | Circular 41/2011/TT-BCA, Art. 4(1), Laos travel pass, 14/11/2021 | `khong_dat`: `superseded_by_other_norm` | From 01/07/2020 Decree 76/2020 (higher rank) prescribes form M01 |
| — | "…main sanction of type c) is regulated how?" | `khong_dat`: `question_leaks` | The item letter is a location |
| — | "Which acts are fined 20,000,000–40,000,000 VND?" (answer: one item) | `khong_dat`: `question_ambiguous` | The correct answer is the whole list |

## 5. Procedure used for the released data

From 6 November 2026 the released set was labelled by two AI agents (AI-A, AI-B) working independently
with this guideline and the source documents; disagreements were adjudicated; one author reviewed every
lineage after seeing the AI labels; cases marked `[unsure]` went to the lawyer. Blind human readings
(annotators who did not see the store or the AI labels) were collected separately; see the README.

For human double annotation the guideline prescribes: independent work without AI help; a calibration
batch reviewed with the lawyer before batch 1; agreement measured per question Q1–Q4 and on the derived
label (Gwet's AC1 ≥ 0.80 and negative agreement ≥ 0.60, κ reported alongside; AC1 was chosen after a pilot
on 7 October 2026 because of κ's prevalence paradox, not pre-registered); disagreements decided by the
lawyer; raw labels kept and released.

## 6. Decision log (lawyer rulings)

- **24/10/2026, calibration batch.** For T6, judge the seven criteria within the question's scope;
  reference-only answers are `reference_only`; "township" → "special zone" is a real change; incomplete
  short answers are `other`; split effective dates can make a date wrong (`gold_wrong`); extraction
  corruption is `gold_wrong`.
- **26/10/2026, batches 1–18** (the lawyer read an AI-drafted adjudication sheet and agreed, so this round
  was AI-assisted). A T6 row dated before the repeal is `dat` even if the content later moved; when a Law
  and a lower document both regulate the matter, the Law prevails (`superseded_by_other_norm`); "applies
  from budget year X" is retroactive to 1 January of year X.
- **10/11/2026, five open cases.** Near-synonyms are not a new answer unless the scope of conduct differs;
  repeated words caused by a correct replacement are still `q1 = correct`; context that reveals part of the
  answer is `question_leaks`; missing decisive facts with several valid situations is
  `question_ambiguous`; `single_change` when a chain qualifies for neither T3 nor T2.
