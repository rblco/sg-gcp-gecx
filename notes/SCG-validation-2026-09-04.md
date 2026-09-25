---
content-type:
  - Validation-Report
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-04
tags:
  - scg
  - gcp
---

# SCG validation report - Gemini Enterprise for CX

- File validated: `_src/geminiCustomerExperienceGuidance.csv`
- Validated: 2026-09-04
- Result: **22 of 22 structural and content checks pass. Five verification items were carried forward as warnings; two were closed during remediation on 2026-09-04 and three remain open.**
- Superseded in part: this report describes the file as first authored (125 requirements, 11 categories). The guide was reviewed and amended the same day - see [[30-ISA.WORK/20-CapCert-SecurityGuidance/10-GCP/Gemini-Ent-CX/notes/SCG-review-2026-09-04]] and [[SCG-remediation-2026-09-04]]. It now holds 132 requirements in 12 categories, and the post-remediation check results are recorded in the remediation log.

## Structural checks

| # | Check | Result |
| --- | --- | --- |
| 1 | First line matches the canonical 17-column template header exactly, with `Data_Levels` and `Audit Procedures` at positions 7 and 8 | PASS |
| 2 | Every row parses to exactly 17 fields; every category heading row is `## Name` followed by 16 empty fields | PASS - 11 heading rows, 125 data rows at authoring; 12 and 132 after remediation |
| 3 | IDs are unique and whole multiples of ten | PASS - 10 through 1250 at authoring, extended to 1320 after remediation, no duplicates. The ascending-in-document-order form of this check was retired during remediation: it encodes the initial-draft state rather than the numbering rule, because a requirement added to an early category correctly takes the highest ID plus ten. |
| 4 | The eight downstream columns are empty on every data row | PASS |
| 5 | ID, Revision, Requirement, Rationale, Risk, Cost, References, and Mappings are populated on every data row | PASS |
| 6 | Every Requirement contains exactly one bolded directive | PASS |
| 7 | Risk and Cost use only `High`, `Med`, `Low`, case-sensitive | PASS |
| 8 | File contains no non-ASCII characters | PASS - byte-level scan found no smart quotes, en or em dashes, non-breaking spaces, or ellipses |
| 9 | Every References cell holds at least one well-formed https URL | PASS |
| 10 | NIST 800-53 segment present on every data row | PASS - 125 of 125 |
| 11 | At least one requirement restricts non-GA features | PASS - IDs 10, 50, and 1250 |
| 12 | Markdown line discipline across all companion `.md` files | PASS - `check_md_wrapping.py` reports clean for every file |

## Content and citation checks

| # | Check | Result |
| --- | --- | --- |
| 13 | Every NIST SP 800-53 control identifier and title verified against the Rev. 5 OSCAL catalog | PASS - 71 distinct controls checked by identifier and exact title; no fabricated identifiers, no paraphrased titles |
| 14 | No withdrawn NIST controls cited | PASS - `SA-12 (Supply Chain Protection)` confirmed `status: withdrawn` in the catalog and excluded; `SR-3`, `SR-5`, and `SR-11` used for supply chain instead |
| 15 | Rev. 4 titles not carried forward | PASS - `AU-2` cited as Event Logging, `SA-9` as External System Services |
| 16 | Mapping segment order is HIPAA, then NIST, then OWASP, then other frameworks | PASS |
| 17 | Every cited URL resolves | PASS - all 34 distinct URLs returned HTTP 200 on 2026-09-04 |
| 18 | References cite only vendor primary documentation, vendor compliance and terms pages, and standards bodies | PASS - no third-party tutorials, forum posts, or news sources cited |
| 19 | Directive and Risk pairing internally consistent | PASS - no MUST paired with Risk `Low`, no MAY paired with Risk `High` |
| 20 | No cross-references between requirements, as required for a standalone guide | PASS - no requirement, rationale, or note refers to another requirement by ID |
| 21 | No tracked changes, review comments, or TODO markers in the CSV | PASS |
| 22 | Exactly one CSV deliverable produced | PASS |

## Counts

| Category | Requirements |
| --- | --- |
| General | 9 |
| Identity & Access Management | 13 |
| Agents & Agent Management | 12 |
| Tools & External Integrations | 15 |
| Deployment & Channel Security | 11 |
| Agent Assist & Human Agent Support | 9 |
| Conversation Analytics & Insights | 14 |
| Data Protection & Privacy | 13 |
| Network & Perimeter Security | 10 |
| Monitoring & Analytics | 11 |
| Compliance & Certification | 8 |
| **Total** | **125** |

