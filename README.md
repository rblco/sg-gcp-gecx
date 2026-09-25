---
content-type:
  - Readme
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-24
tags:
  - scg
  - gcp
---

# Gemini Enterprise for CX - Security Configuration Guide

This project holds the Security Configuration Guide (SCG) for Google Cloud's Gemini Enterprise for CX suite: CX Agent Studio, Agent Assist, CX Insights, and Commerce agents. The deliverable is a single CSV of prescriptive security requirements used as the input to capability certification, GRC control mapping, and policy and monitoring enrollment.

## The deliverable

`_src/geminiCustomerExperienceGuidance.csv` is the guide. Everything else in this folder exists to support, explain, or evidence it. There is exactly one CSV, and there will only ever be one - a second variant would drift from the first the moment either was revised, and a reviewer would have no way to tell which one governs certification.

The current draft holds **141 requirements across 12 categories**. It was authored 2026-09-04, amended the same day following a deep review, and updated on 2026-09-23 against the release notes, the three documentation gaps left open on 2026-09-04, and the restored HITRUST CSF v11 mapping requirement, then deep-reviewed and corrected the same day. On 2026-09-24 an external audit was verified and applied, together with Google's 2026-09-24 release, adding IDs 1390, 1400, and 1410. Forty-eight rows are at Revision 2, seventy-seven at Revision 1, and sixteen at Revision 0 (nine of them new this cycle); each changed row was incremented once for the cycle, which has not yet been published. The review and remediation are recorded in `notes/SCG-review-2026-09-04.md` and `notes/SCG-remediation-2026-09-04.md`; the 2026-09-23 update is recorded in `notes/SCG-update-2026-09-23.md`, its deep review in `notes/SCG-review-2026-09-23.md`, and the review fixes in `notes/SCG-remediation-2026-09-23.md`; the 2026-09-24 audit fixes are recorded in `notes/SCG-remediation-2026-09-24.md`, with the audit itself in `notes/Gemini-Ent-CX-GPT-Audit.md`; every row's change across the cycle is traced in `_src/geminiCustomerExperienceGuidance-diff-map-2026-09-23.csv`.

## How to read a row

| Column | What it holds |
| --- | --- |
| ID | Stable identifier. Flat and append-only across the whole file in multiples of ten, with no relationship to the category a row sits under. A new requirement takes the highest existing ID plus ten wherever it belongs, so the file does not read in ascending order once amendments begin - IDs 1260 to 1380 sit in earlier categories. Never renumbered: downstream GRC mapping, policy linkage, and audit evidence all key on this number. |
| Revision | `0` on initial authoring; incremented whenever the substance of that row changes. |
| Requirement | One normative sentence carrying exactly one bolded directive: **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, **MAY**, or **MAY NOT**. |
| Rationale | Why the control exists - the specific threat, failure mode, or compliance driver, not a general appeal to good practice. |
| Risk / Cost | `High`, `Med`, or `Low`. Risk is the consequence of the control being absent; Cost combines implementation, engineering, and ongoing operations. |
| Notes | Implementation guidance, sequencing, exception paths, and the caveats that would clutter the requirement sentence. Where a fact could not be established from vendor documentation, the Notes say so. |
| References | Semicolon-separated authoritative URLs. Vendor primary documentation, vendor compliance pages, and standards bodies only. |
| Mappings | `HIPAA: ...; NIST 800-53: ...; HITRUST CSF v11: ...; OWASP ...; MITRE ATLAS: ...`. NIST 800-53 Rev. 5 appears on every row. HITRUST CSF v11 appears on every row whose control touches access control, data protection or privacy, audit logging or monitoring, or transmission security (114 of 141); its titles have not been verified against the organization's licensed v11 catalog. The others appear where they genuinely apply. |

IAM roles and permissions are written as their IAM identifiers - `roles/ces.admin`, `ces.tools.execute` - which is the form a binding or custom role takes. Google's CX Agent Studio access-control page prints the same roles as `ces.googleapis.com/admin`, which is not the role ID that IAM documents for bindings.

Eight columns - Data_Levels, Audit Procedures, grcRationale, Prisma Policies, Sentinel Modules, CSP Controls, Monitoring, and full_id_field - are deliberately empty. A separate downstream process owns them. Do not populate them here.

