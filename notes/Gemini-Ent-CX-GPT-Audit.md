# Gemini Enterprise for CX security guide: requirement review

Reviewed 24 September 2026 against `_src/geminiCustomerExperienceGuidance.csv` (138 numbered requirements; 12 section headers). This is a review and proposed disposition, not an edit to the source guide. “Retain” in the row register means no material defect was identified in this review; it does not certify a deployment or a licensed HITRUST mapping.

## Priority corrections

| IDs | Finding and recommended correction | Evidence |
| --- | --- | --- |
| 320, 10 | **Conflict on launch stage.** The note says Start with AI reached GA in February 2026, but its feature page still displays **Preview** and the Pre-GA disclaimer. Treat it as barred from production PHI use under ID 10 until the feature page changes; keep ID 320 as a review rule for any later GA use. Replace the product-launch inference in its note. | [Start with AI](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/quick/generate-agent); [February launch notes](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/resources/release-notes) |
| 10, 50, 740, 1270 | **Pre-GA scope is internally inconsistent.** ID 10 prohibits *all* use of Pre-GA features; IDs 50 and 740 allow non-production evaluation of authorized views, and ID 1270 bars Preview models only for production traffic. Define whether ID 10 covers all environments or production/PHI. If non-production evaluation is allowed, specify synthetic/non-PHI data, isolation, and a prohibition on production dependency. | [CX Insights authorized views](https://docs.cloud.google.com/gemini-enterprise-cx/insights/overview-of-fine-grained-access-control) |
| 30 | **Unsupported API-stage inference.** The note concedes that Pre-GA status is inferred from `v1beta`/`v1alpha1` naming. A version string alone does not establish launch stage, HIPAA coverage, or BAA status. Keep a production API-version allowlist if that is organization policy, but state it as policy and obtain a vendor statement for any coverage assertion. Explain how v1beta-only evaluations can be performed outside production. | [CX Agent Studio API references](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/resources/release-notes) |
| 120, 140, 150, 160, 530 | **Unusable IAM identifiers.** The guide uses `ces.googleapis.com/admin`, `.../securitySettingsEditor`, `.../deploymentEditor`, `.../client`, and `ces.googleapis.com/tools.execute` as though they were IAM role/permission IDs. For binding and custom-role instructions, use `roles/ces.admin`, `roles/ces.securitySettingsEditor`, `roles/ces.deploymentEditor`, `roles/ces.client`, `ces.tools.execute`, `ces.sessions.runSession`, and `ces.sessions.bidiRunSession`. Audit all occurrences, including notes. | [Canonical IAM roles and permissions](https://docs.cloud.google.com/iam/docs/roles-permissions/ces) |
| 110 | **MFA scope is impossible as written.** “Every account” includes non-human service identities. Require MFA for human/workforce principals with these roles; govern service accounts through workload identity, key prohibition (ID 180), and least privilege. | [Google service-account best practices](https://docs.cloud.google.com/iam/docs/best-practices-service-accounts) |
| 130, 150, 500, 580 | **New Meta channel creates a permission conflict.** The 24 September release adds WhatsApp/Instagram; the channel guide says only project Owners can create its deployment. ID 130 bans Owner, while ID 150 expects the deployment editor to promote changes. Do not silently exempt Owner: either obtain a least-privilege supported path from Google or treat the channel as unavailable under this baseline. Add Meta to channel inventory and third-party PHI/BAA review. | [Release notes](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/resources/release-notes); [WhatsApp/Instagram deployment](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/deploy/whatsapp) |
| 330, 360, 500, 590, 1220 | **New agent-to-agent boundary is unaddressed.** The 24 September release adds A2A Protocol tools and explicitly marks Agent as a tool GA. Update ID 330 to cover external/remote agents, inbound and outbound authentication, context/PHI transfer, effective tool reach, and cross-project trust. Add A2A to tool inventory, gateway/perimeter design, and data-flow inventory. Correct ID 330’s launch-stage note. Also assess new Supervisor agents and composite voice model against the capability baseline. | [Release notes](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/resources/release-notes); [A2A tools](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/a2a-protocol) |
| 140, 1120 | **Security-setting scope is overstated.** The `securitySettingsEditor` role governs project Security Settings; agent application logging, redaction and export settings are controlled separately through app editing. Revise ID 140’s rationale and separation-of-duties test to include `roles/ces.appEditor` where it can change protected application settings. ID 1120 already reflects the split. | [SecuritySettings resource](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/reference/rest/v1beta/SecuritySettings); [Application settings](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/settings) |
| 370 | **Credential rule is too broad.** OpenAPI/MCP can use generated identity tokens or OAuth flows; these are not static secrets to store in Secret Manager. Apply the Secret Manager rule to stored static API keys, passwords, client secrets and tokens, while separately requiring short-lived identity credentials and safe token handling. | [OpenAPI tool authentication](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/open-api) |
| 380 | **Requirement contradicts its note.** It forbids any backend containing a broader data population, while the note allows a backend that enforces a verified session-bound member scope. Say the tool may reach a broader backend only when the backend enforces object-level authorization from a non-model-controlled identity. Otherwise reject it. | [OpenAPI session context](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/open-api) |
| 400 | **Source-scope contradiction.** The requirement permits only sources under organization control; the note calls for periodic re-review of third-party content. Decide whether approved third-party snapshots are allowed. If yes, state the admission criteria and change/re-ingestion review; if no, remove the third-party exception language. | [Data store tools](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/data-store) |
| 500, 580 | **Channel list is stale.** The note’s “eight distinct” deployment surfaces and the enumerated vendor list no longer cover new WhatsApp/Instagram options. Replace the fixed count with a maintained channel inventory and scope ID 580 to every non-Google intermediary, including Meta. | [Release notes](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/resources/release-notes) |
| 1190, 1180, 1200 | **BAA scope is overstated and repetitive.** The Google HIPAA list includes the Gemini Enterprise for Customer Experience umbrella and named components, so absence of an individual feature name is not conclusive exclusion. Require a documented product-to-covered-product mapping and written Google clarification where ambiguous (especially `dialogflow.googleapis.com`/Dialogflow ES). Consolidate the three controls while preserving a clear prohibition on uncovered PHI processing. | [Google HIPAA covered products](https://cloud.google.com/security/compliance/hipaa) |

## Consolidation and clarity candidates

| IDs | Disposition |
| --- | --- |
| 20, 40, 50, 1270, 1320, 1350 under 10 | These are largely feature-specific instances of the Pre-GA prohibition. Preserve a current blocked-feature inventory in ID 10’s notes or evidence; retire separate rows where they add no independently testable configuration. Retain a separate row only where there is a distinct technical test (for example model selection). |
| 70, 1250 | Combine release monitoring and mandatory reassessment on Preview-to-GA transitions. ID 1250 repeats ID 70’s trigger and cadence. |
| 1260, 1270, 1290 | Put launch-stage verification and its evidence in the model allowlist control (1260); keep 1270 only if model-picker prohibition is tested separately. ID 1290 is chiefly an implementation note. |
| 280, 290, 320 | Keep security review and evaluation as separate tests. ID 320 can become a note or test case under 280 once Start with AI is GA; until then ID 10 governs its production use. |
| 480, 1010 | ID 1010 repeats ID 480 for OpenAPI targets. Consolidate into ID 480, preserving Service Directory setup instructions and dedicated-tool scope. |
| 690, 820 | Same workforce-monitoring approval for Agent Assist/CX Insights; one cross-component control is sufficient. |
| 850, 860 | Both cover key disablement/destruction. Keep the strict “never while encrypted data is retained” test in 860; narrow 850 to monitoring and multi-party change approval, or combine them. |
| 950, 1220 | Transient-cache documentation is a detail of the full data-flow inventory. Move ID 950 into ID 1220 notes; no separate product configuration exists. |
| 1070, 1080 | Similar logging controls for different services; they may remain separate because each API requires an independent audit-log setting and test. If consolidated, name both services and all three subtypes explicitly. |
| 360, 490 | Keep both inventory and least-functionality tests, but correct rationale: registration alone does not give an agent access; assignment to an agent does. Inventory both registered and assigned tools. |
| 430, 450 | Remove overconfident or long explanatory notes. A Cloud Run MCP endpoint may have other IAM-authorized callers; a perimeter alone does not make the agent path exclusive. ID 450’s long documentation reconciliation belongs in supporting evidence, leaving a short code-review and execution-boundary requirement. |
| 340 | Remove `CP-9` unless actual backups of agent configuration are required. A changelog or export retained for investigations maps more directly to `CM-3`, `AU-11`, and `SI-12`. |
| 600 | Change “before content is captured” to a testable disclosure/recording point appropriate to the channel. Remove HIPAA `164.502(a)` as a recording-consent authority; the note correctly identifies state recording law as the legal driver. |
| 1100 | Keep the chosen retention period, but label six years as organization policy/interpretation. HIPAA `164.316(b)(2)` prescribes retention of required documentation, not a universal six-year audit-log minimum. |

Additional wording and mapping adjustments: ID 300 should qualify the ERISA citation to ERISA-governed benefit plans and avoid treating `SI-18` as a direct adverse-determination control; ID 310 should cite the particular disclosure law if it remains in the rationale. ID 320’s `SA-11` mapping is weak without a testing requirement. ID 430’s “only through the agent” claim is too absolute. ID 450’s HIPAA `164.312(e)(1)` is not a direct code-review mapping. ID 520’s HITRUST `01.j` and OWASP API2 authentication mappings do not match origin checking/reCAPTCHA for an intentionally anonymous widget; retain abuse and resource-consumption mappings. ID 540’s HIPAA `164.312(d)` should be removed if its agent remains intentionally unauthenticated. IDs 630 and 710 map conversation retention to `AU-11` audit-record retention; `SI-12` is the closer match. ID 810’s HIPAA `164.514(a)` concerns de-identification, not classifying derived artifacts as sensitive. IDs 890 and 900 use HIPAA Privacy Rule documentation `164.530(j)` for conversation retention/logging settings; this is not a direct control mapping. ID 940’s HIPAA `164.504(e)` concerns BA contracts rather than the mechanics of deleting all copies. ID 980’s note should avoid an exact count of affected operations. ID 1150’s `AC-6(7)` is privilege review, not anomalous-read detection. ID 1170’s HIPAA `164.410` is the business associate’s notification obligation, not a direct mapping for this organization’s incident plan. ID 1240 needs a direct inventory/IAM or AI governance reference instead of only broad product landing pages. IDs 1360–1380 should state explicitly that a US **region** for RAG Engine does not itself supply a contractual data-residency guarantee; verify the File search creation path and covered-product mapping before allowing PHI.

## Link and standards audit

The CSV has **98 distinct reference URLs**. The prior [23 September validation](SCG-validation-2026-09-23.md) recorded HTTP 200 for all 98. On this review, 96 rendered through the web fetcher; the SecureCo deployment and CX Insights `bulkDelete` REST pages failed in that fetcher but loaded in the browser with the expected content. These are **fetcher failures, not confirmed broken links**. One eCFR reference to 29 CFR 2560.503-1 redirects from `subchapter-F` to `subchapter-G`; replace it with the canonical `subchapter-G` path. No confirmed dead reference was found.

Reachability is not the same as relevance. A substantial set of citations use broad overview, deploy-index, or tool-index pages; those are acceptable for general background but insufficient as the only proof of a specific setting. Prioritize direct feature or API pages for IDs 30, 320, 330, 500, 530, 590, 1240, and 1360–1380. The `whatsapp` and A2A pages above are examples of the now-needed specific citations. The release notes should supplement, not replace, a feature page when the two disagree on launch stage.

The existing [validation record](SCG-validation-2026-09-23.md) checked NIST 800-53 Rev. 5 identifier/title existence against NIST OSCAL; this review flags **semantic** mismatches above. For example, [45 CFR 164.316](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-C/section-164.316) applies its six-year period to required documentation. HIPAA and 29 CFR provisions were checked for applicability in the cited rows, not as a legal opinion. HITRUST CSF v11 exact control titles and semantic mappings could not be independently certified from the public material available; validate these against the licensed v11 catalog before publication. OWASP LLM mappings are explicitly to the **2025** edition; retain that year in the guide and set a separate decision to update to the 2026 edition, rather than silently mixing versions.

## Per-requirement disposition

“Retain” means no material change proposed. “Revise” identifies a correction in the tables above. “Merge candidate” requires preserving the distinct test if it exists. Rows are listed in the CSV’s order.

| ID | Disposition | Review note |
| --- | --- | --- |
| **General** | | |
| 10 | Revise | Keep feature-level Pre-GA rule; maintain a current blocked-feature register. |
| 20 | Merge candidate | Commerce is a specific Pre-GA/BAA case of 10 and 1180. |
| 30 | Revise | Do not infer contractual or launch stage from API version naming. |
| 40 | Merge candidate | Specific Pre-GA block; preserve a Maps-tool test if separately audited. |
| 50 | Merge candidate | Specific Preview block; retain narrower production-dependency nuance. |
| 60 | Retain | No material issue identified in this review. |
| 70 | Merge candidate | Combine release monitoring and transition reassessment with 1250. |
| 80 | Retain | No material issue identified in this review. |
| 90 | Retain | No material issue identified in this review. |
| **Identity & Access Management** | | |
| 100 | Retain | No material issue identified in this review. |
| 110 | Revise | Scope MFA to human principals; service identities need separate controls. |
| 120 | Revise | Use canonical roles/ces.admin role ID. |
| 130 | Revise | Resolve Owner-only Meta channel deployment conflict before use. |
| 140 | Revise | Use canonical role IDs; include appEditor in settings duties analysis. |
| 150 | Revise | Use canonical role ID; address Owner-only Meta deployment. |
| 160 | Revise | Use canonical roles/ces.client role ID. |
| 170 | Retain | No material issue identified in this review. |
| 180 | Retain | No material issue identified in this review. |
| 190 | Retain | No material issue identified in this review. |
| 200 | Retain | No material issue identified in this review. |
| 210 | Retain | No material issue identified in this review. |
| 220 | Retain | No material issue identified in this review. |
| **Agents & Agent Management** | | |
| 230 | Retain | No material issue identified in this review. |
| 240 | Retain | No material issue identified in this review. |
| 250 | Retain | No material issue identified in this review. |
| 260 | Retain | No material issue identified in this review. |
| 270 | Retain | No material issue identified in this review. |
| 280 | Retain | Keep distinct predeployment security approval; cross-reference 320. |
| 290 | Retain | Keep distinct evaluation test; do not merge into approval alone. |
| 300 | Revise | Qualify ERISA applicability; drop tangential SI-18 mapping. |
| 310 | Revise | Add a direct citation if a state bot-disclosure law is relied upon. |
| 320 | Revise | Start with AI feature page still says Preview; correct note and mapping. |
| 330 | Revise | Agent-as-tool now GA; extend to external A2A trust boundary. |
| 340 | Revise | Remove CP-9 unless an actual backup control is specified. |
| **AI Models & Model Management** | | |
| 1260 | Revise | Record feature-specific model stage and evidence in allowlist. |
| 1270 | Merge candidate | Specific model Preview rule under 10; preserve picker test. |
| 1280 | Retain | No material issue identified in this review. |
| 1290 | Merge candidate | Picker verification is implementation detail of 1260. |
| **Tools & External Integrations** | | |
| 350 | Revise | Soften note implying relocation always requires recreation. |
| 360 | Revise | Inventory A2A tools and distinguish registration from assignment. |
| 370 | Revise | Scope Secret Manager to stored static credentials. |
| 380 | Revise | Permit broader backend only with verified per-user backend authorization. |
| 390 | Retain | No material issue identified in this review. |
| 400 | Revise | Resolve organization-controlled versus third-party-content conflict. |
| 410 | Retain | No material issue identified in this review. |
| 420 | Retain | No material issue identified in this review. |
| 430 | Revise | Do not claim VPC perimeter alone makes MCP reachable only via agent. |
| 440 | Retain | No material issue identified in this review. |
| 450 | Revise | Shorten long note; remove HIPAA transmission-security mapping unless egress is tested. |
| 460 | Retain | No material issue identified in this review. |
| 470 | Retain | No material issue identified in this review. |
| 480 | Merge candidate | Absorb OpenAPI subset from 1010; retain dedicated-tool scope. |
| 490 | Revise | Remove unused registrations, but distinguish assigned reachable tools. |
| 1310 | Retain | No material issue identified in this review. |
| 1330 | Retain | No material issue identified in this review. |
| 1340 | Retain | No material issue identified in this review. |
| 1360 | Revise | US region and GA stage do not imply residency guarantee. |
| 1370 | Revise | Verify CMEK/VPC-SC configuration before automatic KB creation. |
| 1380 | Revise | Clarify PHI policy in light of absent RAG data-residency guarantee. |
| **Deployment & Channel Security** | | |
| 500 | Revise | Update channel inventory for WhatsApp and Instagram; remove fixed count. |
| 510 | Retain | No material issue identified in this review. |
| 520 | Revise | Drop authentication mappings for anonymous origin/reCAPTCHA controls. |
| 530 | Revise | Use canonical ces.tools.execute and session permission IDs. |
| 540 | Revise | Drop person-authentication mapping if the bounded agent remains anonymous. |
| 550 | Retain | No material issue identified in this review. |
| 560 | Retain | No material issue identified in this review. |
| 570 | Retain | No material issue identified in this review. |
| 580 | Revise | Include Meta and all third-party channel processors in review. |
| 590 | Revise | Assess inbound A2A/API paths against gateway requirement. |
| 600 | Revise | Clarify disclosure timing; remove HIPAA recording-consent mapping. |
| 1300 | Retain | No material issue identified in this review. |
| **Agent Assist & Human Agent Support** | | |
| 610 | Retain | No material issue identified in this review. |
| 620 | Retain | No material issue identified in this review. |
| 630 | Revise | Conversation TTL maps to SI-12, not audit-log retention AU-11. |
| 640 | Retain | No material issue identified in this review. |
| 650 | Retain | No material issue identified in this review. |
| 660 | Retain | No material issue identified in this review. |
| 670 | Retain | No material issue identified in this review. |
| 680 | Retain | No material issue identified in this review. |
| 690 | Merge candidate | Combine workforce-monitoring approval with 820. |
| 1320 | Merge candidate | Preview prohibition already in 10; preserve feature test if needed. |
| 1350 | Merge candidate | Preview prohibition already in 10; preserve feature test if needed. |
| **Conversation Analytics & Insights** | | |
| 700 | Retain | No material issue identified in this review. |
| 710 | Revise | Conversation TTL maps to SI-12, not audit-log retention AU-11. |
| 720 | Retain | No material issue identified in this review. |
| 730 | Retain | No material issue identified in this review. |
| 740 | Revise | Align synthetic/non-PHI Preview evaluation exception with scope of 10. |
| 750 | Retain | No material issue identified in this review. |
| 760 | Retain | No material issue identified in this review. |
| 770 | Retain | No material issue identified in this review. |
| 780 | Retain | No material issue identified in this review. |
| 790 | Retain | No material issue identified in this review. |
| 800 | Retain | No material issue identified in this review. |
| 810 | Revise | Drop HIPAA 164.514(a) unless de-identification is required. |
| 820 | Merge candidate | Same cross-component workforce approval as 690. |
| 830 | Retain | No material issue identified in this review. |
| **Data Protection & Privacy** | | |
| 840 | Retain | No material issue identified in this review. |
| 850 | Revise | Narrow to monitoring and multi-party authorization or merge with 860. |
| 860 | Merge candidate | Retain hard key-lifetime test if merged with 850. |
| 870 | Retain | No material issue identified in this review. |
| 880 | Retain | No material issue identified in this review. |
| 890 | Revise | Drop Privacy Rule documentation mapping 164.530(j) for a TTL setting. |
| 900 | Revise | Drop Privacy Rule documentation mapping 164.530(j) for logging choice. |
| 910 | Retain | No material issue identified in this review. |
| 920 | Retain | No material issue identified in this review. |
| 930 | Retain | No material issue identified in this review. |
| 940 | Revise | BA contract provision is not a direct deletion-procedure mapping. |
| 950 | Merge candidate | Move transient-cache detail into data-flow requirement 1220. |
| 960 | Retain | No material issue identified in this review. |
| **Network & Perimeter Security** | | |
| 970 | Retain | No material issue identified in this review. |
| 980 | Revise | Avoid precise operation counts in note; retain sequencing rule. |
| 990 | Retain | No material issue identified in this review. |
| 1000 | Retain | No material issue identified in this review. |
| 1010 | Merge candidate | Duplicate OpenAPI subset of 480. |
| 1020 | Retain | No material issue identified in this review. |
| 1030 | Retain | No material issue identified in this review. |
| 1040 | Retain | No material issue identified in this review. |
| 1050 | Retain | No material issue identified in this review. |
| 1060 | Retain | No material issue identified in this review. |
| **Monitoring & Analytics** | | |
| 1070 | Retain | Separate API audit-log test is useful; may consolidate with 1080. |
| 1080 | Retain | Separate API audit-log test is useful; may consolidate with 1070. |
| 1090 | Retain | No material issue identified in this review. |
| 1100 | Revise | Separate organizational six-year policy from literal HIPAA text. |
| 1110 | Retain | No material issue identified in this review. |
| 1120 | Retain | No material issue identified in this review. |
| 1130 | Retain | No material issue identified in this review. |
| 1140 | Retain | No material issue identified in this review. |
| 1150 | Revise | Drop AC-6(7), which reviews privileges rather than anomalous reads. |
| 1160 | Retain | No material issue identified in this review. |
| 1170 | Revise | Drop HIPAA 164.410 mapping to internal incident procedure. |
| **Compliance & Certification** | | |
| 1180 | Merge candidate | Use as primary BAA scope rule; fold in 1190/1200. |
| 1190 | Merge candidate | Umbrella covered-product naming defeats name-only exclusion test. |
| 1200 | Merge candidate | Require written clarification for ambiguous API mappings only. |
| 1210 | Retain | No material issue identified in this review. |
| 1220 | Revise | Include A2A and transient caches in full data-flow inventory. |
| 1230 | Retain | No material issue identified in this review. |
| 1240 | Revise | Add a source specific to deployed agent inventory/reconciliation. |
| 1250 | Merge candidate | Transition trigger belongs with monthly release review in 70. |