- Directive distribution at authoring: MUST 87, MUST NOT 26, SHOULD 11, SHOULD NOT 1.
- Risk distribution at authoring: High 87, Med 36, Low 2. After remediation: High 91, Med 39, Low 2.
- Cost distribution at authoring: Low 62, Med 62, High 1. After remediation: Low 67, Med 64, High 1.

Framework coverage: NIST 800-53 on 125 rows (100 percent), HIPAA on 95 rows (76 percent), OWASP on 28 rows, MITRE ATLAS on 8 rows.

The cost distribution is worth a comment for a reviewer: a single High-cost requirement (ID 350, project separation by trust domain) is not an error. Almost every other control in this suite is a configuration decision that costs nothing to make correctly at the right moment and a great deal to correct afterwards, which is why so many rows pair High risk with Low cost. That pairing is the central economic fact about this product and the reason the certification gate matters more than the remediation budget.

## Warnings - verification items carried into the deliverable

These are documentation gaps, not defects in the CSV. Each affected requirement states its own limitation in the Notes column rather than asserting a fact this session could not establish. All five should be closed before certification sign-off.

**1. CLOSED 2026-09-04. Four Agent Assist pages rendered as navigation only.** The CMEK, regionalization, data redaction and retention, and release notes pages returned HTTP 200 but no article body across repeated attempts. The consequence is that requirement ID 610 asserts that Agent Assist has its own CMEK initialization - which the existence of the page establishes - but does not assert its immutability semantics; ID 680 states the residency obligation and directs the reader to verify the region list rather than reproducing one; ID 630 rests on the retention values documented on the CX Agent Studio conversation history page rather than an Agent Assist-specific source; and no individual Agent Assist feature is prohibited by name, so the baseline Pre-GA requirement at ID 10 carries that weight alone. **Closed:** all four pages were retrieved successfully during the review and their contents applied to IDs 610, 620, 630 and 680. Two of them contradicted the guide - the Agent Assist retention default is 30 days rather than one year, and Smart Reply is excluded from CMEK coverage. See the remediation log.

**2. OPEN. Two CX Agent Studio tool pages returned HTTP 404.** The Python code tool and client function tool pages could not be retrieved at any URL tried, though both tool types are confirmed to exist from the tools overview page. ID 450 therefore asserts no sandboxing, isolation, or egress property and instructs the reader to assume none until Google documents otherwise; ID 460 states explicitly that client-side execution is inferred from the tool taxonomy rather than read from the page. **Verify the Python code tool execution model with Google before that tool type is used with member data** - it is the only tool type in the suite that runs arbitrary code, and the guide currently constrains it by review rather than by a documented technical boundary.

**3. OPEN. Perimeter sequencing is derived, not quoted.** Requirement ID 980 requires the service perimeter to be enforced before resources are created. Google's VPC Service Controls page for this product documents seven operations that fail inside a perimeter but states no sequencing requirement. The requirement is an inference from those seven breakages and is labelled as such in its own Notes. It is sound engineering and should survive review, but a reviewer should know it is not a vendor quotation.

**4. OPEN. Data store retrieval access-control behaviour rests on documented silence.** Requirement ID 390 prohibits grounding a data store tool on content whose audience is narrower than the agent's, on the basis that the data store tool page describes grounding but says nothing about ACL-aware retrieval, per-user result filtering, or retrieval identity. This is an absence of evidence rather than a documented absence of the capability. The conservative reading is the correct one for certification, but **confirming it with Google would either close a real exposure or relax a requirement**, and either outcome is worth the question.

**5. OPEN. Model training position is scoped narrowly in the source.** Google's statement that customer content is never used to train its production machine learning models appears on the CX Agent Studio conversation history page and is scoped to a single toggle on a single component. Requirement ID 910 converts this into an obligation to obtain the position contractually across all four components and every data path, including audio through Speech-to-Text and analytics in CX Insights. Until that is answered in writing, the guide should not be read as establishing that no component uses conversation content for model improvement.

## Notes on scope

This guide was authored standalone. The prompt named Gemini Enterprise Agent Platform as a dependency; Google's documentation asserts no hierarchical or runtime relationship, and the user confirmed standalone scoping on 2026-09-04. The reasoning is recorded in `prompt-log.md`.

An earlier and independent draft of a guide for this product exists at `_GCP/z-archive/Gemini-Ent-CX/`. By user decision it was left untouched and none of its content was read into this draft. Its IDs bear no relationship to the IDs in this file, and the two documents should not be merged.
