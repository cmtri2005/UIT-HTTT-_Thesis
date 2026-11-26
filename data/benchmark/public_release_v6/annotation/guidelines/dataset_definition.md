# ViLexTime dataset definition

This document defines what a ViLexTime item is, which items are admitted, and why items are excluded.
The annotation guideline (`annotation_guideline_v4.0.md`) implements it; where they differ, this document
prevails. Rules marked ⚖ were reviewed by a practising lawyer.

## 1. Task

- **Input:** a Vietnamese legal question `x` and a reference date `t` (`as_of`).
- **Output:** an answer `a` and the evidence (Article, Clause, Point) **in force at `t`**, or a statement
  that the provision is no longer in force.
- **Meaning of `t`** ⚖. The version legally in force at `t`, according to the validity intervals set by
  the documents themselves, **including retroactive effect** (Law 80/2015/QH13, Art. 152 and Art. 156(1);
  Law 64/2025/QH15, Art. 55 and Art. 58(2)). The question asks "what does the law provide on this matter at
  `t`"; the facts are taken to arise at `t`. It does not ask how a past act would be handled today (more
  lenient later law, transitional rules for ongoing matters).
- **Counting units:** document → question (one *lineage*: a provision and its chain of versions) →
  (question, date) pair → row.

## 2. Scope

| In scope | Out of scope |
|---|---|
| Central-level legal normative documents on the national legal database (vbpl.vn) | Local and individual documents; consolidated texts (used only for checking) |
| **Amendment**, including replacement at provision level ("Replace Article 48 as follows") | **Supplement** (new provisions such as 3a, 51a) |
| **Repeal** of a provision, including repeal of a parent provision | Territorial or personal scope of validity; conflicts between norms |
| Retroactive effect; documents signed but not yet in force | More lenient later law applied to past acts; questions that need several provisions |

**End of validity** ⚖. A document ceases to be in force when its stated term ends, when it is amended or
replaced by a new document of the same issuer, or when it is repealed (Law 80/2015/QH13, Art. 154; Law
64/2025/QH15, Art. 57(1)). An end date must therefore rest on a written basis (the document's own term, or
a replacing or repealing sentence). A later document that regulates differently without saying it replaces
the earlier one does not end it (see R3). End of validity is not inferred in a chain from a guided document
to its guiding document.

## 3. General rules

- **R1. Substantive change only** ⚖. A version transition counts only if it changes legal content. Not
  counted: (i) formatting (case, punctuation, spacing, numbering); (ii) renaming of an agency or
  administrative unit after restructuring with the same duties (Resolution 190/2025/QH15, Art. 3(1): the
  name in legal texts is converted to the successor's name, so the legal answer changes on the
  restructuring date); (iii) an update of a cross-reference that the document already applied automatically.
- **R2. Relocated content** ⚖. A content question is answered by the norm that currently governs that
  content. If a repealed provision's content was moved elsewhere (all seven criteria in the guideline,
  Step 7), the item is not used as "no answer" and is excluded (`relocated`); if unsure,
  `relocation_unclear`.
- **R3. Overriding norm** ⚖. A higher-ranked document, or a later document of the same issuer, regulates
  the same matter differently without amending the provision (Law 80/2015/QH13, Art. 156(2)–(3); Law
  64/2025/QH15, Art. 58(3)–(4)): excluded (`superseded_by_other_norm`).
- **R4. 2025 restructuring** ⚖. `t` after the restructuring (ministries merged from 1 March 2025; districts
  abolished and provinces merged from 1 July 2025) while the answer still names the old agency, level or
  place: excluded (`restructuring_2025`).
- **R5. Legal content.** A provision that only refers elsewhere ("as provided in Circular X") is not an
  answer (`reference_only`).
- **R6. Unapplied events.** An item touched by an amending or repealing event still under review is not
  admitted until the event is resolved (`pending_event`).
- **R7.** No date after the corpus snapshot (`after_snapshot`).

## 4. Groups

| Group | Capability measured | Condition (per question) | Correct answer |
|---|---|---|---|
| **control** (T1) | Adding time does not harm time-independent questions | One version only; document in force at `t`; parent not repealed | The single version |
| **two_versions** (T2) | Choosing the right version | Exactly two different answers in the history; the change is substantive (R1) and touches what is asked | One version per date |
| **multi_versions** (T3) | Choosing within a long chain | At least three different answers, each change substantive | One version per date |
| **repealed** (T6) | Knowing a provision is no longer in force | Clause or Point repealed by name (`repealed_by_name`) or through a repealed parent (`repealed_via_parent`); no relocation (R2) | No answer: the provision has been repealed |

two_versions items whose versions are ≥ 90% similar and differ in a decisive detail (amount, time limit, actor,
scope, condition) form the `near_identical` subgroup. The codes in parentheses are the internal codes used in the
annotation files; the codes T4 and T5 (signed-but-not-yet-effective and retroactive cases) were defined but
no such items were admitted to the release.

## 5. Date placement

| Mode | Role |
|---|---|
| `boundary` | Two consecutive days around a transition: the system must know the exact effective date |
| `mid_interval` | At least 30 days from both ends of a validity interval: the typical case; recommended for headline results |

The annotation files use the internal names `ranh_gioi` (= boundary) and `giua_khoang` (= mid_interval).

## 6. Admission conditions

An item is admitted only when all of the following hold, with evidence:

1. The answer is the version legally in force at `t`, checked against the original documents.
2. The question targets the change: different answers at the two dates (two_versions), or at least three (multi_versions).
3. The question matches one provision and has one answer.
4. The question does not reveal the date, document number, Article or Clause, and does not contain the answer.
5. The answer is legal content (R5).
6. No overriding norm, no restructuring issue (R3, R4).
7. (repealed) The seven relocation criteria were checked and the content was not relocated (R2).
8. The short answer is correct and complete.
Machine conditions R6 and R7 also hold.

In the released data, conditions 1–8 were checked by two independent AI agents (one Claude, one Codex)
with the source documents; disagreements were adjudicated; one author reviewed every lineage; hard cases
were re-checked by an AI adjudicator agent; a subset was read blind by humans (see `annotation/README.md`).

## 7. Exclusion codes (closed list)

| Code | When |
|---|---|
| `cosmetic_only` | Transition changes formatting only (R1-i) |
| `rename_only` | Transition only renames an agency or unit (R1-ii) |
| `auto_reference_update` | Transition only updates an automatic reference (R1-iii) |
| `single_change` | Chain qualifies for neither multi_versions nor two_versions |
| `relocated` | Repealed content was moved (R2) |
| `relocation_unclear` | Unsure whether content was moved (R2) |
| `superseded_by_other_norm` | Another norm overrides (R3) |
| `restructuring_2025` | Affected by the 2025 restructuring (R4) |
| `reference_only` | Answer is only a reference (R5) |
| `pending_event` | Unapplied event (R6) |
| `after_snapshot` | Date after the snapshot (R7) |
| `group_mismatch` | Question does not do its group's job (does not touch the change; a repealed-group question asks only yes/no) |
| `question_ambiguous` | Question matches several provisions or answers |
| `question_leaks` | Question reveals the answer, number, Article, Clause or version |
| `unanswerable_as_asked` | The answer does not answer what is asked |
| `gold_wrong` | Answer is not the version in force at `t` |
| `transitional_rule` | Answer depends on a transitional provision that applies to the asked situation |
| `other` | Other error, described in the note |
