---
content-type:
  - Project-Overview
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-24
tags:
  - scg
  - gcp
---

# Gemini Enterprise for CX

Gemini Enterprise for CX is Google Cloud's agentic customer experience suite. It has four components: CX Agent Studio, a low-code builder for conversational agents that reached general availability on February 4, 2026; Agent Assist, which puts generated summaries, suggested replies, live translation, and real-time coaching in front of a human contact center representative; CX Insights, which analyzes conversation transcripts and audio at scale and evaluates representative performance; and Commerce agents, which Google's own page describes only as coming soon. 

We are certifying it because it would place a generative agent in front of members while simultaneously ingesting every conversation the contact center handles into an analytics store. The primary data surface is unstructured member conversation, which in a health plan is among the densest concentrations of protected health information the organization holds, and the suite writes copies of it to at least six places: the Google-managed CX Agent Studio conversation store, CX Insights, Cloud Storage audio, Cloud Logging, a BigQuery export, and any Pub/Sub subscriber.

## Why this capability needs a guide

**Tools execute as the platform, not as the caller.** Google states it plainly on the OpenAPI tool page: actions performed by the OpenAPI tool are executed using the permissions granted to the CX Agent Studio service account, not the direct permissions of the end user. Tools can instead authenticate with a customer service account, OAuth, or an API key, but each of those is still a shared tool identity rather than the member's, and the service agent is one identity per project. The effective reach of an agent is therefore every grant held by the identities its tools use, and the project's service agent grants are shared by every application in the project. Retrieval is not documented as permission-aware either. Combine that with the web widget's Google-hosted token broker, which exists specifically to allow public access and offers origin checks and reCAPTCHA as optional recommendations rather than defaults, and the worst plausible configuration is an unauthenticated caller whose effective reach is every backend the project touches. The project boundary is therefore a security design decision made before the first agent exists, and it is why requirement ID 350 is the only High-cost row in the guide.

**Four decisions close permanently at or before first use, and a fifth is costly to reverse.** The CX Insights encryption specification cannot be changed after a location has been enabled for CMEK. CX Agent Studio projects without CMEK cannot be integrated retroactively; the vendor's remedy is to export and restore into a new project. A project-level conversation TTL is not irreversible - it can be changed, and conversations can always be deleted, including in bulk - but it applies only to conversations created after it is set, so anything ingested during a pilot persists until it is found and deleted explicitly. Key rotation is supported but data re-encryption is not, which means every key version must live as long as the data written under it. And a service perimeter established after resources exist produces seven documented simultaneous breakages - audio writes, BigQuery export, redaction templates, OpenAPI secrets, data store tools, arbitrary HTTP endpoints, and cross-perimeter agent references - at exactly the moment there is pressure to write exceptions instead of relocating resources. None of these is a toggle; all are sequencing.

**The analytics surface has no supported per-conversation boundary.** Fine-grained access control through authorized views is Pre-GA, carrying the Pre-GA Offerings Terms. Because it cannot be the control of record, every approved CX Insights user sees every conversation in the project. The guide states that consequence rather than implying a boundary that does not exist, and compensates with a deliberately small named access group and Data Access audit logging. Google's own warnings sharpen the point: granting authorized view set permissions at the project level results in users having access to all authorized view sets, and an empty filter allows unrestricted access - so the mechanism can appear to be present in an IAM review while granting exactly what it looks like it constrains.

**Model selection is a security decision made through a dropdown.** CX Agent Studio lets a builder set the model for an agent application and override it per sub-agent, and it ships a Preview model in the same picker as the generally available ones. Google states that all models here are considered generally available unless explicitly marked Preview in the interface, and that this launch stage "operates independently of a model's status in other Google Cloud products" - so a model that is GA in Vertex AI may be Preview here, and checking its Vertex AI page reaches a conclusion that does not govern this product. IDs 1260 to 1290 govern the allowlist, the Preview prohibition, the per-sub-agent override, and where the launch stage is read from.

