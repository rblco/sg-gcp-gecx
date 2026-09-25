---
content-type:
  - Remediation-Log
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-04
tags:
  - scg
  - gcp
---

# Remediation log - Gemini Enterprise for CX

Records what was changed in `_src/geminiCustomerExperienceGuidance.csv` in response to `notes/SCG-review-2026-09-04.md`, and what was deliberately not changed. The pre-remediation file is preserved at `_src/archive/geminiCustomerExperienceGuidance.PRE-REVIEW-2026-09-04.csv`.

## Outcome

All 10 Major findings and all 21 Minor findings are resolved. Of the 5 Info findings, 2 were applied and 3 required no action. The guide moved from 125 requirements in 11 categories to **132 requirements in 12 categories**. Fifty-three existing rows were revised in place and incremented to Revision 1; seven new rows were added at Revision 0. **No existing ID was renumbered, reused, or removed** - all 125 prior IDs are present and carry their original numbers.

## A note on the new IDs and document order

The seven new requirements take IDs 1260 through 1320, the highest prior ID plus ten and upward, and they sit in the categories where they belong rather than at the end of the file. IDs 1260 to 1290 appear in a new category positioned between Agents and Tools; 1300 sits in Deployment, 1310 in Tools, and 1320 in Agent Assist. The file therefore no longer reads 10, 20, 30 straight through in document order, and that is the intended result of flat append-only numbering rather than a defect. An ID records when a requirement was added, not where it sits. Any validation script that asserts globally ascending IDs is encoding the initial-draft state rather than the rule, and should be relaxed to check uniqueness, multiples of ten, and the absence of renumbering.

## Major findings - all resolved

**Finding 1 - ID 630, wrong retention figures.** The Notes claimed "the default is one year with a two-year maximum", which are the CX Agent Studio conversation-history values. Replaced with Google's Agent Assist statement that "The default and maximum retention window is 30 days", set through `retention_window_days`. The correction changes the character of the requirement, so the Notes now say so directly: 30 days is a ceiling rather than a starting point, and a business need longer than 30 days cannot be met by changing this setting - it has to be met by exporting to a store the organization controls. Revision 1.

**Finding 2 - no model governance.** Added `## AI Models & Model Management` with four requirements. ID 1260 requires an approved model allowlist. ID 1270 prohibits any model designated Preview in the picker, naming `gemini-3-flash` as the current instance while directing the reader to verify the live designation. ID 1280 governs per-sub-agent model override, which is invisible from the application-level configuration. ID 1290 requires the launch stage to be read from the CX Agent Studio interface rather than inferred elsewhere, quoting Google's statement that the CX Agent Studio launch stage "operates independently of a model's status in other Google Cloud products".

**Finding 3 - ID 610, Smart Reply CMEK exclusion.** Notes now carry Google's exclusion verbatim - "CMEK is not available for features that are disabled in Agent Assist locations and smart reply" - and instruct that the exclusion be recorded in the data flow record and weighed when deciding whether Smart Reply is enabled at all. The stale caveat was replaced with the documented immutability statement and the CMEK service agent name. Revision 1.

**Finding 4 - ID 620, redaction fail-open.** Notes now lead with the binding rule and the fail-open default, quoting Google: security settings must be specified per conversation profile, and "If you don't specify any security settings in a conversation profile, Agent Assist doesn't apply any redaction for that conversation profile." Verification is now defined as enumerating every conversation profile and confirming an attachment on each, with the explicit warning that a project can satisfy the template half of the requirement and still store unredacted transcripts. Revision 1.

**Finding 5 - stale caveats.** The "did not render during authoring" hedges on IDs 610, 630 and 680 are gone, replaced by the facts those pages actually document. The equivalent caveats on IDs 450 and 460 were left in place unchanged, because the Python code tool and client function tool pages were re-checked during remediation and still return HTTP 404 at every path tried.

**Finding 6 - ID 680, residency limitations.** Notes now carry both documented limitations: that Vertex AI does not support the `us` multi-region so Agent Assist generative features fall back to US single-region endpoints, and that the AI/ML data location commitment covers only US and EU locations and does not apply to data in use or in transit. The first is cross-referenced in substance to the CX Agent Studio `us` pin, since two components pinned to the same nominal region do not produce the same processing locality. Revision 1.

**Finding 7 - ID 1100, six-year retention floor.** Notes now state the HIPAA 164.316(b)(2) six-year floor explicitly and note that Cloud Logging's default bucket retention is far shorter. `164.316(b)(2) (Time Limit)` added to Mappings. Revision 1.

