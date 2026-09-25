---
content-type:
  - Validation-Report
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-23
tags:
  - scg
  - gcp
  - validation
---
# SCG validation - Gemini Enterprise for CX - 2026-09-23

Validated file: `_src/geminiCustomerExperienceGuidance.csv` in its final state for 2026-09-23, after the morning update and the review fixes. This report supersedes the post-update validation written earlier the same day. All checks were run programmatically against the file on disk.

## Result

**Pass - 0 errors.** 138 requirements across 12 categories; IDs 10 to 1380, unique, all multiples of 10; 45 rows at Revision 2, 79 at Revision 1, 14 at Revision 0. Risk: 105 High, 31 Med, 2 Low. Directives: 96 MUST, 30 MUST NOT, 10 SHOULD, 2 SHOULD NOT.

| # | Check | Result |
| --- | --- | --- |
| 1 | Header matches `_scg-scaffold/templates/securityConfigGuidance.csv` exactly (17 columns) | Pass |
| 2 | Every row has 17 fields; category headings are `## <Name>` followed by 16 empty fields | Pass |
| 3 | IDs unique, integer, multiples of 10; flat append-only numbering | Pass |
| 4 | Eight downstream columns empty on every data row | Pass |
| 5 | ID, Revision, Requirement, Rationale, Risk, Cost, References, and Mappings populated on every data row | Pass |
| 6 | Exactly one bolded directive per Requirement; no lowercase must, should, or may; every High row uses MUST or MUST NOT | Pass |
| 7 | Risk and Cost limited to High, Med, Low | Pass |
| 8 | No non-ASCII bytes, no embedded line breaks | Pass |
| 9 | References are live URLs | Pass - 98 of 98 distinct URLs returned HTTP 200 |
| 10 | Mappings: NIST 800-53 on every row, every identifier and title matching the Rev. 5 OSCAL catalog, parents present for enhancements; segment order HIPAA, NIST 800-53, HITRUST CSF v11, OWASP, others; HITRUST references in dotted notation from the approved list | Pass - 75 distinct NIST controls; HITRUST CSF v11 on 111 of 138 rows (22 distinct references); HIPAA on 114 |
| 11 | At least one requirement restricting non-GA features | Pass - IDs 10, 20, 30, 40, 50, 740, 1270, 1320, 1350 |
| 12 | No companion markdown file hard-wraps a rendered paragraph (`check_md_wrapping.py`) | Pass for every Obsidian file; see the warning on the rendered guidance document |
| - | No cell references another requirement ID or another SCG | Pass |

## Requirements per category

| Category | Count |
| --- | --- |
| General | 9 |
| Identity & Access Management | 13 |
| Agents & Agent Management | 12 |
| AI Models & Model Management | 4 |
| Tools & External Integrations | 21 |
| Deployment & Channel Security | 12 |
| Agent Assist & Human Agent Support | 11 |
| Conversation Analytics & Insights | 14 |
| Data Protection & Privacy | 13 |
| Network & Perimeter Security | 10 |
| Monitoring & Analytics | 11 |
| Compliance & Certification | 8 |

## Warnings

- **HITRUST CSF v11 titles are unverified.** HITRUST is licensed content with no public catalog; confirm the 22 references against the organization's licensed copy before publication.
- **The rendered guidance document is an MkDocs target.** `_src/geminiCustomerExperienceGuidance.md` uses the organization template's MkDocs admonitions (`!!! warning`), whose indented body the Obsidian wrapping checker reports as a wrapped paragraph. That is a false positive for this target, recorded in the Gemini RAG Engine session of the same day.
- **eCFR references** (IDs 300, 1100) return HTTP 200 but present a bot check to scripts; their text was confirmed through the eCFR renderer API.
- **Launch-stage rows are volatile.** IDs 20, 40, 50, 740, 800, 1270, 1320, 1350, and 1360 rest on a Preview, Pre-GA, or unreleased status that can change without a release note.
- **Residual documentation gaps.** The Python code tool sandbox identity (ID 450) and BAA coverage of Google Search grounding (ID 470) remain unconfirmed by Google.
