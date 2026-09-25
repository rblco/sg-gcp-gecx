---
title: SCG Review - Gemini Enterprise for CX
date: 2026-09-04
reviewer: scg-reviewer skill
file_under_review: _src/geminiCustomerExperienceGuidance.csv
review_depth: deep
---

# SCG Review - Gemini Enterprise for CX

## Executive Summary

Reviewed `_src/geminiCustomerExperienceGuidance.csv` at `deep` depth: 125 requirements across 11 categories, IDs 10 to 1250, all at Revision 0. The guide is structurally flawless - the canonical 17-column header, unique ascending IDs on multiples of ten, all eight downstream columns empty, pure ASCII, no embedded newlines, exactly one bolded directive per requirement, and every one of the 71 distinct NIST SP 800-53 identifiers and titles verified against the Rev. 5 OSCAL catalog with no fabrications and no withdrawn controls. All 34 cited URLs returned HTTP 200 with titles matching the requirement they support. **Zero Blockers.** The substance is where the defects are, and the most consequential ones come from a single root cause: four vendor pages that the authoring session could not read were readable at review time, and their contents contradict what the guide says about them. One requirement states a vendor retention default that is wrong by a factor of twenty-four, three requirements carry caveats disclaiming facts that are in fact documented, and one whole control domain - model selection - is absent from a product that offers a model picker containing a Preview model. **Verdict: fix the Majors, then publish.** None of these is a structural or citation-integrity failure, and the guide's architecture and reasoning are sound; the corrections are localized and mostly additive.

## Counts

| Severity | Count |
|----------|-------|
| Blocker | 0 |
| Major | 10 |
| Minor | 21 |
| Info | 5 |

## Findings

