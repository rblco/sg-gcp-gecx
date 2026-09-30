---
content-type:
  - Remediation-Record
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-30
tags:
  - scg
  - gcp
  - remediation
---
# SCG remediation - Gemini Enterprise for CX - 2026-09-30

The findings of the standard review in `notes/SCG-review-2026-09-30.md` were applied to `_src/geminiCustomerExperienceGuidance.csv` at the owner's direction ("Go ahead and resolve and edit"). **13 rows were revised; no ID was added, renumbered, merged, or retired, and no Revision was incremented.** The guide holds 141 requirements across 12 categories. The pre-fix state is archived as `_src/archive/geminiCustomerExperienceGuidance.2026-09-30.pre-review-fixes.csv`.

## Method

Edits were applied by a script that replaces whole cells by row ID and column, or asserts that a replaced passage occurs exactly once, so no edit could land on stale text; the CSV writer was first confirmed to reproduce the unmodified file byte for byte. The guidance markdown was re-rendered with the organization's `svcGuidanceTemplate.jinja` through the same preprocessing `processDocs.py` applies, after confirming that a render of the pre-fix CSV reproduced the existing `_src/geminiCustomerExperienceGuidance.md` except for one table that an editor had re-padded and one stale ID 10 Rationale cell, both of which the re-render corrects. The validation suite was then re-run on the file on disk.

## Decisions recorded so they are not re-litigated

**Revision reset (finding 1).** The owner reset every row to Revision 0 on 2026-09-30, before the review ran. The guide has never been published (`DocGen.json` records `publish: false`), so the increments accumulated on 2026-09-23 and 2026-09-24 were drafting-phase increments, and the first published state carries Revision 0 on every row. This supersedes the "one increment per recertification cycle" rule recorded on 2026-09-23 for this unpublished draft; that rule applies from the first publication onward. The README, `DocGen.json` (`lastUpdated`), and the 2026-09-24 remediation and validation notes were reconciled with a dated addendum, and the four wording changes made the same morning ("the vendor's" to "Google's" in IDs 10, 230, 450, and 980) are carried in this record. The fixes below therefore leave Revision at 0.

**Tool registration alerting (finding 2).** ID 1140 raised from SHOULD at Med to MUST at High. The guide identifies tool registration as the path by which an agent's reach widens without a deployment (IDs 360, 490, 1310 all assume the alert exists), so its detective control sits at the same level as the settings and deletion alerts (IDs 1120, 1130). Cost stays Low because ID 1070 already mandates the DATA_WRITE logging the alert reads.

**Transient caches (finding 3).** ID 950 raised from Risk Low to Med, matching the data-flow record requirement (ID 1220, High) whose completeness it serves. Directive unchanged at MUST.

**Notes length (finding 4).** Eight Notes cells over 1,000 characters were trimmed to at most 999 with every substantive point retained: the six residency exceptions on ID 680, the fail-open and per-profile behaviour on ID 620, the forced-upgrade and composite-v1 guidance on ID 1260, and the three-mode authentication table on ID 530. The Google documentation inconsistencies formerly narrated on IDs 390 and 450 are kept as one sentence each in the Notes and recorded in full in `notes/external-source-log.md`. The 2026-09-24 decision to keep ID 450's long Notes is superseded by the owner's direction to resolve the finding.

**Pre-GA mapping family (findings 5 and 8).** IDs 1320 and 1350 gained `HIPAA: 164.308(a)(8) (Evaluation)` alongside their business associate citations, so every Pre-GA prohibition maps to Evaluation; ID 1320 also gained `HITRUST CSF v11: 05.k`, matching the third-party basis of its HIPAA mapping. The business associate citations were kept because Google's BAA excludes Pre-GA offerings.

**Citation depth and consistency (findings 6 and 7).** ID 1100 now cites `164.316(b)(2)(i) (Time Limit)`; ID 680 gained `HIPAA: 164.308(a)(1)(ii)(B) (Risk Management)`, matching the other residency rows.

**Info findings.** Finding 9 (the phrase "this guide" in sixteen cells): no change; no row cites another by ID. Finding 10 (eCFR bot check): no change; confirm in a browser before publication. Finding 11 (host-migration redirects): left as cited for this cycle. Finding 12 (OWASP LLM 2026 edition): remains an open workstream item. Finding 13 (category names): no change. Finding 14 (HITRUST titles): still to be confirmed against the licensed catalog. Finding 15 (launch-stage rows): every stage was re-read on 2026-09-30 and found unchanged; see `notes/external-source-log.md`.

## Rows changed

| ID | Change |
| --- | --- |
| 10 | Notes trimmed (1,125 to 993 characters). |
| 60 | Notes trimmed (1,002 to 843). |
| 390 | Notes trimmed (1,099 to 977). |
| 450 | Notes trimmed (1,249 to 987). |
| 530 | Notes trimmed (1,031 to 999). |
| 620 | Notes trimmed (1,224 to 994). |
| 680 | Notes trimmed (1,433 to 996); HIPAA 164.308(a)(1)(ii)(B) added. |
| 950 | Risk Low to Med. |
| 1100 | HIPAA 164.316(b)(2) deepened to 164.316(b)(2)(i). |
| 1140 | Directive SHOULD to MUST; Risk Med to High. |
| 1260 | Notes trimmed (1,026 to 962). |
| 1320 | HIPAA 164.308(a)(8) added; HITRUST CSF v11 05.k added. |
| 1350 | HIPAA 164.308(a)(8) added. |

## Open items

- Confirm the 22 HITRUST CSF v11 references against the organization's licensed catalog before publication.
- Confirm the two eCFR references in a browser before publication.
- Decide when to migrate OWASP LLM mappings from the 2025 to the 2026 edition.
- Ask Google for the launch stage of `v1beta` and `v1alpha1` (ID 30), where the OpenAPI and MCP OAuth option holds its client secret (ID 370), the Python sandbox identity (ID 450), and BAA coverage of Google Search grounding (ID 470).
- Commit the working tree; the review, remediation, and validation records for 2026-09-30 and the Revision reset are all uncommitted until then.

## Verification

See `notes/SCG-validation-2026-09-30.md`.
