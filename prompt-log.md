---
title: Gemini Enterprise for CX SCG
product: GCP Gemini Enterprise for CX
tags:
  - prompt
---

# Generation history

## 2026-09-04 - Initial draft (scg-generator)

### Prompt as submitted

```prompts
Create a Security Configuration Guide (SCG) named `Gemini Enterprise for CX` for [Gemini Enterprise for CX](https://docs.cloud.google.com/gemini-enterprise-cx) within the specified cloud platform folder (i.e., target parent folder).
**Parameters**:
- product_title: `Gemini Enterprise for CX`
- product_name: `geminiCustomerExperienceGuidance.csv`
- product_name_abbreviation: `GECX`
- cloud_platform: `GCP`
- target_parent_folder: `_GCP`
- author_name: `R. Lucier`
- dependency: `Gemini Enterprise Agent Platform`
```

### Confirmed inputs

| Input | Value | Source |
| --- | --- | --- |
| Product title | Gemini Enterprise for CX | Supplied |
| Product slug | geminiCustomerExperience | Derived from the supplied `product_name` value `geminiCustomerExperienceGuidance.csv` rather than from the scaffolder default `geminiEnterpriseForCx` |
| Deliverable filename | `_src/geminiCustomerExperienceGuidance.csv` | Supplied |
| Product abbreviation | GECX | Supplied; independently corroborated by Google's HIPAA covered-products entry "Gemini Enterprise for Customer Experience (GECX)" |
| Cloud platform | GCP | Supplied |
| Target parent folder | `_GCP/` | Supplied |
| Product folder | `_GCP/Gemini-Ent-CX/` | Chosen to match the sibling naming convention already in `_GCP/` (Gemini-Ent-Agent-Platform, Gemini-Ent-Connector); the scaffolder default was `Gemini-Enterprise-for-CX` |
| Author | R. Lucier | Supplied |
| Root authoritative doc URL | https://docs.cloud.google.com/gemini-enterprise-cx | Supplied |
| Mapping frameworks | NIST 800-53 Rev. 5 (every row), HIPAA (rows touching ePHI), OWASP LLM Top 10 2025 and OWASP API Top 10 2023 (agent, tool, and API-layer rows), MITRE ATLAS (prompt injection, tool invocation, and tool poisoning rows) | Skill default |
| Organization context | None named in the prompt; requirements are written organization-neutral | Prompt |
| Standalone or component | Standalone - see the dependency analysis below | User decision, 2026-09-04 |

### Decisions confirmed with the user during this session

Three decisions were put to the user before authoring, because each would have produced materially different work.

**Prior work in `z-archive`.** A complete prior SCG for this exact product was found at `_GCP/z-archive/Gemini-Ent-CX/` - 114 requirements across 12 categories, dated 2026-09-03, with its own resources, references, validation report, and session output. Its CSV used the earlier column order in which Notes, References, and Mappings sit at positions 7 to 9 rather than Data_Levels and Audit Procedures at 7 and 8, and `references/csv-columns.md` names this guide specifically as one of three that diverge and require reconciliation. The user was offered three paths: promote and reconcile the archived project preserving its IDs, build fresh and leave the archive untouched, or build fresh while reusing the archived research. **The user chose to build fresh with the archive untouched.** No content from the archived requirements, resources, references, or notes was read into this draft; the archived folder remains exactly as it was. The consequence to record is that the IDs in this guide do not correspond to the IDs in the archived one, and the two are independent documents.

**Dependency scoping.** The prompt named `Gemini Enterprise Agent Platform` as a dependency. Google's Gemini Enterprise for CX documentation asserts no hierarchical or runtime relationship to that platform: the suite exposes its own API services (`ces.googleapis.com` and `contactcenterinsights.googleapis.com`) rather than the Agent Platform APIs, its own ten-role IAM family under the `ces.googleapis.com/` namespace, its own per-component CMEK initialization, its own two-multi-region residency model, and its own documentation tree. Google's HIPAA covered products list names "Gemini Enterprise for Customer Experience (GECX)" alongside "Gemini Enterprise Agent Platform" rather than beneath it. The one documented connection is that CX Agent Studio is built on the Agent Development Kit, which is a shared building block rather than a control-inheriting dependency. **The user chose standalone scoping with the relationship recorded here.** Accordingly, no requirement in this guide cross-references another requirement or another SCG, in this guide or elsewhere, and each row stands on its own.