| # | Severity | Row ID | Category | Finding | Recommendation |
|---|----------|--------|----------|---------|----------------|
| 1 | Major | 630 | Agent Assist & Human Agent Support | Notes state "Leaving the setting at the vendor default is not a decision; the default is one year with a two-year maximum." Google's Agent Assist page states "The default and maximum retention window is 30 days" and that `retention_window_days` has "The default retention window in Agent Assist is 30 days." The one-year/two-year figures belong to the CX Agent Studio conversation history page and were carried onto an Agent Assist row. A reader following the guide would believe two-year retention is available where the product caps at 30 days. | Replace the retention figures with "default and maximum 30 days" and cite `agent-assist/data-redaction-retention`. Re-check whether the requirement's premise still holds - at a 30-day cap the row is closer to a verification control than a minimization control. |
| 2 | Major | n/a (category) | Coverage | No `## AI Models & Model Management` category and no requirement anywhere governing model selection. CX Agent Studio documents "You can select a global model for the entire agent application or override it for specific sub-agents", offers `gemini-3.7-flash`, `gemini-3.5-flash`, `gemini-3-flash`, `gemini-2.5-flash` and `gemini-3.1-flash-live`, and marks `gemini-3-flash` **Preview** in the same picker as the GA models. It further states "All models in CX Agent Studio are considered General Availability (GA) unless explicitly marked as 'Preview' in the UI. This launch stage is specific to CX Agent Studio and operates independently of a model's status in other Google Cloud products." | Add a category with at minimum: an approved model allowlist; a named prohibition on `gemini-3-flash` while it is Preview (mirroring how ID 40 handles the Google Maps tool); governance of per-sub-agent model override, which can silently place one sub-agent on a different model than the application; and a statement that CX Agent Studio launch stage is independent of a model's status elsewhere in Google Cloud. |
| 3 | Major | 610 | Agent Assist & Human Agent Support | The requirement mandates Agent Assist CMEK but omits a documented exclusion. Google states "CMEK is not available for features that are disabled in Agent Assist locations and smart reply." Smart Reply is explicitly outside CMEK coverage, and the guide separately requires Smart Reply governance at ID 650 without noting that its data is not customer-key protected. | Add the Smart Reply exclusion to Notes and cross-check it against the Smart Reply requirement, so a reader configuring both understands that one of them operates outside the encryption boundary the other establishes. |
| 4 | Major | 620 | Agent Assist & Human Agent Support | The requirement mandates inspect and deidentify templates but misses the binding step and the fail-open default. Google states "You must specify the security settings to apply for each conversation profile. If you don't specify any security settings in a conversation profile, Agent Assist doesn't apply any redaction for that conversation profile." Security settings are created at project-and-location level but take effect only where attached to a conversation profile; an unattached profile silently redacts nothing. | Extend the requirement or its Notes to require that security settings are attached to every conversation profile, and add a verification step enumerating profiles against attached settings. As written, a deployment can satisfy this row completely and still store unredacted transcripts. |
| 5 | Major | 610, 630, 680 | Agent Assist & Human Agent Support | Three rows carry caveats asserting that vendor documentation was unreadable - "The vendor CMEK page for Agent Assist did not render during authoring, so the immutability and coverage semantics are not established here" (610), and "The Agent Assist regionalization page did not render during authoring, so the specific region list and any per-feature exclusions were not established" (680). All four Agent Assist pages render and are readable at review time. A reviewer who opens the cited URL finds the answer the guide says is unavailable, which undermines confidence in every other caveat in the document. | Replace each caveat with the documented fact. For 610, Google states "You cannot change encryption key settings for a location once it has been specified" and names the `service-PROJECT_NUMBER@gcp-sa-ccai-cmek.iam.gserviceaccount.com` service agent - the immutability the row assumed is confirmed, so the requirement stands and only the hedge needs removing. |
| 6 | Major | 680 | Agent Assist & Human Agent Support | Two documented residency limitations are absent. Google notes that Vertex AI does not support the `us` multi-region, so "using Agent Assist's Generative AI features in 'us' multi-region will rely on the respective existing US single region endpoints" - pinning to `us` does not keep generative processing in the multi-region. Separately, "The AI/ML data location (data residency for ML processing, or DRZ) commitment is only supported in locations in the US and EU... For non-AI/ML data (in use or transit) or data outside US and EU multi-regions, the DRZ commitment doesn't apply." | State both. The first interacts directly with ID 870, which pins CX Agent Studio to the `us` multi-region: the two components pinned to the same nominal region do not produce the same processing locality for generative features. |
| 7 | Major | 1100 | Monitoring & Analytics | The audit-log retention requirement names no duration and does not cite `164.316(b)(2)`, the HIPAA provision requiring documentation retention for six years. The coverage baseline lists six-year audit retention as an always-required row for a HIPAA-covered SCG. As written the row defers entirely to "the organization's retention requirement" without establishing the regulatory floor. | Add `164.316(b)(2)` to Mappings and state the six-year floor explicitly in the requirement or Notes, so the row is actionable without a second lookup. |
| 8 | Major | multiple | Coverage | The OWASP Top 10 for Agentic Applications (ASI) is cited on zero rows. The coverage baseline is explicit: an SCG for a product that hosts, executes, or orchestrates agents with tool access that cites the LLM list on agent-behaviour rows and the ASI list on none is a Major coverage finding. This guide covers exactly that surface - sixteen tool types, agent-as-tool delegation, and MCP servers. | Add ASI citations alongside the existing LLM ones on the rows the baseline names: tool and skill allowlists (ASI02) at 380/490, agent identity and privilege scoping (ASI03) at 170/350, human confirmation for irreversible actions (ASI01, ASI09) at 300/650, memory and context poisoning (ASI06) at 400/410, code execution (ASI05) at 450, inter-agent communication (ASI07) at 330, and third-party MCP review (ASI04) at 420. ASI is additive to the NIST spine, never a replacement. |
| 9 | Major | 520, 590 | Deployment & Channel Security | Unbounded-consumption coverage is thin for the exposure. Only two rows address denial of service or resource consumption, and neither carries an OWASP LLM Top 10 2025 `LLM10 (Unbounded Consumption)` mapping, despite the guide itself establishing that the Google-hosted token broker permits anonymous access to a generative inference endpoint. Cost-based exhaustion against a per-token-billed model is the characteristic failure of this surface. | Add `LLM10` to 520 and 590, and consider a dedicated requirement for per-deployment quota caps and cost alerting, which neither existing row covers - the gateway rate limit at 590 does not constrain the web-widget channel at all. |
| 10 | Major | 170, 350, 380 | Tools & External Integrations | No requirement propagates an agent or session identifier to the backend systems that tools call. The guide establishes thoroughly that every tool call arrives at the backend as one project service agent, but stops at limiting what that identity can reach. The consequence is that the backend's own audit log cannot attribute an action to an agent, a conversation, or a member, so the attribution the monitoring category depends on exists only on the Google Cloud side of the boundary. | Add a requirement that tool calls carry a correlation identifier - conversation or session ID, and the agent application ID - in a header or request field that the backend logs, so a backend action can be traced to the conversation that caused it. This is the compensating control that makes the confused-deputy residual risk at ID 1230 investigable rather than merely accepted. |
| 11-23 | Minor | 190, 210, 220, 470, 620, 680, 720, 750, 790, 800, 870, 880, 1150 | multiple | Control enhancement cited without its parent control: `AC-6(7)` at 190 and 1150, `AC-2(11)` at 210 and 220, `SC-7(10)` at 470, 620, 720, 790 and 800, `SA-9(5)` at 680, 750, 870 and 880. Parent plus enhancement is the canonical citation form. | Add the parent control alongside each enhancement. Note that `SC-7(10)` at 620 and 720 is doing real work on a DLP row and the parent `SC-7` would be a boundary claim the row does not make - for those two, confirm the enhancement alone is intended before adding. |
| 24-25 | Minor | 260, 1160 | Agents; Monitoring | Lowercase `may` appears in the Requirement cell alongside the bolded directive, creating RFC 8174 ambiguity: 260 reads "terms the organization has determined must never appear" and 1160 reads "where conversation logging may be disabled". | Rephrase to avoid the lowercase modal - "terms the organization has designated as prohibited in a generated response" and "where conversation logging is disabled or can be disabled". |
| 26 | Minor | 70 | General | `SI-2 (Flaw Remediation)` is cited on a requirement about reviewing newly released vendor features. SI-2 governs patch and vulnerability remediation, not feature-release review. The other three controls on the row (`CM-3`, `CM-4`, `CA-7`) fit precisely. | Drop `SI-2`. |
| 27 | Minor | 1170 | Monitoring & Analytics | The incident-response row cites `164.404` and `164.410` but omits `164.408 (Notification to the Secretary)`, which the HIPAA overlay lists alongside them as an always-required breach-notification citation. | Add `164.408`. |
| 28-30 | Minor | 10, 50, 70, 190, 200, 710, 730, 740, 810, 820, 830, 890, 940, 1080, 1130, 1150, 1230, 1250 | multiple | Eighteen rows cite CX Insights pages under the legacy `docs.cloud.google.com/contact-center/insights/docs/` path. All three distinct URLs return 200 but via a 301 to the canonical `docs.cloud.google.com/gemini-enterprise-cx/insights/` path - the TTL page, the fine-grained access control best-practices page, and the CX Insights release notes. The references are live and land on the correct specific page, so this is hygiene rather than a broken citation. | Update to the canonical `gemini-enterprise-cx/insights/` URLs so the citations do not depend on a redirect the vendor may retire during a documentation migration that is evidently already under way. |
| 31 | Minor | multiple | Coverage | NIST AI RMF (AI 100-1) is cited on no row. The mappings reference lists it among the AI-specific frameworks expected on agent-bearing rows; OWASP LLM and MITRE ATLAS are both present, so the framework group is represented, but the risk-management function view is absent. | Optionally add function-level citations (`GOVERN`, `MAP`, `MEASURE`, `MANAGE`) on the governance rows - 60, 1230, 1240, 1250. Cite at function level only; subcategory identifiers should not be used without direct verification. |
| 32 | Info | 630, 1100 | multiple | `164.530(j)` is given the short name "Documentation and Retention". The section title in 45 CFR 164.530 is Administrative requirements, with (j) titled Documentation. | Consider "Documentation" for exactness, or leave as-is - the identifier is correct and unambiguous, and the descriptive name aids the reader. |
| 33 | Info | 610-690 | Agent Assist & Human Agent Support | The Agent Assist regionalization page names "Build your own assist (Preview)" in its regional feature table. The guide names no Agent Assist Preview feature, relying on the ID 10 baseline, and its own validation report flags this as a consequence of the unreadable pages. A named feature is now available. | Add a named prohibition row for "Build your own assist" while it is Preview, matching the pattern ID 40 uses for the Google Maps tool. |
| 34 | Info | n/a | Mappings | No `HITRUST:` segment appears on any row. This is correct - HITRUST is out of scope for this programme - and is recorded here only as a positive confirmation, not a finding. | No action. |
| 35 | Info | n/a | Calibration | Risk distribution is 87 High, 36 Med, 2 Low; cost is 62 Low, 62 Med, 1 High. Seventy percent of the guide is High risk, which compresses the triage signal a remediation owner needs. The distribution is defensible for a product whose failures are mostly irreversible-at-first-use, and the review found no individual row where High is wrong. | No change required. If a future revision wants sharper triage, the natural split is between rows that must hold before first use and rows that are ongoing operational discipline. |
| 36 | Info | 440, 500 | Tools; Deployment | Two rows were flagged by the automated `AC-3`-on-an-authentication-row heuristic and cleared on inspection. 440 governs a connection identity and correctly cites `IA-9`; 500 is an inventory requirement whose Requirement text merely mentions authentication mode. | No action. Recorded so the heuristic's output is not re-raised at the next review. |

