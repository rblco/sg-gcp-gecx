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

# Session output - Gemini Enterprise for CX SCG

Session date: 2026-09-04. Skill: `scg-generator`. Author: R. Lucier. Model: Claude Opus 5 (1M context).

## Summary of findings

Gemini Enterprise for CX is a four-component agentic customer experience suite - CX Agent Studio (GA February 4, 2026), Agent Assist, CX Insights, and Commerce agents (unreleased). Three findings shaped roughly a third of the requirements.

The first is a confused-deputy condition that Google documents plainly and that has no in-product remedy: actions performed by an OpenAPI tool execute using the permissions granted to the CX Agent Studio service account rather than those of the end user, MCP tools share that authentication model, and the service account is one identity per project. The effective reach of any agent is therefore the union of every backend any tool in its project authenticates to. Paired with the Google-hosted token broker, which exists to allow public access and offers origin checks and reCAPTCHA as optional recommendations rather than defaults, the worst plausible configuration is an unauthenticated caller reaching that entire union. The project boundary is consequently the only real security boundary the product offers, which is why the dedicated-project requirement is the guide's single High-cost row.

The second is that several decisions close permanently. The CX Insights encryption specification cannot be changed after a location is enabled for CMEK; CX Agent Studio cannot be CMEK-integrated retroactively; a project-level conversation TTL does not apply to conversations already ingested, so pilot data persists indefinitely; key rotation is supported but data re-encryption is not, which binds key version lifetime to data retention; and a perimeter enforced after resources exist produces seven documented simultaneous breakages. None of these is a toggle - all are sequencing, and all are cheap to get right and expensive to correct.

The third is that the analytics surface has no supported per-conversation access boundary, because CX Insights fine-grained access control is Pre-GA. Every approved user sees every conversation in the project. The guide states that consequence and compensates with a small named group plus Data Access audit logging, rather than implying a boundary that does not exist.

A fourth observation worth recording: Google's HIPAA covered products list names "Gemini Enterprise for Customer Experience (GECX)", "Customer Experience Agent Studio", "CX Insights", "Contact Center AI Agent Assist", and "Conversational Agents" individually, while "Dialogflow" by that name is absent. Because CX Insights accepts ingestion from Dialogflow, a supported integration path can carry conversations outside BAA coverage without a deliberate decision.

## Deliverable

One CSV: `_src/geminiCustomerExperienceGuidance.csv`. 125 requirements across 11 categories, IDs 10 through 1250 flat and append-only, all at Revision 0.

| Category | Requirements |
| --- | --- |
| General | 9 |
| Identity & Access Management | 13 |
| Agents & Agent Management | 12 |
| Tools & External Integrations | 15 |
| Deployment & Channel Security | 11 |
| Agent Assist & Human Agent Support | 9 |
| Conversation Analytics & Insights | 14 |
| Data Protection & Privacy | 13 |
| Network & Perimeter Security | 10 |
| Monitoring & Analytics | 11 |
| Compliance & Certification | 8 |

Supporting artifacts produced: `_Gemini Enterprise for CX.md`, `README.md`, `resources.md`, `references.md`, `prompt-log.md`, `DocGen.json`, `notes/external-source-log.md`, `notes/SCG-validation-2026-09-04.md`, and this file.

## Validation

All 22 structural and content checks pass. Every NIST SP 800-53 identifier and title (71 distinct controls) was verified programmatically against the Rev. 5 OSCAL catalog rather than carried forward from another guide; `SA-12` was confirmed withdrawn and excluded. All 34 distinct cited URLs returned HTTP 200. The file is pure ASCII. Markdown line discipline is clean across all seven companion files.

## Recommendations

Close the five verification items in the validation report before certification sign-off, and prioritise two of them. Verify the Python code tool execution model with Google directly - it is the only tool type that runs arbitrary code in the conversation path, its documentation page returns 404 at every URL tried, and the guide currently constrains it by code review rather than by any documented technical boundary. Then confirm whether data store retrieval is access-control aware; requirement ID 390 takes the conservative reading of documented silence, and a definitive answer either closes a real exposure or permits relaxing a High-risk requirement.

Beyond that, obtain Google's model-training position contractually across all four components rather than relying on a sentence scoped to one toggle on one page, and read the four Agent Assist pages that rendered as navigation only.

One structural recommendation for the library: three guides now carry the pre-2026-09 column order in which Notes, References, and Mappings sit at positions 7 to 9 - Azure Bot Service, Conversational Analytics API, and the archived Gemini Enterprise CX draft. This guide uses the canonical order. Reconciling those three would remove the ambiguity that `references/csv-columns.md` currently has to explain in prose.