**Category set.** Eleven categories were proposed and **confirmed by the user** before any requirement was authored: General, Identity & Access Management, Agents & Agent Management, Tools & External Integrations, Deployment & Channel Security, Agent Assist & Human Agent Support, Conversation Analytics & Insights, Data Protection & Privacy, Network & Perimeter Security, Monitoring & Analytics, Compliance & Certification. Four of these are product-specific rather than drawn from the catalog - Tools & External Integrations, Deployment & Channel Security, Agent Assist & Human Agent Support, and Conversation Analytics & Insights - and they exist because CX Agent Studio's sixteen tool types, its seven deployment channels, Agent Assist, and CX Insights each present a distinct attack surface that would have been diluted inside a general heading. A twelfth category, Availability & Resource Management, was offered and not taken.

### ID numbering

Flat and append-only across the whole file, 10 through 1250 in document order, with no relationship between an ID and the category it sits under. There are no gaps, because this is an initial draft and no requirement has yet been retired. All 125 rows carry Revision 0.

### Verification performed during authoring

Every NIST SP 800-53 control identifier and title cited in the Mappings column was checked programmatically against the Rev. 5 OSCAL catalog rather than carried forward from another guide; 71 distinct controls were verified, and SA-12 was confirmed withdrawn and excluded. All 34 distinct URLs cited in the References column were HTTP-checked and returned 200.

### Known gaps carried into the deliverable

Four Agent Assist documentation pages and two CX Agent Studio tool pages could not be read during this session - the Agent Assist CMEK, regionalization, data redaction and retention, and release notes pages rendered as navigation only, and the Python code tool and client function tool pages returned HTTP 404. The requirements that touch those surfaces state the limitation in their own Notes rather than asserting a fact this session could not establish, and each is recorded as a verification item in `notes/external-source-log.md` and in the validation report.

## 2026-09-04 - Deep review and remediation (scg-reviewer, then in-place edit)

### Prompts as submitted

```prompts
Review and QA the geCustomerExperienceGuidance.csv security configuration guide
```

```prompts
yes, please apply the fixes to the CSV
```

### File resolution

The filename in the request, `geCustomerExperienceGuidance.csv`, does not exist on disk. It is the `csvFile` value still declared in the archived draft's `DocGen.json` at `_GCP/z-archive/Gemini-Ent-CX/`, whose CSV was renamed to `geminiCustomerExperienceGuidance.csv` on 2026-09-03. Two files now carry that name - the archived draft and the current one. The user was shown both with their row counts, ID ranges, and column order, and **chose the current draft** at `_GCP/Gemini-Ent-CX/_src/`. Review depth `deep` was explicitly confirmed, as the skill requires before fetching every reference URL.

### Review outcome

0 Blockers, 10 Major, 21 Minor, 5 Info. Report at `notes/SCG-review-2026-09-04.md`. The guide passed every structural and citation-integrity check - canonical header, ID hygiene, pure ASCII, one directive per row, 71 NIST identifiers and titles verified against the Rev. 5 OSCAL catalog, all 34 URLs live with matching titles. The Majors were substantive rather than structural, and most traced to one root cause: four Agent Assist documentation pages that the authoring session recorded as unreadable were readable at review time, and two of them contradicted what the guide said.

The single most consequential finding was a factual error at ID 630, which stated the Agent Assist retention default as one year with a two-year maximum. Google documents the default and maximum as 30 days; the one-year and two-year figures belong to the CX Agent Studio conversation-history page and had been carried onto an Agent Assist row.

### Remediation outcome

