---
content-type:
  - Session-Output
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-23
tags:
  - scg
  - gcp
  - session
---
# Session output - Gemini Enterprise for CX - deep review, fixes, and guidance document - 2026-09-23

## Recap

This session followed the morning update (recorded in `session-output_2026-09-23.md`). It ran a deep scg-reviewer pass with a HIPAA and HITRUST CSF v11 overlay, applied the findings at the owner's direction, and rendered the guidance markdown. The review found 0 Blocker, 49 Major, 60 Minor, and 20 Info. The three most consequential problem groups:

- audit-log types that would have left the required alerts silent;
- IAM role inheritance that the rows did not account for;
- a perimeter missing the Agent Assist API.

All three are fixed. The guide holds 138 requirements across 12 categories, 121 of them revised in this pass, with no ID added, merged, or retired.

## Counts

| Metric | Value |
| --- | --- |
| Requirements / categories | 138 / 12 (unchanged) |
| Rows revised by the review fixes | 121 |
| Revisions (cycle) | 45 at 2, 79 at 1, 14 at 0 |
| Risk raised Med to High | 10 rows |
| HITRUST CSF v11 / HIPAA rows | 111 / 114 of 138 |
| Distinct NIST controls | 75, all verified against the Rev. 5 OSCAL catalog |
| Distinct references | 98, all HTTP 200 |
| Diff map (cycle vs 2026-09-04) | 90 corrected, 28 strengthened, 14 carried, 6 new, 0 weakened |

## Artifacts

- `_src/geminiCustomerExperienceGuidance.csv` - corrected guide
- `_src/geminiCustomerExperienceGuidance.md` and `.pdf` - rendered guidance document (organization template)
- `_src/geminiCustomerExperienceGuidance-diff-map-2026-09-23.csv` - regenerated for the whole cycle
- `_src/archive/geminiCustomerExperienceGuidance.2026-09-23.pre-review-fixes.csv` - pre-fix snapshot
- `notes/SCG-review-2026-09-23.md` - review report with Resolutions section
- `notes/SCG-remediation-2026-09-23.md` - what changed and what did not
- `notes/SCG-validation-2026-09-23.md` - validation of the final file (supersedes the post-update version)
- `README.md`, `_Gemini Enterprise for CX.md`, `prompt-log.md`, `_src/DocGen.json` (`internalReview: true`) - updated

## Recommendations

1. Decide whether ID 1380 should be MUST NOT: RAG Engine supports neither data residency nor Access Transparency.
2. Obtain Google's written confirmations required by IDs 910 (training restriction scope) and 1200 (covered status of every API in the data path, including the Dialogflow API used by Agent Assist).
3. Have counsel name the state recording-consent and bot-disclosure statutes for IDs 600 and 310.
4. Verify the HITRUST CSF v11 references against the licensed catalog.
5. Commission an independent human review. Fresh-context agents reduced but did not remove the same-model limitation.

## Errors in processing

- The lead's NIST title lookup converted the catalog's em dashes to a double-spaced hyphen, producing one malformed SC-7(5) title in a patch; caught and normalized at merge.
- One verification query used a search string with a leading space and briefly suggested a missing statute reference on ID 310; the text was present, and the note was correct.
- In the review stage, the lead's first executive summary misattributed ID 1310 to the morning update; corrected before the report was delivered.

## Skill defects

- The reviewer checklist (D5, I2) still expects category ID buckets, which contradicts the generator's flat append-only rule; its HIPAA shape pattern (M4) rejects valid citations such as 164.308(a)(1)(ii)(A).
- Neither skill specifies how to render the guidance markdown; the organization template and `processDocs.py` were used by precedent.
- The generator's PDF rule does not say whether the rendered MkDocs guidance counts; it was produced for consistency with the RAG Engine guide.

## Deviations from the request

None. "Apply the fixes" was taken to mean all Major and Minor findings plus actionable Info items; one Info item needs the owner.

## Processing time, tokens, and cost

- This session: review started about 21:10 MDT, completed 2026-09-23 22:20 MDT.
- Tokens: roughly 1.5 million across the lead and six reviewer and patch-agent turns (the agents reported between 178,000 and 275,000 tokens each). These figures are approximate, taken from the session's counters rather than billing.
- Cost: not measurable from inside the session.

## Completed

2026-09-23 22:20 MDT