## What the directives mean

**MUST** and **MUST NOT** are hard requirements; an exception needs CISO-level approval with documented risk acceptance. **SHOULD** and **SHOULD NOT** are strong recommendations; an exception needs documented risk acceptance at the architect level. **MAY** and **MAY NOT** are advisory, and deviating is a design choice rather than a risk acceptance.

## Reading order for someone new to this product

The three findings that shape most of the guide are worth understanding before reading the requirements individually, because roughly a third of the rows are consequences of one of them.

First, **tools execute as the platform rather than as the caller**. Google states on the OpenAPI tool page that actions are executed using the permissions granted to the CX Agent Studio service account, not the direct permissions of the end user, and MCP tools carry the same authentication options. Since that service account is one identity per project, the project - not the agent - is the real security boundary. Start at ID 350.

Second, **several decisions are irreversible**. The CX Insights encryption specification cannot be changed once a location is enabled; CX Agent Studio cannot be CMEK-integrated retroactively; a project-level conversation TTL does not apply to conversations already ingested; and a perimeter enforced after resources exist produces seven simultaneous documented breakages. Start at IDs 700, 710, 840, and 980.

Third, **the analytics surface has no supported per-conversation boundary**, because fine-grained access control is Pre-GA. Every approved CX Insights user sees every conversation in the project. Start at IDs 50 and 730.

A fourth, added after review: **tools execute as one identity and nothing carries attribution across the boundary**. Because every tool call reaches the backend as the same service agent, the backend's own log cannot tell one agent, conversation, or member from another. ID 1310 requires a correlation identifier to close that, and it is what makes the residual risk at ID 1230 investigable rather than merely accepted.

## This is a standalone guide

No requirement in this file references another requirement, in this guide or in any other. Each row stands on its own so that it can be lifted into a control matrix, assigned to an owner, or assessed in isolation without carrying a dependency chain. Where two controls genuinely interact - a perimeter that must exist before a resource, a key that must be initialized before ingestion - the Notes column describes the interaction substantively rather than pointing at an ID.

The prompt named Gemini Enterprise Agent Platform as a dependency. Google's documentation asserts no hierarchical or runtime relationship between the two products, and the analysis behind treating this guide as standalone is recorded in `prompt-log.md`.

## Maintaining this guide

Review monthly against three independent release-note streams - CX Agent Studio, CX Insights, and Agent Assist - plus the suite overview page and the CX Agent Studio tools overview, which have no release notes but are where new components and tool types first appear. This suite ships features continuously and they arrive enabled and visible in the console rather than behind an opt-in. The 2026-09-23 update found the File search and Widget tool types in the tools overview with no corresponding release note.

When a requirement changes substantively, edit it in place and increment its Revision. When a requirement is retired, delete the row and leave the gap - a gap is the record of a retirement, not a defect to be closed. When a requirement is added, give it the highest existing ID plus ten regardless of which category it belongs to. Never renumber.

Pay particular attention to rows whose basis is a launch stage rather than a documented behaviour, because a launch stage changes without a release note more often than a behaviour does. When a Preview capability reaches general availability, the prohibition, the compensating control that stood in for it, and the residual risk accepted because of it all change together.

## Folder layout

```
Gemini-Ent-CX/
  _src/geminiCustomerExperienceGuidance.csv   the deliverable
  _src/geminiCustomerExperienceGuidance.md    rendered guidance document (organization template), with PDF
  _Gemini Enterprise for CX.md                overview, certification posture, workstream
  README.md                                   this file
  resources.md                                curated sources and what each establishes
  references.md                               flat citation list
  prompt-log.md                               generation history and confirmed inputs
  _src/DocGen.json                            publishing metadata
  _src/*-diff-map-<date>.csv                  row-by-row change trace per update
  notes/                                      source log, validation, review, remediation
  _src/archive/                               pre-change snapshots of the CSV
  output/                                     session records
  templates/                                  snapshot of the CSV template
  _corres/  _img/  docs/  instructions/       supporting material
```

## Related

- [[_Security Configuration Guidance]]
- [[Capability Certification - Security Guidance]]
- [[CONTRIBUTING to guidance]]