All 10 Major and all 21 Minor findings resolved; 2 of 5 Info applied, 3 needed no action. The guide moved from 125 requirements in 11 categories to 132 in 12. Fifty-three rows were revised in place and incremented to Revision 1; seven new rows were added at Revision 0 with IDs 1260 to 1320. No existing ID was renumbered, reused, or removed. The pre-remediation file is preserved at `_src/archive/geminiCustomerExperienceGuidance.PRE-REVIEW-2026-09-04.csv`. Full detail, including what was deliberately not changed, is in `notes/SCG-remediation-2026-09-04.md`.

The new category `## AI Models & Model Management` was added on review finding 2, after confirming that CX Agent Studio exposes a model picker with per-sub-agent override and ships `gemini-3-flash` as a Preview entry alongside generally available models. That category was not in the eleven the user confirmed before authoring, because the authoring session did not establish that the product had a model-selection surface at all.

### Numbering consequence to record

IDs 1260 to 1320 sit in categories that appear earlier in the document, so the file no longer reads in ascending ID order. This is flat append-only numbering behaving as specified - an ID records when a requirement was added, not where it sits - and the ascending-order check used during initial authoring was retired as a result. It encoded the initial-draft state rather than the rule.

## 2026-09-23 - Update (scg-generator skill)

### Prompt as submitted

```prompts
Update the Gemini Enterprise CX (GECX) Security Configuration Guide (SCG):
- product_title: `Gemini Enterprise for CX`
- product_name: `geminiCustomerExperienceGuidance.csv`
- product_folder: `Gemini-Ent-CX`
```

### Confirmed inputs

Inputs unchanged from 2026-09-04 and confirmed by the author: product title Gemini Enterprise for CX; slug `geminiCustomerExperience`; deliverable `_src/geminiCustomerExperienceGuidance.csv`; abbreviation GECX (`SG-GCP-GECX`); cloud platform GCP; product folder `10-GCP/Gemini-Ent-CX/` (the parent folder is now `10-GCP`, previously recorded as `_GCP`); author R. Lucier; root documentation https://docs.cloud.google.com/gemini-enterprise-cx; standalone scoping; the same 12 categories. The mapping frameworks now include HITRUST CSF v11 (restored program-wide on 2026-09-20), in the order HIPAA; NIST 800-53; HITRUST CSF v11; OWASP; others. No organization context.

### Decisions confirmed with the author during this session

**Scope.** All four offered areas: release notes since 2026-09-04, HITRUST CSF v11 mappings on applicable rows, re-verification of every reference, and a retry of the open documentation gaps (Python code tool, client function tool, data store ACL awareness).

**Revision on HITRUST-only rows.** A row whose only change is an added HITRUST CSF v11 segment keeps its Revision; the change is recorded in the diff map as `carried`. Rows that also changed in substance were incremented once.

**Existing folder.** Merged in place rather than re-scaffolded: the folder existed and was non-empty, so the scaffolder was not re-run. `DocGen.json` was moved from the folder root into `_src/`, where the skill places it, and its `lastUpdated` set to 2026-09-23.

### Outcome

138 requirements across 12 categories (was 132). Amended in place: IDs 10, 50, 70, 360, 390, 400, 450, 460, 680, 770. New: IDs 1330 to 1380. HITRUST CSF v11 on 100 rows. Details: `notes/SCG-update-2026-09-23.md`; row trace: `_src/geminiCustomerExperienceGuidance-diff-map-2026-09-23.csv`; validation: `notes/SCG-validation-2026-09-23.md`.

## 2026-09-23 - Deep review (scg-reviewer)

### Prompt as submitted

```prompts
Review and QA the geCustomerExperienceGuidance.csv security configuration guide
```

### File resolution and inputs

As on 2026-09-04, the requested filename does not exist; it is the `csvFile` value in the archived draft's `DocGen.json`. The author chose the current guide at `_src/geminiCustomerExperienceGuidance.csv` (138 requirements), confirmed `deep` depth, and selected a HIPAA plus HITRUST CSF v11 compliance overlay. Structural, NIST OSCAL, HIPAA, URL, and coverage checks were run directly. Per-row factual review was delegated to three independent reviewer agents split by category, and their highest-impact claims were re-verified against the live pages before inclusion.