**Finding 8 - OWASP Agentic (ASI) absent.** ASI citations added to eleven agent-behaviour rows, additive to the existing NIST spine and LLM citations: ASI03 on 170 and 350, ASI09 on 300 and 650, ASI07 on 330, ASI02 on 380 and 490, ASI06 on 400, ASI01 and ASI06 on 410, ASI04 on 420, ASI05 on 450. The new attribution requirement at ID 1310 also carries ASI03. Edition string used is `OWASP Agentic Top 10 2025`, matching the December 2025 announcement; re-verify the string when the catalog's final numbering settles.

**Finding 9 - unbounded consumption.** `LLM10 (Unbounded Consumption)` added to IDs 520 and 590, and a new requirement at ID 1300 covers per-deployment quota caps and cost alerting. The new row exists because the gateway rate limit at 590 constrains only the API channel and sits nowhere in the path of the web widget or the telephony connections, which is where anonymous access actually lands.

**Finding 10 - backend attribution.** New requirement at ID 1310 requires tool invocations to carry a correlation identifier - agent application ID plus conversation or session ID - that the receiving backend records in its own logs, with the explicit constraint that the identifier is metadata and must not carry member identifiers or conversation text. This is the control that makes the confused-deputy residual risk investigable after the fact rather than merely accepted.

## Minor findings - all resolved

Thirteen enhancement citations gained their parent control: `AC-6` on 190 and 1150, `AC-2` on 210 and 220, `SC-7` on 470, 620, 720, 790 and 800, `SA-9` on 680, 750, 870 and 880. Two lowercase modals were rephrased - ID 260 now reads "terms the organization has designated as prohibited in a generated response" and ID 1160 reads "where conversation logging is disabled or can be disabled". `SI-2 (Flaw Remediation)` was dropped from ID 70, leaving the three controls that fit. `164.408 (Notification to the Secretary)` was added to ID 1170. Eighteen rows citing the legacy `contact-center/insights/docs/` path were canonicalized to `gemini-enterprise-cx/insights/`, and all three replacement URLs were confirmed to return HTTP 200 directly rather than through a redirect. NIST AI RMF function-level citations were added to the four governance rows, 60, 1230, 1240 and 1250.

## Info findings

Applied: the `164.530(j)` short name was corrected from "Documentation and Retention" to "Documentation" on all ten rows citing it, and a named Preview prohibition for the Agent Assist "Build your own assist" capability was added at ID 1320, matching the pattern ID 40 uses for the Google Maps tool.

Not applied, no action needed: the absence of HITRUST is correct and was recorded as a positive confirmation; the Risk distribution skew is defensible for a product whose failures are mostly irreversible at first use, and no individual row was found where High is wrong; and the two rows flagged by the `AC-3`-on-an-authentication-row heuristic were confirmed correct on inspection.

## Post-remediation verification

Re-ran the full check set against the amended file. Header canonical; all 132 rows parse to 17 fields; category rows clean; IDs unique and all multiples of ten; all 125 prior IDs present and unchanged; the eight downstream columns empty on every row; required columns populated; Risk and Cost within enum; exactly one bolded directive per requirement; pure ASCII; no embedded newlines; NIST 800-53 on every row; no cross-references between requirements; no HITRUST segment; no lowercase modal in any Requirement cell; every enhancement now accompanied by its parent; mapping segments in priority order on every row.

Citation integrity re-verified: 71 distinct NIST identifiers and titles checked against the Rev. 5 OSCAL catalog with zero problems, including the newly added `CP-2 (Contingency Plan)`. All reference URLs, including the four new and three canonicalized ones, return HTTP 200.

Revision hygiene confirmed mechanically: all 53 rows whose content changed were incremented to Revision 1, and no row was incremented without a content change.

## What still needs a human

Three of the five original verification items in `notes/SCG-validation-2026-09-04.md` remain open and are not remediable by editing the guide. The Python code tool execution model is still undocumented and still needs to be confirmed with Google before that tool type touches member data. Whether data store retrieval is access-control aware still rests on documented silence, and ID 390 still takes the conservative reading. Google's model-training position across all four components still needs to be obtained contractually under ID 910. The perimeter sequencing requirement at ID 980 also remains a derived inference rather than a vendor quotation, which is stated in its own Notes.

The independence caveat on the review report stands: the guide, the review, and this remediation were produced by the same agent in one session.
