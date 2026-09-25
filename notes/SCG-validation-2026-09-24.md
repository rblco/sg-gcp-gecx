---
content-type:
  - Validation-Report
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-24
tags:
  - scg
  - gcp
  - validation
---
# SCG validation - Gemini Enterprise for CX - 2026-09-24

Validated file: `_src/geminiCustomerExperienceGuidance.csv` after the 2026-09-24 audit fixes recorded in `notes/SCG-remediation-2026-09-24.md`. All checks were run programmatically against the file on disk.

## Result

**Pass - 0 errors.** 141 requirements across 12 categories; IDs 10 to 1410, unique, all multiples of 10; 48 rows at Revision 2, 77 at Revision 1, 16 at Revision 0. Risk: 107 High, 32 Med, 2 Low. Directives: 97 MUST, 32 MUST NOT, 10 SHOULD, 2 SHOULD NOT.

| # | Check | Result |
| --- | --- | --- |
| 1 | Header matches `templates/securityConfigGuidance.csv` exactly (17 columns) | Pass |
| 2 | Every row has 17 fields; category headings are `## <Name>` followed by 16 empty fields | Pass |
| 3 | IDs unique, integer, multiples of 10; flat append-only numbering | Pass |
| 4 | Eight downstream columns empty on every data row | Pass |
| 5 | ID, Revision, Requirement, Rationale, Risk, Cost, References, and Mappings populated on every data row | Pass |
| 6 | Exactly one bolded directive per Requirement; no lowercase must, should, or may; every High row uses MUST or MUST NOT | Pass (one lowercase "must" in ID 370 was found and reworded before this run) |
| 7 | Risk and Cost limited to High, Med, Low | Pass |
| 8 | No non-ASCII bytes, no embedded line breaks | Pass |
| 9 | References are live URLs | Pass - 104 of 104 distinct URLs returned HTTP 200 |
| 10 | Mappings: NIST 800-53 on every row, every identifier and title matching the Rev. 5 OSCAL catalog, parents present for enhancements; segment order HIPAA, NIST 800-53, HITRUST CSF v11; HITRUST references in dotted notation; HIPAA citations in 164.xxx form | Pass - 74 distinct NIST controls; HITRUST CSF v11 on 114 of 141 rows (22 distinct references); HIPAA on 116 |
| 11 | At least one requirement restricting non-GA features | Pass - IDs 10, 20, 30, 40, 50, 740, 1270, 1320, 1350, 1390 |
| 12 | No companion markdown file hard-wraps a rendered paragraph (`check_md_wrapping.py`) | Pass for every Obsidian file; the rendered guidance document carries the known MkDocs admonition false positive |
| - | No cell references another requirement ID or another SCG | Pass |

## Requirements per category

| Category | Count |
| --- | --- |
| General | 9 |
| Identity & Access Management | 14 |
| Agents & Agent Management | 12 |
| AI Models & Model Management | 4 |
| Tools & External Integrations | 22 |
| Deployment & Channel Security | 13 |
| Agent Assist & Human Agent Support | 11 |
| Conversation Analytics & Insights | 14 |
| Data Protection & Privacy | 13 |
| Network & Perimeter Security | 10 |
| Monitoring & Analytics | 11 |
| Compliance & Certification | 8 |

## Warnings

- **HITRUST CSF v11 titles are unverified.** HITRUST is licensed content with no public catalog; confirm the 22 references against the organization's licensed copy before publication.
- **ID 950 is Low risk with a MUST directive.** This is deliberate: the transient stores are low exposure, but the complete data-flow record they belong in is mandatory, so documenting them cannot be optional.
- **eCFR references** (IDs 300, 1100) return HTTP 200 but present a bot check to scripts; the Subchapter G address for 29 CFR 2560.503-1 was confirmed as canonical in a browser on 2026-09-24.
- **Host migration redirects.** 19 references on `cloud.google.com` redirect to the same page on `docs.cloud.google.com`, one to a changed path; all resolve and were left as cited.
- **Launch-stage rows are volatile.** IDs 20, 40, 50, 320, 740, 800, 1270, 1320, 1350, 1360, and 1390 rest on a Preview, Pre-GA, or unreleased status that can change without a release note.
- **Residual documentation gaps.** The Python code tool sandbox identity (ID 450), BAA coverage of Google Search grounding (ID 470), the launch stage of `v1beta` and `v1alpha1` (ID 30), and where the OAuth tool option holds its client secret (ID 370) remain unconfirmed by Google.