### Outcome

0 Blocker, 49 Major, 60 Minor, 20 Info. Report at `notes/SCG-review-2026-09-23.md`. The CSV was not edited. The top risks are audit-log type errors that would leave alerts silent (IDs 150, 570, 1050, 1070, 1080), IAM role inheritance that gives every agent builder read access to the CX Insights corpus and would give OAuth members `tools.execute` (IDs 160, 530, 730), and a perimeter that omits the Agent Assist API (ID 970).

## 2026-09-23 - Review fixes and guidance document

### Prompt as submitted

```prompts
Go ahead and apply the fixes. When complete, create a markdown file based on the CSV.
```

### Decisions recorded so they are not re-litigated

**Fixes applied.** Every Major and Minor finding in `notes/SCG-review-2026-09-23.md` was applied, and Info findings where they were a concrete correction or a needed hedge. 121 rows were revised; no ID was added, merged, or retired. The only finding left open is the owner decision on ID 1380. The resolution of every finding is in the review report's Resolutions section, and the detail in `notes/SCG-remediation-2026-09-23.md`.

**Revision numbering.** One increment per recertification cycle over the 2026-09-04 value, following the Gemini RAG Engine precedent of the same day. HITRUST-only additions still do not count as substance.

**Diff map.** One map for the cycle, regenerated against the 2026-09-04 state with both update and review-fix notes per row.

**Guidance markdown.** Rendered to `_src/geminiCustomerExperienceGuidance.md` (the DocGen `guidanceFile`) with the organization's template through `processDocs.py`'s own preprocessing, using the DocGen `title` and a fixed `../../` link prefix as in the RAG Engine and Agent Gateway guides. The template and `processDocs.py` were not modified. A PDF sits beside it.

## 2026-09-24 - External audit analysis and fixes

### Prompts as submitted

```prompts
I had ChatGPT do an audit of the CSV that you generated. Analyze the result: "Gemini-Ent-CX-GPT-Audit.md"
```

```prompts
Move forward based on best judgment.
```

### Analysis

The audit at `notes/Gemini-Ent-CX-GPT-Audit.md` was verified finding by finding against the CSV and, through two independent agents plus direct reads, against Google's live documentation. Of its 13 priority findings, 9 were confirmed in full and 4 in part, and none was wrong. Two depended on Google's 2026-09-24 release (WhatsApp and Instagram deployments; A2A Protocol tools; Agent as a tool GA), which post-dates the 2026-09-23 update. Most consolidation proposals were declined, five findings were already addressed in the CSV, and several mapping corrections were improved on.

### Decisions recorded so they are not re-litigated

The owner delegated the four open decisions. They were taken as follows, with the reasoning in `notes/SCG-remediation-2026-09-24.md`: ID 10 covers production and personal data, with isolated synthetic-data evaluation allowed; WhatsApp and Instagram are not approved (new ID 1400); third-party retrieval content is allowed only as a reviewed snapshot in organization-controlled storage (ID 400); OWASP LLM mappings stay on the 2025 edition for now. The 2026-09-23 no-merge decision was applied, which withdrew two merges proposed earlier in this session. The cycle is still unpublished, so Revision stays at the 2026-09-04 value plus one for every row changed in the cycle.

### Outcome

141 requirements across 12 categories (was 138). 41 rows revised; IDs 1390 (A2A Protocol tools prohibited while Preview), 1400 (WhatsApp and Instagram prohibited), and 1410 (App Editor separated from builder roles, SHOULD) added; no ID retired. Validation passed with 0 errors and 104 of 104 reference URLs live (`notes/SCG-validation-2026-09-24.md`). The cycle diff map carries an audit-fix clause per changed row. The guidance markdown was re-rendered with the organization template, after confirming the render reproduces the previous file byte for byte from the pre-fix CSV, and its PDF regenerated.
