---
content-type:
  - Remediation-Record
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-23
tags:
  - scg
  - gcp
  - remediation
---
# SCG remediation - Gemini Enterprise for CX - 2026-09-23

The findings in `notes/SCG-review-2026-09-23.md` (0 Blocker, 49 Major, 60 Minor, 20 Info) were applied to `_src/geminiCustomerExperienceGuidance.csv` at the owner's direction. **121 of 138 rows were revised; no ID was renumbered, merged, retired, or added.** The guide still holds 138 requirements across 12 categories. The pre-fix state is archived as `_src/archive/geminiCustomerExperienceGuidance.2026-09-23.pre-review-fixes.csv`, and the 2026-09-04 published state as `_src/archive/geminiCustomerExperienceGuidance.2026-09-23.pre-update.csv`.

## Method

The three reviewers who produced the findings drafted replacement cell values for their own rows as JSON patch files, reusing the vendor pages they had already read, under written authoring rules: ASCII only; one bolded directive; no cross-references; NIST titles taken exactly from the Rev. 5 OSCAL catalog, with parents present for every enhancement; HITRUST references only from the approved list; every new URL fetched and confirmed to support its claim. No agent edited the CSV. The lead merged the patches with a validator that enforces those rules. Before the file was written, the lead read every rewritten requirement and the full text of the highest-impact rows (630, 730, 1310). One defect was caught at merge: a double-spaced SC-7(5) title, caused by the lead's own title lookup, which was normalized.

## Decisions recorded so they are not re-litigated

**Revision numbering.** One increment per recertification cycle, following the Gemini RAG Engine precedent of the same day: every row whose substance changed at any point on 2026-09-23 carries its 2026-09-04 Revision plus one, whether the change came from the morning update, the review fixes, or both. Neither intermediate state was published. HITRUST-only additions do not count as substance, per the owner's decision earlier in the cycle. Result: 45 rows at Revision 2, 79 at Revision 1, 14 at Revision 0 (the 6 rows added this cycle, plus 8 unchanged since first authoring).

**One diff map for the cycle.** `_src/geminiCustomerExperienceGuidance-diff-map-2026-09-23.csv` was regenerated against the 2026-09-04 state, and each row's note now carries both an "Update:" and a "Review fix:" clause where both apply. Totals: 90 corrected, 28 strengthened, 14 carried, 6 new, 0 weakened.

**No merges.** Where a finding suggested merging rows (1050 into 1020; 690 with 820), the rows were kept and the inconsistency resolved instead: 1040 and 1050 were raised to High to match 1020, and 690 and 820 were split by data flow (Agent Assist and CX Insights Quality AI respectively).

## Material changes

| Change | Rows |
| --- | --- |
| Risk raised Med to High | 40, 800, 860, 930, 1030, 1040, 1050, 1060, 1150, 1320 |
| Directive changed | 480 MUST NOT to MUST (untestable wording replaced); 860 MUST to MUST NOT (testable key-version rule); 1150 SHOULD to MUST; 1190 MUST to MUST NOT (reads as a prohibition); 1200 MUST NOT to MUST (written confirmation of every API in the data path) |
| Audit logging corrected to the documented log types | 150, 570, 1050, 1070, 1080 (all three Data Access sub-types now required) |
| Role inheritance addressed | 160, 500, 530, 730 |
| Perimeter and egress | 970 adds `dialogflow.googleapis.com`; 1000 requires the allowed-origins policy with an ALWAYS scope; 1030 prohibits broad ingress and egress rules |
| Pre-GA basis rebased on the Pre-GA terms section 5(d) and Google's HIPAA guidance | 10, 20, 740, 800, 1270, 1320, 1350 |
| Factual corrections | 60 (TTL not irreversible), 250 and 310 (guardrail handoff is to an agent), 300, 440, 590, 600, 630 (Rationale now matches the 30-day ceiling), 660, 680, 910 (Service Specific Terms section 18), 960 (Access Approval), 1100, 1120, 1310 |
| HIPAA misfits removed | 164.514(a) on redaction rows (620, 720, 920, now SI-19); 164.526 on 940; 164.530(j) on retention rows; 164.312(a)(2)(i) on 270 and 370; 164.312(e)(2)(ii) on CMEK rows |
| Mapping coverage | HITRUST CSF v11 now on 111 of 138 rows (was 100); HIPAA on 114; 75 distinct NIST controls |
| OWASP Agentic edition relabeled | `OWASP Agentic Top 10 2026` on every ASI row |

## Not applied, or applied with a variation

- **ID 1380 (Info).** Whether File search tools with PHI should be MUST NOT rather than SHOULD NOT is an owner decision; it is in the workstream.
- **ID 600.** The state recording-consent statute is not cited, because no allowed reference domain carries state law. The Notes direct confirmation with counsel.
- **ID 310.** The row now names California Business and Professions Code section 17941 in its Rationale without a URL, for the same reason; its Mappings are PT-5, PT-5(1), and NIST AI RMF only, with HIPAA 164.520 and HITRUST 06.d removed as misfits.
- **ID 1210.** Compliance Reports Manager renders client-side, so the SOC 2 page, which names it, is cited instead.
- **eCFR URLs (IDs 300, 1100)** return HTTP 200 but serve a bot check to scripts; the text was confirmed through the eCFR renderer API. They are the canonical addresses for human readers.

## Verification

The merged CSV passed every generator Step 6 check with 0 errors, and all 98 distinct reference URLs returned HTTP 200 on 2026-09-23. See `notes/SCG-validation-2026-09-23.md`.

## Guidance document

`_src/geminiCustomerExperienceGuidance.md`, the DocGen `guidanceFile`, was rendered with the organization's template, `_docs/Cloud-security-requirements repo_2026-05/src/templates/svcGuidanceTemplate.jinja`. The render imports `processDocs.py` and calls its own `readcsv`, `contentPreprocessor` (six-digit zero-padded IDs, `snippets.json` replacement, `{;}` line breaks), and `writeTemplate`, so its preprocessing is the repository's code rather than a reimplementation. Two parameters differ from `generateMdFilev2`, following the Gemini RAG Engine and Agent Gateway precedent: the title uses the DocGen `title` rather than the camelCase `serviceName`, and the relative link prefix is fixed at `../../`, the depth of a `gcp/<service>/` folder under `cloudSecurityDocs`. The template and `processDocs.py` were not modified. `DocGen.json` now records `internalReview: true`.
