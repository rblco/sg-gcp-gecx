---
content-type:
  - Resource-Doc
subject: Gemini Enterprise for CX
cloud: GCP
date: 2026-09-04
tags:
  - scg
  - gcp
---

# Resources

`src`: [Gemini Enterprise for CX](https://docs.cloud.google.com/gemini-enterprise-cx)

> Curated authoritative sources used to ground the SCG CSV in `_src/geminiCustomerExperienceGuidance.csv`.
> Every external page consulted, with its retrieval date and what it established, is logged separately in [[30-ISA.WORK/20-CapCert-SecurityGuidance/10-GCP/Gemini-Ent-CX/notes/external-source-log]].

<!--
CITATION RULE: vendor primary documentation, vendor compliance and trust pages, vendor release
notes, and standards bodies (NIST, CIS, ISO, OWASP, MITRE) only. Never cite third-party
tutorials, community forum posts, Medium articles, security news sites, or AI-generated summaries.
If a requirement cannot be cited from an authoritative source, note the gap rather than shipping it.

Each entry: markdown link + one sentence on what that source provides. Say what it ESTABLISHES
(the limit, the default, the launch stage, the immutable field), not what it is about, so a later
reviewer can tell which requirement rests on which fact.
-->

## Documentation

### Product core

- [Gemini Enterprise for CX](https://docs.cloud.google.com/gemini-enterprise-cx) - Establishes that the suite comprises exactly four components (Customer Experience Agent Studio, Agent Assist, Customer Experience Insights, Commerce agents) and describes it as an agentic solution bringing shopping and customer service onto a single interface; it makes no claim of being hosted inside, or inheriting controls from, Gemini Enterprise or the Gemini Enterprise Agent Platform.
- [CX Agent Studio](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio) - Establishes that CX Agent Studio is built on the Agent Development Kit, exposes the `google.cloud.ces.v1` API surface with a parallel `v1beta`, and enumerates the sixteen tool types an agent may call.
- [Customer Experience Insights](https://docs.cloud.google.com/gemini-enterprise-cx/insights) - Establishes that CX Insights ingests conversation transcripts and audio, runs ML analysis over them, and exposes the `google.cloud.contactcenterinsights` API with a parallel `v1alpha1`.
- [Agent Assist](https://docs.cloud.google.com/gemini-enterprise-cx/agent-assist) - Establishes the Agent Assist feature set that operates on live conversation content: Knowledge Assist, Generative Knowledge Assist, Summarization, Smart Reply, Live Translation, AI Coach, Sentiment Analysis, and Gemini Transcribe Live.
- [Commerce agents](https://docs.cloud.google.com/gemini-enterprise-cx/commerce-agents) - Establishes that Commerce agents are not released - the page states "Commerce agents coming soon" - and that the intended capability includes "executing consented actions" such as adding items to a shopping cart.

### Identity, access, and tool execution

- [Access control with IAM (CX Agent Studio)](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/access-control) - Establishes the ten-role CX Agent Studio IAM family under the `ces.googleapis.com/` namespace: admin, viewer, client, appEditor, agentEditor, toolsEditor, guardrailsEditor, evalsEditor, securitySettingsEditor, and deploymentEditor.
- [OpenAPI tools](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/open-api) - Establishes the confused-deputy condition at the centre of this guide, stating that "Actions performed by the OpenAPI tool are executed using the permissions granted to the CX Agent Studio service account, not the direct permissions of the end-user"; also establishes that credentials are held in Secret Manager and that Google's own mitigations are dedicated projects, VPC Service Controls, and minimum IAM on the service agent.
- [MCP tools](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/mcp) - Establishes that MCP tools carry "the same authentication options as OpenAPI tools", that only StreamableHttpTransport servers are supported, and that third-party and self-built MCP servers are explicitly contemplated.
- [Data store tools](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/data-store) - Establishes that data store tools ground responses on website and uploaded content, and - by its silence on ACLs, retrieval identity, and per-user result filtering - establishes that document-level permission-aware retrieval is not a documented property of this surface.
- [Tools overview](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool) - Establishes the full tool inventory and flags the Google Maps tool as Preview.
- [Python code tools](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/python) - Retrieved 2026-09-23 (404 at the earlier guessed path). Establishes that Python code tools can make external network requests to public internet endpoints only, with no private network access even where Service Directory is configured; that code can read the full conversation history and session state and modify state; and that code can call other tools defined in the agent application.
- [Python runtime reference](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/reference/python) - Establishes that Python tools and callbacks run in a secure sandbox on Python 3.12, with imports limited to the standard library, Pydantic, numpy, and protobuf, and HTTP calls made through a provided client.
- [Client function tools](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/function) - Retrieved 2026-09-23 (404 at the earlier guessed path). Establishes that client function tools are always executed on the client side, not by the agent, that the server-side session waits for the client's toolResponses, and that the input and output schema is defined in OpenAPI format.
- [File search tools](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/file) - Establishes that File search tools upload files, or connect an existing RAG knowledge base, for retrieval-augmented generation; that a knowledge base is created automatically in a builder-selected location when local files are uploaded; and that RAG Engine bills separately and is region-restricted.
- [Website data store tools](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/website-data-store) and [Cloud Storage data store tools](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/cloud-storage-data-store) - Establish the two data store types that can be created directly within CX Agent Studio.
- [Tool resource REST reference](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/reference/rest/v1/projects.locations.apps.tools) - Establishes the data store tool's engine behavior ("If empty, the search applies to all DataStores associated with the Engine"), the model-chosen filter parameter behavior, the absence of any end-user identity field on data store tools in contrast to the connector tool's end-user authentication support, the client function hand-off, and a remote agent tool type with no page in the tools overview.
- [RAG Engine overview](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/rag-engine/rag-overview) - Establishes that VPC Service Controls and CMEK are supported by RAG Engine while data residency and Access Transparency controls are not, and lists US regions at different launch stages (us-central1 and us-east4 Allowlist, GA; us-east1 Allowlist, Preview).

### Deployment and channels

- [Deploy agent applications](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/deploy) - Establishes the deployment channel set: web widget, AudioCodes, Five9, Google Cloud CCaaS, Google Telephony Platform, Twilio, and direct API access, plus traffic splitting across deployment environments.
- [Web widget](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/deploy/web-widget) - Establishes that the Google-hosted token broker exists specifically "to allow public access to your chat widget", and that origin checks and reCAPTCHA are offered as optional recommendations rather than defaults: "enable public access, and optionally enable origin and reCAPTCHA checks (recommended to prevent spoofing and abuse)".

### Data protection, encryption, and residency

- [CMEK (CX Agent Studio)](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/cmek) - Establishes that "All agent application data-at-rest can be protected with CMEKs", that "Existing resources in non-CMEK integrated projects cannot be CMEK integrated retroactively", that "Key rotation is supported but data re-encryption is not", and that "One key should be used per project location".
- [CMEK (CX Insights)](https://docs.cloud.google.com/gemini-enterprise-cx/insights/cmek) - Establishes the hard immutability rule - "You cannot change encryption key settings for a location after that location has been enabled for CMEK" - names the `service-PROJECT_NUMBER@gcp-sa-ccai-cmek.iam.gserviceaccount.com` service agent, excludes the `global` location from CMEK, requires separate CMEK configuration in Speech-to-Text and BigQuery, and warns that revoking a key for more than 30 days destroys the data.
- [Regionalization and data residency (CX Agent Studio)](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/reference/region) - Establishes that only two multi-regions exist (`us` and `eu`), that the EU multi-region excludes London and Zurich, that residency covers callback code, tool code and configurations, agent instructions and prompts, application variables, and conversation history, and that Cloud Logging is governed separately.
- [Regionalization (CX Insights)](https://docs.cloud.google.com/gemini-enterprise-cx/insights/regionalization) - Establishes the regional endpoint hostname pattern `LOCATION_ID-contactcenterinsights.googleapis.com`, that both the host and the resource path must be changed to stay regional, and that the global endpoint stores data at rest in the US under the legacy location ID `us-central1`.
- [Conversation history](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/conversation-history) - Establishes the retention model (configurable, "default 1 year, maximum 2 years"), the "Log your customer conversations" toggle that disables long-term conversational storage, the 30-minute session memcache and 1-hour TTS cache TTLs, that Google support may access Google-managed operational data for critical debugging, and Google's statement that content shared through the toggle is "never" used to train Google's production machine learning models.
- [Set a TTL on conversation data (CX Insights)](https://docs.cloud.google.com/contact-center/insights/docs/ttl) - Establishes that without a TTL "If a conversation is not set to expire, it will remain in CX Insights indefinitely", that project-level `conversation_ttl` "only applies to conversations that are created after this TTL is set" and is not retroactive, that expiry deletion lands 24 hours after the expiration time, and that expired conversations "are not recoverable".
- [Integrate conversation data (CX Insights)](https://docs.cloud.google.com/gemini-enterprise-cx/insights/common-integrations) - Establishes the ingestion paths into CX Insights: Google Cloud CCaaS built-in ingestion of call audio and Cloud Storage chat transcripts, telephony providers via SIPREC, Google's telephony platform, Dialogflow, Agent Assist, and direct API import.

### Network and perimeter

- [Configure VPC Service Controls for CX Agent Studio](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/vpc-service-controls) - Establishes that both `ces.googleapis.com` and `contactcenterinsights.googleapis.com` must be restricted, enumerates the seven documented in-perimeter breakages (arbitrary HTTP endpoints from tools and callbacks, Cloud Storage audio, BigQuery export, Sensitive Data Protection redaction templates, OpenAPI secrets, data store tools, and cross-perimeter agent calls and import/export), and establishes the Service Directory private network access path with the `service-agent-project-number@gcp-sa-ces.iam.gserviceaccount.com` service agent requiring `servicedirectory.viewer` and `servicedirectory.pscAuthorizedService`.

### Monitoring and audit

- [Audit logging (CX Agent Studio)](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/audit-logging) - Establishes that Admin Activity logs are on by default while Data Access logs require explicit enablement, and that the log filter service name is `ces.googleapis.com`.
- [Audit logging (CX Insights)](https://docs.cloud.google.com/gemini-enterprise-cx/insights/audit-logging) - Establishes the same split for CX Insights and that the log filter service name is `contactcenterinsights.googleapis.com`.

### Guardrails and safety

- [Guardrails](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/guardrail) - Establishes the four guardrail types (Prompt guard, Blocklist, Safety, Rules), the three safety levels (Relaxed, Balanced, Strict), that "Each agent application is provided with default guardrails, which you can modify to suit your needs", that guardrails cover both model input and model output, and - by not naming a default level - establishes that the shipped default is not documented.

### Analytics access control

- [Best practices for fine-grained access control (CX Insights)](https://docs.cloud.google.com/contact-center/insights/docs/best-practices-for-fine-grained-access-control) - Establishes that fine-grained access control is Pre-GA, carrying the notice "This feature is subject to the 'Pre-GA Offerings Terms' in the General Service Terms section of the Service Specific Terms"; names the `roles/contactcenterinsights.authorizedViewer` and `authorizedEditor` roles against the project-level `viewer` and `editor`; warns that "Granting authorized view set permissions at the project level results in users having access to all authorized view sets" and that "Empty filters or '' allow unrestricted access"; and establishes that authorized views cannot import or edit conversation data.

### Release notes and launch stages

- [CX Agent Studio release notes](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/resources/release-notes) - Establishes the February 4, 2026 general availability of Customer Experience Agent Studio, which lists Start with AI among the launch's capabilities without declaring that feature GA, and the dates of the capabilities layered on since, including traffic splitting and the Confluence, Jira, and SharePoint tools. The 2026-09-24 entry adds supervisor agents, the composite model, WhatsApp and Instagram deployments, the SecureCo deployment option, and A2A Protocol tools, and states that agent as a tool is now generally available.
- [CX Insights release notes](https://docs.cloud.google.com/contact-center/insights/docs/release-notes) - Establishes which CX Insights capabilities remain Preview, notably fine-grained access control (April 23, 2025), conversation datasets, and multiple scorecards, against GA capabilities such as CMEK and sentiment analysis.
- [Agent Assist release notes](https://docs.cloud.google.com/gemini-enterprise-cx/agent-assist/release-notes) - Establishes the 2026-09-01 Preview release of Gemini Transcribe Live, the only release across the three streams between 2026-09-04 and 2026-09-23.
- [Gemini Transcribe Live](https://docs.cloud.google.com/gemini-enterprise-cx/agent-assist/gemini-transcribe-live) - Establishes Preview status under the Pre-GA Offerings Terms, enablement through use_gemini_asr in a conversation profile, and that the feature remaps traffic to the Vertex AI supported global region.

### Added 2026-09-24

- [IAM roles and permissions - ces](https://docs.cloud.google.com/iam/docs/roles-permissions/ces) - Establishes the IAM role IDs (`roles/ces.admin`, `roles/ces.appEditor`, `roles/ces.client`, and the other eight) and permission strings (`ces.tools.execute`, `ces.sessions.runSession`, `ces.securitySettings.update`); that `roles/ces.client` holds `ces.sessions.*` and `ces.tools.execute`; that `ces.apps.update` is held by App Editor and Admin but not by Agent Editor or Tools Editor; and that Admin carries every `contactcenterinsights` permission.
- [SecuritySettings REST resource](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/reference/rest/v1beta/SecuritySettings) - Establishes that project Security Settings hold only the endpoint control policy (allowed origins and enforcement scope) and no logging, redaction, or export settings.
- [Create an agent with Gemini (Start with AI)](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/quick/generate-agent) - Establishes that Start with AI is Preview under the Pre-GA Offerings Terms as of 2026-09-24.
- [A2A Protocol tools](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/tool/a2a-protocol) - Establishes outbound delegation to remote agents and inbound invocation by external applications through the v1 `message:send` endpoint with `roles/ces.client`; that inbound `gecx_a2a_agent_context` metadata is written into the session; the outbound authentication options; and that sessions with other CX Agent Studio applications are stateful. The tools overview and navigation mark it Preview; the page itself carries no banner.
- [WhatsApp and Instagram deployment](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/deploy/whatsapp) - Establishes that only owners of the associated project can create the deployment, and that setup connects Meta business assets without describing Meta's handling of conversation content.
- [Best practices for using service accounts](https://docs.cloud.google.com/iam/docs/best-practices-service-accounts) - Establishes that service accounts have no password and cannot be used for browser-based sign-in, which is why interactive MFA cannot apply to them.
- [NIST AI 100-1](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf) - Establishes GOVERN 1.6, mechanisms to inventory AI systems resourced according to organizational risk priorities.
- [eCFR 29 CFR 2560.503-1](https://www.ecfr.gov/current/title-29/subtitle-B/chapter-XXV/subchapter-G/part-2560/section-2560.503-1) - Establishes the ERISA claims-procedure rule for employee benefit plans; Part 2560 sits in Subchapter G, and the Subchapter F address previously cited redirects there.

## Sites

- [Google Cloud HIPAA Compliance](https://cloud.google.com/security/compliance/hipaa) - Establishes that the covered products list names "Gemini Enterprise for Customer Experience (GECX)", "Customer Experience Agent Studio", "CX Insights", "Contact Center AI Agent Assist", and "Conversational Agents" individually, and that "Dialogflow" by that name is not present.
- [Google Cloud Service Specific Terms](https://cloud.google.com/terms/service-terms) - Establishes the Pre-GA Offerings Terms that govern every Preview capability in this suite: provided as is, no production SLA, changeable or withdrawable without notice, and outside the compliance certifications the organization relies on.
- [Google Cloud HIPAA BAA](https://cloud.google.com/terms/hipaa-baa) - Establishes the contractual instrument that must be executed and scoped before any component of this suite processes ePHI.
- [Gemini Enterprise for Customer Experience](https://cloud.google.com/gemini-enterprise-cx) - Vendor product page; establishes the marketing-level positioning of the suite and its intended contact centre and commerce use cases.

## Books

<!-- No vendor-published handbook, whitepaper, or blueprint specific to Gemini Enterprise for CX was found at the time of authoring. -->

## YouTube / Media

- [Google Cloud press release, January 11, 2026](https://www.googlecloudpresscorner.com/2026-01-11-Google-Cloud-Brings-Shopping-and-Customer-Service-Together-with-Gemini-Enterprise-for-Customer-Experience) - Vendor announcement establishing the NRF 2026 launch and the intended single-interface positioning across shopping and customer service.

## Samples / Examples

<!-- Vendor code samples for the web widget token broker are embedded in the Web widget deployment page rather than published as a standalone repository. -->

## Repositories (GitHub)

<!-- No vendor-maintained repository specific to this suite was identified at the time of authoring. -->

---

# Training & Development

## Online Training Resources

- [Gemini Enterprise for Customer Experience: CX Agent Studio Foundations](https://partner.skills.google/paths/522) - Google Skills for Partners learning path covering CX Agent Studio build and deployment fundamentals.

## Tutorials

- [Use the CX Agent Studio MCP server](https://docs.cloud.google.com/gemini-enterprise-cx/cx-agent-studio/mcp-server) - Vendor walkthrough of connecting an MCP server, useful for validating the authentication path a reviewer needs to check against requirement text.

## Practice quizzes & exams

<!-- No certification track specific to this suite exists at the time of authoring. -->