## Coverage Analysis

### Categories

| Expected (universal + AI/LLM platform) | Present | Row count | Notes |
|----------------------------------------|---------|-----------|-------|
| `## General` | yes | 9 | Pre-GA prohibition at ID 10, Risk=High, correctly placed in the first category. |
| `## Identity & Access Management` | yes | 13 | Strong. Ten-role family, service-agent scoping, key prohibition, recertification. |
| `## Data Protection & Privacy` | yes | 13 | Strong. CMEK, residency, retention, deletion, key availability. |
| `## Network & Perimeter Security` | yes | 10 | Strong. All seven documented perimeter breakages covered. |
| `## Monitoring & Analytics` | yes | 11 | Strong, except the retention floor at finding 7. |
| `## AI Models & Model Management` | **no** | 0 | **Missing** - see finding 2. The product has a model picker containing a Preview model. |
| `## Agents & Agent Management` | yes | 12 | Guardrails, evaluation, delegation chains, human authority limits. |
| `## Tool / Connector Use` | yes (as `## Tools & External Integrations`) | 15 | The guide's strongest category. Naming differs from the catalog; the coverage is what matters. |
| `## Prompt & Output Safety` | partial | - | Folded into Agents (230-260) and Tools (410). Adequate as organized; no separate category needed. |
| `## Search & Grounding` | partial | - | Folded into Tools (390, 400, 470). Adequate. |
| `## Data Input & File Handling` | partial | - | Folded into Tools and Conversation Analytics ingestion rows. Adequate. |
| `## Content Generation` | partial | - | Folded into Agents guardrails and Agent Assist output rows. Adequate. |
| `## Personalization & User Experience` | n/a | - | The product exposes conversation history, not per-user memory. Correctly absent. |
| `## Translation & Language` | yes (in Agent Assist) | 1 | ID 670. A single row, correctly placed inside a broader category rather than given its own. |
| `## Compliance & Certification` | yes | 8 | Above baseline. BAA, covered-products, Dialogflow exclusion, residual risk. |