**The documentation gaps have narrowed, and what they revealed is sharper than what was assumed.** The Python code tool and client function tool pages that returned HTTP 404 on 2026-09-04 were at different paths, and were read on 2026-09-23. Python code tools run in a documented sandbox, but they can read the whole conversation history and session state, call any other tool in the agent application - inheriting that tool's authority - and reach the public internet, with no private network path at all (ID 450). Client function tools run on the end user's own client, so their arguments are disclosed to that device (ID 460) and their results are whatever that device returns (ID 1330). The identity under which the Python sandbox runs is still undocumented. Data store retrieval has no documented end-user identity mechanism anywhere in the REST definition, so ID 390 now rests on the schema rather than on silence, and an engine-backed data store tool with no data stores named searches every store on the engine (ID 1340). Separately, Google's statement that customer content is never used to train its production models is scoped to one toggle on one page of one component, so ID 910 converts that into a requirement to obtain the position contractually across all four components rather than relying on a documentation sentence.

**The 2026-09-23 deep review found three failures that the guide would have caused.** It read the audit-method tables, role definitions, and settings pages that the guide did not cite. Several rows placed deployment changes, agent export, and CX Insights mutations in Admin Activity logs; Google records them as Data Access events (DATA_WRITE or ADMIN_READ) that are off unless each sub-type is enabled, so the required alerts would never have fired. IDs 1070 and 1080 now require all three sub-types. Role inheritance was invisible to the IAM rows: the CX Agent Studio viewer role carries the CX Insights viewer role, so every agent builder can read the whole conversation corpus (ID 730 now counts inherited access), and the client role, which includes `tools.execute`, is inherited by every builder role (IDs 160 and 530 now keep it away from members). Finally, Agent Assist runs on `dialogflow.googleapis.com`, which the perimeter omitted (ID 970), and the product's native allowed-origins egress policy was unused (ID 1000). The review and its resolutions are in `notes/SCG-review-2026-09-23.md`.

**File search tools put member content in a different product.** A File search tool, a simpler alternative to data store tools that appeared in the tools overview without a release note, creates a RAG Engine knowledge base in a location the builder selects. Google states that RAG Engine supports VPC Service Controls and CMEK but not data residency or Access Transparency controls, and its US regions sit at different launch stages. None of this guide's CX Agent Studio pinning, encryption, or perimeter settings reach that knowledge base; IDs 1360 to 1380 configure it separately and steer protected health information away from it.

**The 2026-09-24 release opened two boundaries the guide keeps closed.** Agent-to-agent (A2A) Protocol tools let a CX agent delegate to remote agents and let external applications invoke a CX agent directly; inbound callers hold the Client role, which includes `ces.tools.execute`, and can write context straight into the session. The tool is Preview and is prohibited in both directions until it is GA and assessed (ID 1390). WhatsApp and Instagram deployments can only be created by project owners, which the basic-role prohibition forbids, and they route member conversations through Meta outside Google's BAA, so they are not approved (ID 1400). An external audit verified the same day also found that Start with AI, which the guide had treated as GA, still carries the Pre-GA terms on its feature page (ID 320), and that the guide wrote IAM roles in the product page's `ces.googleapis.com/admin` form rather than as `roles/ces.admin`. The audit and its disposition are in `notes/Gemini-Ent-CX-GPT-Audit.md` and `notes/SCG-remediation-2026-09-24.md`.

## Certification posture

