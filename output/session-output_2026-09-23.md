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
# Session output - Gemini Enterprise for CX SCG update - 2026-09-23

## Recap

An ID-stable update of `_src/geminiCustomerExperienceGuidance.csv` covering the release notes since 2026-09-04, HITRUST CSF v11 mappings, reference re-verification, and the three documentation gaps left open on 2026-09-04. The guide moved from 132 to **138 requirements across 12 categories**. Ten rows were amended with one Revision increment each, six rows were added (IDs 1330 to 1380), and HITRUST CSF v11 was added to 100 rows. No ID was renumbered or removed.

Findings that changed the guide:

- **Gemini Transcribe Live** (Agent Assist, Preview, 2026-09-01) remaps live audio to the Vertex AI global region. It is prohibited (ID 1350) and recorded as a residency limitation (ID 680) and a transcription path (ID 770).
- **File search tools** appeared in the tools overview without a release note. They create a RAG Engine knowledge base, and RAG Engine supports neither data residency nor Access Transparency controls. New IDs 1360, 1370, and 1380 cover it; IDs 390 and 400 were extended to it.
- **Python code tools** can read the whole conversation, call every other tool in the app, and reach only the public internet (ID 450 revised). The sandbox's identity is still undocumented.
- **Client function tools** run on the end user's client (ID 460 revised), so their results must not establish identity or entitlement (new ID 1330).
- **Data store retrieval** has no end-user identity field in the REST schema (ID 390 now rests on the schema). An engine-backed tool with no data stores named searches all of them (new ID 1340).
- The suite has no release notes of its own, so there are **three** streams, not four (IDs 10, 70).

## Counts

| Metric | Value |
| --- | --- |
| Requirements | 138 (was 132) |
| Categories | 12 (unchanged) |
| Amended | 10: IDs 10, 50, 70, 360, 390, 400, 450, 460, 680, 770 |
| Added | 6: IDs 1330, 1340, 1350, 1360, 1370, 1380 |
| Retired | 0 |
| HITRUST CSF v11 rows | 100 (was 0) |
| Distinct references | 46, all HTTP 200 |
| Diff map | 122 carried (90 with HITRUST only), 6 corrected, 4 strengthened, 6 new |

## Artifacts

- `_src/geminiCustomerExperienceGuidance.csv` - updated guide
- `_src/geminiCustomerExperienceGuidance-diff-map-2026-09-23.csv` - row-by-row trace
- `_src/archive/geminiCustomerExperienceGuidance.2026-09-23.pre-update.csv` - pre-update snapshot
- `_src/DocGen.json` - moved from the folder root; `lastUpdated` 2026-09-23
- `notes/SCG-update-2026-09-23.md`, `notes/SCG-validation-2026-09-23.md` - update record and validation report, each with an adjacent PDF
- `resources.md`, `references.md`, `notes/external-source-log.md`, `README.md`, `_Gemini Enterprise for CX.md`, `prompt-log.md` - updated

## Recommendations

1. Verify the 21 HITRUST CSF v11 references and titles against the organization's licensed catalog before publication.
2. Decide whether IDs 460 and 1330 should extend to Widget tools, which send model-populated data to the client and return the user's selection.
3. Ask Google what identity the Python code tool sandbox runs as, and whether its HTTP client attaches a Google credential.
4. Run an independent scg-reviewer pass. The draft, review, remediation, and this update were all produced by the same agent.
5. Add the CX Agent Studio tools overview to the monthly release-note review, since tool types appear there without release notes.

## Errors in processing

- On 2026-09-04 the Agent Assist release notes were recorded as "navigation only". This session found that was a summarizer artifact; parsing the raw HTML showed real entries.
- The first draft of the update note named the wrong four rows as combining HITRUST with a substantive change, and claimed Widget tools had no distinct data surface. Both were caught by checking against the data and the page before the session ended, and corrected.
- No PDF tooling was installed (no pandoc, weasyprint, or wkhtmltopdf). PDFs were rendered with headless Chrome from HTML produced by the Python `markdown` package in a scratch virtual environment.

## Skill defects

- The skill's Step 1 scaffolder and "stop and ask whether to abort, overwrite, or merge" apply to new projects. The skill gives no explicit update path, so this session followed the precedent of the Gemini Enterprise recertification of 2026-09-21.
- The skill tree shows `DocGen.json` under `_src/`, but the 2026-09-04 run left it at the folder root. It was moved.
- The PDF-output section does not say which markdown files need PDFs. This session produced them for the three files it created: the update record, the validation report, and this session output.
- The skill's Step 6 check 3 says IDs ascend through the file, which conflicts with its own flat append-only rule once rows are added to earlier categories. The prompt log for 2026-09-04 records the same conflict.

## Deviations from the request

None in scope. The scaffolder was not re-run, because the folder already existed and was merged in place.

## Processing time, tokens, and cost

- Started 2026-09-23 20:44:30 MDT; completion time is recorded below.
- Tokens: about 200,000 in the main session context plus 181,519 across two research subagents (82,742 and 98,777), roughly 380,000 in total. These are approximate figures from the session's token counters, not billing records.
- Cost: not measurable from inside the session. No billing data is available to the agent.

## Completed

2026-09-23 20:59 MDT