### STRIDE Coverage

| STRIDE | Addressed? | Row(s) | Notes |
|--------|------------|--------|-------|
| Spoofing | yes | 100, 110, 160, 170, 180, 200, 220, 370, 430, 440, 510, 530, 540 | Federation, MFA, service-agent scoping, key prohibition, token broker identity. Thorough. |
| Tampering | yes | 450, 460, 480, 640, 650, 670, 770, 1010, 1090, 1100, 1110 | Includes audit-log integrity at 1110, which is the one most guides omit. |
| Repudiation | yes | 200, 530, 1070, 1080, 1090, 1100, 1110, 1120, 1130 | Data Access logging on both API services, SIEM export, tamper protection, admin alerting. |
| Information Disclosure | yes | 40 rows | The guide's centre of gravity, appropriately. |
| Denial of Service | **partial** | 520, 590 | Two rows only, no LLM10 mapping, and the gateway rate limit at 590 does not cover the web-widget channel. See finding 9. |
| Elevation of Privilege | yes | 120, 130, 140, 150, 160, 170, 180, 190, 210, 220 | Separation of duties at 140 and just-in-time elevation at 210 both present. |

### Identity Governance Domains

| Domain | Addressed? | Row(s) | Notes |
|--------|------------|--------|-------|
| Identification | yes | 100, 110, 160, 170, 180, 200, 430, 440, 530 | Human, service, and connection identities all named. |
| Authentication | yes | 100, 110, 180, 200, 510, 530, 540 | MFA, federation, OAuth2 end-user path, service-account key prohibition. |
| Authorization & Delegation | yes | 330, 350, 380, 410, and 60 others | Agent-as-tool delegation chains at 330 are explicitly treated as an aggregating rather than attenuating surface, which is the correct and non-obvious reading. |
| Lifecycle | yes | 120, 130, 160, 190, 210, 220, 490 | Provisioning, recertification, and unused-tool removal. Deprovisioning is implicit in 190 rather than stated - acceptable. |
| Auditability | yes | 1070-1150 | Complete, subject to the retention floor at finding 7. |
| Trust Boundaries | yes | 330, 350, 430, 510, 560, 970-1060 | The project-as-boundary argument at 350 is the guide's spine. |
| **Agent attribution to downstream systems** | **no** | - | **Gap** - see finding 10. Every domain above is addressed on the Google Cloud side; none carries attribution across the tool boundary into the backend. |