| Control | Status | Note |
| --- | --- | --- |
| Network / service perimeter | Available, sequencing-critical | `ces.googleapis.com`, `contactcenterinsights.googleapis.com`, and, where Agent Assist is used, `dialogflow.googleapis.com` must be restricted (ID 970). Tool and callback egress is enforced through the allowed-origins policy with an ALWAYS scope (ID 1000). Enforcement must precede resource creation (ID 980), and seven documented breakages make dry-run validation mandatory first (ID 990). |
| Encryption at rest (CMEK) | Available, three separate initializations, one documented exclusion | CX Agent Studio (ID 840), CX Insights (ID 700), and Agent Assist (ID 610) each initialize independently per project location. Speech-to-Text (ID 770), the Cloud Storage audio bucket (ID 760), and the BigQuery export (ID 780) each need their own. CX Insights and Agent Assist are both immutable once a location is enabled; CX Agent Studio cannot be integrated retroactively. **Smart Reply is excluded from CMEK entirely** (ID 610). |
| Data residency | Available, with several carve-outs | CX Agent Studio offers `us` and `eu` only, with the EU multi-region excluding London and Zurich (ID 870). Cloud Logging is **not** covered and needs separate pinning (ID 880). The CX Insights global endpoint stores in the US under the legacy `us-central1` location ID and supports no CMEK (ID 750). Agent Assist generative features **fall back to US single-region endpoints** when pinned to `us` multi-region, and the AI/ML data location commitment covers US and EU only and does not extend to data in use or in transit (ID 680). The Preview Gemini Transcribe Live capability **remaps live audio to the Vertex AI global region** and is prohibited (ID 1350). |
| Provider access | Available, not on by default | Google support may access Google-managed operational data for critical debugging. Access Approval gates that access and Access Transparency records it; both list the GECX components as supported (ID 960). |
| Audit logging | Available, Data Access off by default | Both `ces.googleapis.com` (ID 1070) and `contactcenterinsights.googleapis.com` (ID 1080) need Data Access logging enabled for all three sub-types (ADMIN_READ, DATA_READ, DATA_WRITE); deployment changes, agent export, and most Insights mutations are recorded nowhere else. Given the absence of a per-conversation access boundary, the Insights log is the primary compensating control. |
| Per-conversation access control | **Not production-eligible** | Fine-grained access control is Pre-GA (ID 50) and must not be applied to PHI (ID 740). Compensated by a small named group - counting access inherited through CX Agent Studio roles - plus Data Access logging (ID 730), with the residual risk formally accepted (ID 1230). |
| Model selection | Available, Preview model in the same picker | Allowlist required (ID 1260), including the composite-v1 voice model, whose sub-models Google may update without notice; Preview models prohibited (ID 1270); per-sub-agent override governed (ID 1280); launch stage read from the CX Agent Studio interface only (ID 1290). |
| Backend attribution of tool calls | Requires a compensating control | Tool calls reach the backend under a shared platform or tool identity, so backend logs cannot distinguish agent, conversation, or member. A correlation identifier, injected through the documented session-context mechanism, is required (ID 1310). |
| Compliance coverage (BAA, SOC 2) | In scope, per component | Google's HIPAA covered products list is flat and names "Gemini Enterprise for Customer Experience (GECX)", "Customer Experience Agent Studio", "CX Insights", "Contact Center AI Agent Assist", "Contact Center AI Platform", and "Conversational Agents". Because the suite itself is an entry, coverage is shown per component by its own entry or by Google's written confirmation (ID 1190). "Dialogflow" is **not** named, yet Agent Assist runs on the Dialogflow API, so every API in the data path must be confirmed in writing as within a covered product (IDs 1180, 1200). Pre-GA offerings must not be used with PHI, per Google's HIPAA guidance. |
| File search tools (RAG Engine knowledge base) | Available, configured outside CX Agent Studio | Location must be a US region at GA for RAG Engine (ID 1360); CMEK and perimeter configured on RAG Engine itself (ID 1370); RAG Engine supports neither data residency nor Access Transparency, so PHI should not be placed there (ID 1380). |
| Agent-to-agent (A2A) Protocol | **Preview, prohibited** | Prohibited for outbound delegation and inbound invocation while Preview (ID 1390). At GA, inbound callers need a role without `ces.tools.execute` and the API gateway (IDs 160, 590), and cross-project delegation chains are reviewed as one surface (ID 330). |
| WhatsApp and Instagram channels | **Not approved** | Creation requires project Owner, which is prohibited (ID 130); Meta carries member conversations outside Google's BAA and needs its own assessment and BAA (ID 580). Prohibited until Google documents a path without Owner (ID 1400). |
| Commerce agents | **Not released** | Google states only that Commerce agents are coming soon, and the product does not appear in the HIPAA covered products list. Prohibited outright (ID 20). |

## WORKSTREAM