## Errors in processing

No tool errors or failed writes. Six vendor documentation pages could not be read: four Agent Assist pages (CMEK, regionalization, data redaction and retention, release notes) returned HTTP 200 with navigation only across repeated attempts, and two CX Agent Studio tool pages (Python code tools, client function tools) returned HTTP 404 at every URL tried. These are recorded as warnings in the validation report and as explicit limitations in the Notes of the four affected requirements rather than being papered over.

Two self-corrections during authoring, both caught by automated checks before the deliverable was written. Twenty-six requirements were initially drafted with two bolded directives, which violates the single-directive rule; each was rewritten to one normative statement with the demoted clause moved into Notes. Separately, thirty ID references in the overview document were initially estimated rather than looked up and were wrong; they were regenerated from the emitted CSV and re-verified individually.

## Skill defects observed

**1. Contradiction on requirement cross-referencing.** `SKILL.md` states under "Do not" that a standalone SCG must not cross-reference requirements, while `references/requirement-authoring.md` instructs the author to call out interdependencies in Notes and illustrates this with ID references. These cannot both be followed. This session took `SKILL.md` as authoritative and described interdependencies substantively without naming IDs. The two files should be reconciled - the useful resolution is probably to say that interdependencies are described by control rather than by ID in a standalone guide.

**2. `full_id_field` prefix is wrong in Step 7.** `SKILL.md` gives the format as `SC-<cloud_platform>-<product_name_abbreviation>` with three worked examples all using `SC-`. Every DocGen.json in the library uses `SG-`, as does the `idPrefix` placeholder in `_scg-scaffold/templates/docgen.json` and the example in `references/csv-columns.md`. This session used `SG-GCP-GECX` to match the library. The skill text should be corrected to `SG-`.

**3. Scaffolder subfolder names diverge from the documented layout.** `scaffold_project.py` creates `img/` and no correspondence folder, while `SKILL.md` documents `_img/` and `corres/` and the existing library folders use `_img/` and `_corres/`. This session renamed and created them manually after scaffolding.

**4. Scaffolder slug and folder derivation do not honour supplied parameters.** For "Gemini Enterprise for CX" the script derives the slug `geminiEnterpriseForCx` and the folder `Gemini-Enterprise-for-CX`, ignoring the caller-supplied `product_name` and the library's abbreviation convention. Both required manual correction. The script could accept `--slug` and `--folder` overrides.

**5. `z-archive` guidance conflicts with the no-clobber principle.** `SKILL.md` says to disregard `z-archive`, but a complete prior guide for this exact product was sitting there, and silently ignoring it would have discarded ID continuity that downstream GRC mapping keys on. This session surfaced the conflict to the user rather than resolving it unilaterally. The skill should say to surface an archived guide for the same product rather than to disregard it.

## Deviations from the request

One, and it was confirmed with the user rather than taken unilaterally. The request supplied `dependency: Gemini Enterprise Agent Platform`, which under the skill's dependency path would produce a component-scoped guide deferring inherited controls to the parent SCG. Google's documentation asserts no hierarchical or runtime relationship between the two products: Gemini Enterprise for CX exposes its own API services, its own ten-role IAM family, its own per-component CMEK initialization, its own residency model, and its own documentation tree, and Google's HIPAA covered products list names them as siblings. The user was presented with this analysis and chose standalone scoping. The dependency is recorded in `prompt-log.md` and in this file rather than being silently dropped.

Two decisions were also confirmed rather than assumed: to build fresh and leave the archived draft untouched, and to author against the eleven-category set. Both are recorded in `prompt-log.md`.

The folder was named `Gemini-Ent-CX` rather than the scaffolder default `Gemini-Enterprise-for-CX`, to match the sibling convention already in `_GCP/`. This is a routine judgement call, noted for completeness.

## Session metrics

- Wall-clock processing time: approximately 40 minutes.
- Tokens consumed: approximately 300,000 of the 15,000,000 available in the session budget, leaving roughly 14.70M. Exact billing figures are not exposed to the model, so this is a read of the session counter rather than an invoice.
- Cost: not exposed to the model; cannot be reported accurately and is deliberately not estimated here.
- Web retrievals: 24 page fetches and 6 searches against vendor and standards sources, plus one 10.4 MB download of the NIST OSCAL catalog for citation verification and 34 HTTP status checks.
- Completed: 2026-09-04.