## Authoritative-source Verification Log

All 34 distinct reference URLs were fetched. Every one returned HTTP 200 with a page title matching the requirement it supports; no 404s, no dead links, no redirect landing on a generic vendor page, no host outside the authoritative allowlist, no credentials or sensitive query parameters in any URL.

Three URLs resolve via a 301 redirect from the legacy `contact-center/insights/docs/` path to the canonical `gemini-enterprise-cx/insights/` path - the TTL page, the fine-grained access control best-practices page, and the CX Insights release notes. Each lands on the correct specific page, so the citations are sound; see finding 28-30 for the hygiene recommendation.

Two URLs cited in the guide's own supporting notes as unreachable were confirmed genuinely unreachable at review time: `cx-agent-studio/tool/python-code` and `cx-agent-studio/tool/client-function` both return 404, as do the alternate paths tried. The caveats on IDs 450 and 460 are therefore accurate and should stand unchanged. Four Agent Assist pages that the authoring session recorded as unreadable were readable at review time and produced findings 1, 3, 4, 5 and 6.

NIST SP 800-53 verification: 71 distinct control identifiers checked by identifier and exact title against the Rev. 5 OSCAL catalog. Zero fabricated identifiers, zero withdrawn controls, zero title mismatches. `SA-12` correctly absent. `AU-2` cited as Event Logging and `SA-9` as External System Services, both Rev. 5 titles rather than Rev. 4 carry-forwards. Three controls sharing the title "Cryptographic Protection" (`SC-8(1)`, `SC-13`, `SC-28(1)`) are each used with the identifier that fits the context.

HIPAA verification: every cited section resolves to a real Security Rule, Privacy Rule, or Breach Notification Rule subsection, and all are cited to subsection depth. One omission (`164.408`) and one missing overlay citation (`164.316(b)(2)`) are recorded as findings 27 and 7.

OWASP verification: the guide cites `OWASP LLM Top 10 2025` uniformly across all 20 LLM rows, which matches the edition currently presented as authoritative at `genai.owasp.org`. Identifier usage was checked against the 2025 numbering and is correct throughout - `LLM01` prompt injection, `LLM02` sensitive information disclosure, `LLM03` supply chain, `LLM05` improper output handling, `LLM06` excessive agency, `LLM09` misinformation. No cross-edition renumbering errors. `OWASP API Top 10 2023` is used on eight rows, all genuinely API-layer.

