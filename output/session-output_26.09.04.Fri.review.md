---
content-type:
  - Session-Output
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-04
tags:
  - scg
  - gcp
---

# Session output - Gemini Enterprise for CX SCG review and remediation

Session date: 2026-09-04. Skills: `scg-reviewer`, then in-place remediation on explicit request. Author: R. Lucier. Model: Claude Opus 5 (1M context). This is the second output file for 2026-09-04; the authoring session is recorded in `session-output_26.09.04.Fri.md`.

## Summary of findings

The guide was reviewed at `deep` depth and passed every structural and citation-integrity check without exception. The defects were substantive, and eight of the ten Majors traced to a single root cause worth recording: four Agent Assist documentation pages that the authoring session recorded as unreadable were fully readable at review time. WebFetch had returned only navigation for them; a direct retrieval with HTML extraction returned the article bodies. Two of those pages contradicted the guide.

The most consequential single finding was a factual error. ID 630 told the reader that Agent Assist conversation retention defaults to one year with a two-year maximum. Google documents the default and the maximum as 30 days. The one-year and two-year figures are real but belong to the CX Agent Studio conversation-history page, and they had been carried across onto an Agent Assist row - the failure mode that survives review precisely because the numbers are genuine and only the attribution is wrong.

Three further Majors came from the same four pages: Smart Reply is excluded from Agent Assist CMEK coverage entirely, which the guide did not state while separately governing Smart Reply; Agent Assist redaction fails open and binds per conversation profile, so a project can satisfy the template half of ID 620 and still store unredacted transcripts; and Agent Assist generative features fall back to US single-region endpoints when pinned to the `us` multi-region, so two components pinned to the same nominal region do not produce the same processing locality.

The largest coverage gap was independent of all that. The guide contained no requirement governing model selection, and CX Agent Studio has a model picker with per-sub-agent override that ships `gemini-3-flash` as a Preview entry alongside generally available models. Google's own wording is the trap: all models are considered generally available unless marked Preview in the interface, and that launch stage "operates independently of a model's status in other Google Cloud products", so checking a model's Vertex AI page reaches a conclusion that does not govern this product.

## Deliverables

Two reports and one amended CSV.

- `notes/SCG-review-2026-09-04.md` - the review report: 0 Blockers, 10 Major, 21 Minor, 5 Info, with coverage analysis against STRIDE and the six identity-governance domains, a per-URL verification log, top-3 risks, and a sign-off checklist.
- `notes/SCG-remediation-2026-09-04.md` - what changed, what did not, and the post-remediation verification results.
- `_src/geminiCustomerExperienceGuidance.csv` - amended from 125 requirements in 11 categories to **132 in 12**. Pre-remediation snapshot preserved at `_src/archive/geminiCustomerExperienceGuidance.PRE-REVIEW-2026-09-04.csv`.

Supporting documents updated to match: the project overview, README, validation report, and prompt log.

## Recommendations

Three items remain open and none is closable by editing the guide. The Python code tool page still returns HTTP 404 at every path tried, re-confirmed during remediation, so the only tool type that executes arbitrary code is constrained by review rather than by any documented technical boundary - verify its execution model with Google before it touches member data. Whether data store retrieval is access-control aware still rests on documented silence, and ID 390 takes the conservative reading; a definitive answer either closes a real exposure or permits relaxing a High-risk requirement. And Google's model-training position still needs to be obtained contractually across all four components under ID 910.

Two recommendations for the library rather than the guide. The `OWASP Agentic Top 10 2025` edition string used on twelve rows should be re-verified when that catalog's numbering settles, since it was announced in December 2025 and this is the first guide in the library to cite it. And three guides still carry the pre-2026-09 column order - Azure Bot Service, Conversational Analytics API, and the archived Gemini Enterprise CX draft; reconciling them would remove an ambiguity the reviewer skill currently has to explain in prose.

## Errors in processing

No tool errors or failed writes. One patch script aborted on a failed assertion before writing anything, because a text anchor for the project overview existed in README.md rather than in the overview; located and corrected, no partial write occurred.

One self-correction worth recording. The validation script written during the authoring session asserted that IDs ascend in document order. That check is wrong as a general rule and passed only because the file was an untouched initial draft. Adding a requirement to an early category correctly assigns it the highest existing ID plus ten, which breaks ascending document order by design - the reference material states this explicitly, and the check was retired rather than the guide being distorted to satisfy it. Placing the new AI Models category at the end of the file purely to keep IDs ascending would have been the wrong trade: category order is a readability decision and IDs record when a requirement was added, not where it sits.

## Skill defects observed

**1. `scg-reviewer` SKILL.md Step 1 states a non-canonical column order.** It lists Notes, References and Mappings at positions 7 to 9 with Data_Levels and Audit Procedures at 10 and 11. The template it names as the authority has Data_Levels and Audit Procedures at 7 and 8. The skill does say the template wins where they disagree, so the outcome was correct, but the header block in Step 1 should simply be fixed - a reviewer following it literally would raise a spurious Blocker against every correctly-formed guide.

**2. The HIPAA shape regex in `validation-checklist.md` §9 M4 rejects valid citations.** It is written as `164\.\d{3}(\([a-z0-9]+\))*`, which cannot match a subsection containing an uppercase letter. The same file's own example, `164.308(a)(1)(ii)(A)`, fails it. The character class needs `A-Za-z0-9`.

**3. Both skills assert globally ascending IDs.** `scg-generator` SKILL.md Step 6 check 3 says IDs are "ascending through the file", and `scg-reviewer` D4 checks ascent within a category. Neither survives the first amendment made under the flat append-only rule that the same reference files describe in detail. The checks should be uniqueness, multiples of ten, and no renumbering.

**4. WebFetch silently returned navigation-only content for four pages that have article bodies.** This is a harness observation rather than a skill defect, but it has a direct process consequence for this workflow: a page that returns HTTP 200 with no readable body is indistinguishable, to the authoring agent, from a page with nothing to say. Both scg skills should prescribe a second retrieval method before an author records a documentation gap, because a false gap propagates into requirement Notes as a durable and misleading caveat.

## Deviations from the request

None. The review was run at the depth and against the file the user selected, the CSV was not modified during the review phase, and the edits were applied only after the user explicitly asked for them.

## Session metrics

- Wall-clock processing time: approximately 25 minutes for review and remediation combined.
- Tokens consumed this phase: approximately 85,000, with roughly 14.96M remaining of the session budget. Exact billing figures are not exposed to the model.
- Cost: not exposed to the model; deliberately not estimated.
- Web retrievals: 34 URL status-and-title checks, 4 direct page extractions, 1 documentation fetch, 1 search, plus re-verification of 4 changed or new URLs.
- Rows revised: 53. Rows added: 7. Existing IDs renumbered or removed: 0.
- Completed: 2026-09-04.