- [x] [TASK] Initial SCG draft - 125 requirements across 11 categories
- [x] [TASK] Deep review against authoritative sources - see `notes/SCG-review-2026-09-04.md` (0 Blockers, 10 Major, 21 Minor, 5 Info)
- [x] [TASK] Apply review findings - now 132 requirements across 12 categories, 53 rows revised; see `notes/SCG-remediation-2026-09-04.md`
- [x] [TASK] Verify the four Agent Assist pages - closed; two of them contradicted the guide and were corrected
- [x] [TASK] Read the Python code tool, client function tool, and data store tool documentation - closed 2026-09-23 (pages had moved to `tool/python` and `tool/function`); IDs 390, 450, 460 revised, IDs 1330 and 1340 added
- [x] [TASK] Release-note review 2026-09-04 to 2026-09-23 - one release (Gemini Transcribe Live, Preview); File search tools found in the tools overview; see `notes/SCG-update-2026-09-23.md`
- [x] [TASK] Add HITRUST CSF v11 mappings (program decision of 2026-09-20) - 111 of 138 rows after the review fixes
- [x] [TASK] Deep review 2026-09-23 - 0 Blocker, 49 Major, 60 Minor, 20 Info; see `notes/SCG-review-2026-09-23.md`
- [x] [TASK] Apply the 2026-09-23 review findings - 121 rows revised, no ID retired; see `notes/SCG-remediation-2026-09-23.md`
- [x] [TASK] Render the guidance document - `_src/geminiCustomerExperienceGuidance.md`
- [x] [TASK] Verify and apply the external (ChatGPT) audit and the 2026-09-24 release - 41 rows revised, IDs 1390, 1400, 1410 added, no ID retired; see `notes/SCG-remediation-2026-09-24.md`
- [ ] [TASK] Verify HITRUST CSF v11 references and titles against the organization's licensed catalog - none verified
- [ ] [TASK] Confirm with Google the identity under which the Python code tool sandbox runs and whether its HTTP client attaches a Google credential (ID 450)
- [ ] [TASK] Decide whether IDs 460 and 1330 extend to Widget tools, which send model-populated data to the client and return the user's selection (see `notes/SCG-update-2026-09-23.md`)
- [ ] [TASK] Confirm with Google that data store retrieval is not access-control aware - ID 390 rests on the REST schema having no identity field, not on a Google statement
- [ ] [TASK] Obtain Google's written confirmation that the Service Specific Terms training restriction covers every GECX component and data path (ID 910)
- [ ] [TASK] Obtain Google's written confirmation that every API in the data path, including the Dialogflow API used by Agent Assist, is within a covered product (ID 1200)
- [ ] [TASK] Confirm whether ID 1380 (File search tools and PHI) should be MUST NOT rather than SHOULD NOT - review Info item left for the owner
- [ ] [TASK] Obtain Google's statement of the launch stage of the `v1beta` CX Agent Studio and `v1alpha1` CX Insights APIs (ID 30)
- [ ] [TASK] Confirm where the OpenAPI and MCP OAuth tool option holds its client secret (ID 370)
- [ ] [TASK] Decide when to migrate OWASP LLM mappings from the 2025 edition to the 2026 edition published 2026-08-03
- [ ] [TASK] Track a WhatsApp and Instagram creation path that does not require project Owner (ID 1400)
- [ ] [TASK] Name the state recording-consent statutes that govern ID 600 with counsel
- [ ] [TASK] Independent human peer review - the 2026-09-23 review used fresh-context reviewer agents, but every artifact in this folder was produced by the same model
- [ ] [TASK] Stakeholder review (data protection, IAM, network, contact center operations, workforce/HR for the representative-evaluation rows)
- [ ] [TASK] CISO sign-off on MUST-level exceptions and accepted residual risk (ID 1230)
- [ ] [TASK] Confirm compliance scope covers every service in the data path, including Speech-to-Text, Cloud Storage, BigQuery, Cloud KMS, Cloud Logging, Secret Manager, and Pub/Sub
- [ ] [TASK] Assess the third-party contact center platform in use (AudioCodes, Five9, Twilio, Genesys, LivePerson) under ID 580
- [ ] [TASK] Downstream population of Data_Levels, Audit Procedures, grcRationale, Prisma Policies, Sentinel Modules, CSP Controls, Monitoring
- [ ] [TASK] Publish to security guidance library