MITRE ATLAS verification: eight rows cite ATLAS techniques, all drawn from the verified identifier set (`AML.T0051.001`, `AML.T0053`, `AML.T0054`, `AML.T0110`) and each matched to an appropriate requirement.

## Top-3 Risks

1. **The Agent Assist retention figure is wrong (finding 1, ID 630).** The guide tells a reader the default is one year with a two-year maximum; the product's default and maximum are both 30 days. This is the only factual error found in the guide, and it is the kind that survives review because it reads plausibly - the figures are real, they simply belong to a different component. Anyone sizing a retention position for Agent Assist from this row will design against a control surface that does not exist.

2. **Model selection is entirely ungoverned (finding 2).** CX Agent Studio lets a builder pick the model for an application and override it per sub-agent, ships a Preview model in the same picker as the GA ones, and warns that its launch stages operate independently of a model's status elsewhere in Google Cloud. The guide's Pre-GA baseline at ID 10 technically covers this, but the same was true of the Google Maps tool and that got a named row at ID 40. A builder selecting `gemini-3-flash` from a dropdown will not connect the action to a general prohibition three categories away.

3. **Three rows disclaim facts that are documented (finding 5).** The caveats on IDs 610, 630 and 680 tell the reader that vendor documentation could not be read. It can. This matters beyond the three rows because the guide uses the same device honestly and correctly elsewhere - IDs 450 and 460 rest on pages that genuinely 404 - and a reviewer who checks one stale caveat has no way to tell which caveats are still true without checking all of them.

## Sign-off Checklist

- [x] All Blocker findings resolved - none were raised
- [x] Pre-GA / Preview prohibition present, Risk=High, in `## General` (ID 10)
- [x] NIST 800-53 cited on every row, all identifiers and titles verified against the Rev. 5 catalog
- [x] No HITRUST segment appears in the Mappings column
- [x] HIPAA cited on all 95 rows where the surface touches ePHI
- [x] References resolve to authoritative sources - 34 of 34 live, all on the allowlist
- [x] No requirement cross-references another, as required for a standalone guide
- [x] Defense in depth verified - exfiltration controlled at perimeter (970-1060), identity (170, 350, 380) and data layer (620, 720, 920)
- [x] Least privilege verified - no row grants broad permission without a compensating control
- [x] Assume-breach posture verified - audit-log integrity (1110), blast-radius limitation (350), incident containment (1170)
- [ ] Correct the Agent Assist retention figures at ID 630 (finding 1)
- [ ] Add model-selection governance (finding 2)
- [ ] Add the Smart Reply CMEK exclusion at ID 610 (finding 3)
- [ ] Add the conversation-profile binding and fail-open default at ID 620 (finding 4)
- [ ] Replace the stale "did not render" caveats at IDs 610, 630, 680 (finding 5)
- [ ] Add the two Agent Assist residency limitations at ID 680 (finding 6)
- [ ] Add `164.316(b)(2)` and the six-year floor at ID 1100 (finding 7)
- [ ] Add OWASP ASI citations on agent-behaviour rows (finding 8)
- [ ] Strengthen unbounded-consumption coverage and add `LLM10` (finding 9)
- [ ] Add backend attribution for tool calls (finding 10)
- [ ] Resolve the 21 Minor findings, or record a decision not to
- [ ] Independent reviewer confirms this report - the guide and this review were produced by the same agent in the same session

## Note on reviewer independence

This report was produced by the same agent that authored the guide earlier in this session, which the user was told before the review began. Self-review reliably finds mechanical defects and reliably under-finds errors of judgement, because the reasoning that produced a weak requirement is the same reasoning evaluating it. The mechanical results here - schema, ID hygiene, ASCII, citation verification, URL liveness - are as trustworthy as the scripts that produced them and can be re-run by anyone. The coverage and calibration judgements should be treated as a first pass, and the last checklist item above is not a formality.
