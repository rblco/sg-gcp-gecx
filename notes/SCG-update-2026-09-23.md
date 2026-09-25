---
content-type:
  - Update-Record
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-23
tags:
  - scg
  - gcp
  - update
---
# SCG update - Gemini Enterprise for CX - 2026-09-23

`_src/geminiCustomerExperienceGuidance.csv` was updated as an ID-stable merge: no ID was renumbered or removed, 10 rows were amended in place with one Revision increment each, and 6 requirements were added (IDs 1330 to 1380). The guide now holds **138 requirements across 12 categories**. The pre-update snapshot is `_src/archive/geminiCustomerExperienceGuidance.2026-09-23.pre-update.csv`, and every row's disposition is traced in `_src/geminiCustomerExperienceGuidance-diff-map-2026-09-23.csv` (122 carried, 6 corrected, 4 strengthened, 6 new).

Scope confirmed by the author before any change: the release notes since 2026-09-04, HITRUST CSF v11 mappings on applicable rows, re-verification of every reference, and a retry of the open documentation gaps. The author also decided that a row whose only change is an added HITRUST segment keeps its Revision; the change is recorded in the diff map instead.

## 1. Release notes (2026-08-25 to 2026-09-23)

| Stream | Newest entry | In-window entries | Disposition |
| --- | --- | --- | --- |
| Agent Assist | 2026-09-01 | Gemini Transcribe Live, Preview. The feature page states it "remaps traffic to the Vertex AI supported global region". | **New ID 1350** prohibits it while Preview. ID 680 gains it as a third residency limitation; ID 770 notes that `use_gemini_asr` routes transcription away from Cloud Speech-to-Text. |
| CX Agent Studio | 2026-07-01 | None | No change. |
| CX Insights | 2026-07-31 | None | No change. |
| Suite root | n/a | The page `gemini-enterprise-cx/release-notes` returns 404; the suite has no release notes of its own. | IDs 10 and 70 corrected from four streams to three. Agent Assist release notes added to ID 10's References, since the only in-window release was there. |

The Agent Assist release notes were recorded on 2026-09-04 as "rendered as navigation only". The raw HTML holds dated entries; the earlier outcome was produced by a page summarizer, and this update parsed the raw page instead.

**Found outside the release notes.** The CX Agent Studio tools overview lists seventeen tool types, including **File search tools** and **Widget tools**, against the sixteen the guide recorded. No release note announces either. File search tools create a RAG Engine knowledge base in a builder-selected location, and Google's RAG Engine overview states: "The VPC-SC security controls and CMEK are supported by Agent Platform RAG Engine. Data residency and AXT security controls aren't supported." This produced **new IDs 1360, 1370, and 1380**, extended IDs 390 and 400 to File search tools, and corrected ID 360's tool count. The REST tool resource also carries a remote agent tool type with no overview page; ID 360 notes it for the inventory, and no requirement governs it until Google documents it.

## 2. Launch stages re-verified

| Item | Stage on 2026-09-23 | Rows confirmed unchanged |
| --- | --- | --- |
| CX Agent Studio models | gemini-3-flash Preview; gemini-3.7-flash, gemini-3.5-flash, gemini-2.5-flash, gemini-3.1-flash-live GA | 1260 to 1290 |
| Google Maps tool | Preview | 40 |
| CX Insights fine-grained access control | Preview | 50, 730 |
| Conversation datasets, multiple scorecards | Preview by release note only; feature pages carry no banner | 50 (notes amended) |
| Agent Assist Build your own assist | Preview | 1320 |
| Commerce agents | "coming soon" | 20 |
| API versions | CX Agent Studio v1 and v1beta; CX Insights v1 (GA) and v1alpha1 | 30 |
| HIPAA covered products | GECX, Customer Experience Agent Studio, CX Insights, Contact Center AI Agent Assist, Conversational Agents named; Dialogflow not named | 1180, 1190, 1200 |

## 3. Open gaps from 2026-09-04

| Gap | Outcome | Rows |
| --- | --- | --- |
| Python code tool execution model | **Mostly closed.** The page is at `tool/python`, with a runtime reference at `reference/python`. Documented: sandbox on Python 3.12, import allowlist, public-internet-only egress with no private network access even with Service Directory, read access to the full conversation history and session state, write access to state, and the ability to call other tools in the app. **Still open:** the identity under which the sandbox runs and whether its HTTP client attaches a Google credential. Two documentation inconsistencies are recorded in the row's Notes. | 450 revised (Revision 2) |
| Client function tool data flow | **Closed.** The page is at `tool/function`: "Client function tools are always executed on the client side, not by the agent." The arguments go to the client and the session waits for its response. | 460 revised; new 1330 (client results must not establish identity or entitlement) |
| Data store retrieval ACL awareness | **Closed as far as documentation allows.** The REST data store tool definition has no end-user identity field, whereas the connector tool supports end-user authentication. An engine-backed tool with no data stores named searches all of the engine's data stores. | 390 revised; new 1340 |

## 4. HITRUST CSF v11

A HITRUST CSF v11 segment was added after NIST 800-53 on every row whose control touches access control, data protection or privacy, audit logging or monitoring, or transmission security: 94 existing rows and all 6 new rows, 100 of 138 in total. Rows outside those four areas are pure governance, launch-stage, guardrail, or change-management controls, and deliberately carry no segment. References and titles were taken only from the clusters in the scg-generator mappings guide (21 distinct references). **None has been verified against the organization's licensed HITRUST CSF v11 catalog.** In line with the author's decision, the 90 rows whose only change was the HITRUST segment keep their Revision. The 4 rows that also changed in substance (IDs 390, 460, 680, 770) are counted among the amended rows.

## 5. Reference re-verification

All 46 distinct reference URLs (35 carried, 11 added) returned HTTP 200 on 2026-09-23 with no redirects. No reference was replaced.

## 6. Deliberately not changed

- No requirement was retired. Every launch-stage prohibition still holds.
- Widget tools received no requirement, and this is an open item rather than a finding of no risk. Google's page states that the agent populates widget data against a declared schema and sends it to the client, and that the user's selection is sent back to the agent - the same outbound-disclosure and untrusted-return flow that IDs 460 and 1330 govern for client function tools. Those rows are worded for client function tools only. The next review should decide whether to extend them to widget tools or add parallel rows. The documented widget types (product carousel, product details, quick actions, product comparison, order summary) are commerce-oriented, which lowers but does not remove the exposure in a member-facing deployment.
- The Google Maps tool page recommends granting an "MCP Tool User (Beta)" permission. ID 40 already prohibits the tool, so no row was added.
- The overview's certification-posture narrative was updated. Its structure and the three original findings were not rewritten.