## Primary Resources

- Vendor docs: https://docs.cloud.google.com/gemini-enterprise-cx
- Vendor release notes: https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/resources/release-notes and https://docs.cloud.google.com/contact-center/insights/docs/release-notes
- Vendor terms / pre-GA offering terms: https://cloud.google.com/terms/service-terms
- Vendor trust / compliance portal: https://cloud.google.com/security/compliance/hipaa

## Project Files

- [[30-ISA.WORK/20-CapCert-SecurityGuidance/10-GCP/Gemini-Ent-CX/_src/geminiCustomerExperienceGuidance.csv|Gemini Enterprise for CX SCG (CSV)]] - the deliverable
- [[30-ISA.WORK/20-CapCert-SecurityGuidance/10-GCP/Gemini-Ent-CX/resources]] - curated authoritative sources, with what each one establishes
- [[30-ISA.WORK/20-CapCert-SecurityGuidance/10-GCP/Gemini-Ent-CX/references]] - flat log of every external page cited
- [[prompt-log]] - generation history, confirmed inputs, and the three decisions taken with the user
- [[30-ISA.WORK/20-CapCert-SecurityGuidance/10-GCP/Gemini-Ent-CX/README]] - how to read the CSV, and maintenance expectations
- `notes/external-source-log.md` - every page consulted, what it established, and the pages that established nothing
- `notes/SCG-validation-2026-09-04.md` - validation report (initial draft)
- `notes/SCG-review-2026-09-04.md` - deep review report
- `notes/SCG-remediation-2026-09-04.md` - what the review changed, and what it deliberately did not
- `notes/SCG-update-2026-09-23.md` - the 2026-09-23 update: release notes, closed gaps, HITRUST CSF v11
- `notes/SCG-validation-2026-09-23.md` - validation report for the 2026-09-23 update
- `notes/Gemini-Ent-CX-GPT-Audit.md` - external audit by ChatGPT, 2026-09-24
- `notes/SCG-remediation-2026-09-24.md` - how each audit finding was verified and resolved
- `notes/SCG-validation-2026-09-24.md` - validation report after the audit fixes
- `_src/geminiCustomerExperienceGuidance-diff-map-2026-09-23.csv` - row-by-row change trace for the whole cycle (2026-09-23 and 2026-09-24) against the 2026-09-04 snapshot
- `output/` - session output

## Related Guides

- [[_Security Configuration Guidance]]
- [[Capability Certification - Security Guidance]]
- [[CONTRIBUTING to guidance]]

## Maintenance

Review monthly against the three independent release-note streams (CX Agent Studio, CX Insights, and Agent Assist), the suite overview page, and the CX Agent Studio tools overview, because this suite ships features continuously and they arrive enabled and visible in the console rather than behind an opt-in. The rows most likely to drift silently are those whose basis is a launch stage rather than a documented behaviour: ID 40 rests on the Google Maps tool being Preview, ID 1390 on the A2A Protocol tool being Preview, ID 320 on Start with AI being Preview, ID 50 and ID 730 rest on CX Insights fine-grained access control being Pre-GA, ID 1270 rests on `gemini-3-flash` being the Preview entry in the model picker, ID 1320 rests on Agent Assist Build your own assist being Preview, ID 1350 rests on Gemini Transcribe Live being Preview, ID 1360 rests on the RAG Engine US region launch stages, and ID 20 rests on Commerce agents being unreleased. The model picker is the most volatile of these, because its composition changes without a release note. When any of those reaches general availability, the prohibition, the compensating control that stood in for it, and the accepted residual risk all change together - ID 1250 makes that a requirement rather than a hope. Increment the Revision column on any row whose substance changes; never renumber an ID, because gaps record retirements and downstream GRC mapping keys on the number. Note that IDs 1260 to 1410 were added after the initial draft and sit in earlier categories, so the file no longer reads in ascending order - that is flat append-only numbering working as intended, not a defect to tidy.

A separate, earlier draft of this guide exists at `_GCP/z-archive/Gemini-Ent-CX/`. It was left untouched by decision during this session and its IDs are unrelated to these. Do not merge the two.
