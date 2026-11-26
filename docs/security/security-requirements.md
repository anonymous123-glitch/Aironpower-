# Airon Power — Security Requirements Specification

**Status:** Proposed — awaiting Product Owner and Enterprise Architect review  
**Version:** 1.0  
**Scope:** Requirements-level security specification  
**Repository context:** Integrates with the revised product requirements and Enterprise Architecture review.

## 1. Purpose and governance

This document records the proposed security requirements arising from the AI Security Engineer review. It is an additive specification. It does not replace or silently alter the product requirements.

The reviewed product baseline contains FR-001–FR-048, NFR-001–NFR-040 and BR-001–BR-013. Preserve those identifiers and their existing status. All requirements in this document remain **Proposed** until explicitly approved by the Product Owner and relevant governance owners.

This is a requirements review, not code, architecture, deployment or penetration-test assurance. No implementation, technology selection or Jira ticket creation is authorised by this document.

## 2. Product and security context

Airon Power is a simulated industrial energy information and training platform. It includes customer and staff portals, simulated supply and outage information, maintenance notices, approved-document retrieval for plant staff, and AI-assisted maintenance-ticket drafting and submission workflows.

The product boundary is simulation-only. It does not operate real electricity infrastructure, control plant equipment or provide direct operational technology (OT) control.

Security principles:
- Customer and facility data must be isolated by organisation and authorised facility scope.
- Staff access must be role-limited, purpose-limited and auditable.
- Authorisation is enforced by deterministic server-side application services.
- AI retrieval receives only information the current user is authorised to access.
- User content, uploaded documents, retrieved text and tool outputs are untrusted.
- Only approved, current and authorised documents may be treated as authoritative knowledge.
- AI must not have unrestricted database access or direct plant/OT control.
- Ticket approval must apply to the exact final payload submitted.
- Security controls must not depend solely on model prompts or model behaviour.

## 3. Security review outcome

**Overall assessment: Conditionally ready for security design; not ready for an approved security requirements baseline.**

Existing foundations include customer/staff separation, organisation/facility access concepts, approved knowledge sources, explicit ticket confirmation, simulation provenance, an AI/OT boundary, access revocation and audit history. The review found that enforcement, verification and operational requirements need strengthening.

### 3.1 Principal findings

| Finding | Description | Severity |
|---|---|---|
| SEC-01 | Authentication and account security are underspecified. | High |
| SEC-02 | Role and permission model is incomplete. | Critical |
| SEC-03 | Tenant isolation needs explicit enforcement requirements. | Critical |
| SEC-04 | AI retrieval permissions need stronger requirements. | Critical |
| SEC-05 | Document ingestion and source integrity need controls. | High |
| SEC-06 | AI action permissions and tool boundaries need definition. | Critical |
| SEC-07 | Data protection and encryption are underspecified. | High |
| SEC-08 | Security audit integrity and coverage need strengthening. | High |
| SEC-09 | Secure development and vulnerability management are missing. | High |
| SEC-10 | Incident response and security recovery are underspecified. | High |
| SEC-11 | AI provider and model supply-chain security is missing. | High |
| SEC-12 | Access revocation needs measurable propagation requirements. | High |

## 4. General security non-functional requirements

All requirements in this section have proposed priority **Must**. Numeric targets, retention periods and specific technologies remain TBD until approved.

### SEC-NFR-001 — Authentication assurance
**Related:** FR-001, FR-024, FR-038  
The platform shall implement authentication appropriate to the sensitivity of information and actions, including secure account verification and recovery. MFA shall be required for privileged accounts and considered for other accounts according to risk.

**Acceptance criteria**
- Protected functionality requires authentication.
- Account recovery uses a controlled, verifiable process.
- Authentication errors do not unnecessarily reveal whether an account exists.
- Privileged accounts use MFA.
- Authentication events are recorded.
- Authentication controls are tested against approved verification criteria.
- The authentication design is documented and approved before implementation.

### SEC-NFR-002 — Server-side authorisation
**Related:** FR-002–FR-005, FR-025, FR-026, FR-032, FR-038; NFR-033  
The platform shall enforce authorisation server-side for every protected operation, considering identity, role, organisation membership, facility association, resource ownership and current permissions. Frontend visibility and client-supplied identifiers are not proof of authority.

**Acceptance criteria**
- Every protected API operation performs server-side authorisation.
- Changing organisation, facility, record or resource identifiers cannot grant access.
- Read and write permissions are checked independently.
- Current account and membership status are considered.
- Unauthorised requests are denied.
- Tests cover customer users, customer administrators, staff and privileged administrators.
- The role-permission model is documented and approved.

### SEC-NFR-003 — Privileged account protection
**Related:** FR-024–FR-026, FR-038, FR-040  
Privileged staff and administrator functionality shall be protected through enhanced authentication, least privilege and controlled workflows.

**Acceptance criteria**
- Privileged roles and permissions are explicitly defined.
- Privileged accounts use MFA.
- Sensitive administrative actions are logged.
- Administrative permissions are reviewable.
- Support access to customer information is restricted and auditable.
- Additional approval for high-impact actions is assessed and documented.

### SEC-NFR-004 — Session security
**Related:** FR-001, FR-024, FR-038, FR-046; BR-013  
The platform shall protect authenticated sessions. Sessions shall be invalidated or revalidated when accounts are disabled or relevant permissions change.

**Acceptance criteria**
- Session identifiers are protected against disclosure and reuse.
- Expiry and reauthentication rules are documented.
- Account deactivation invalidates applicable active sessions.
- Permission changes take effect within an approved revocation objective.
- Sensitive operations revalidate current permissions.
- Session controls are security-tested.

### SEC-NFR-005 — Least-privilege service identities
**Related:** NFR-024, NFR-031, NFR-033, NFR-035  
Every application service, background process and integration shall use an identifiable, appropriately restricted service identity.

**Acceptance criteria**
- Security-relevant services have identifiable service identities.
- Permissions are limited to required resources and actions.
- Credentials are not hardcoded in source code.
- Service-to-service authentication is documented.
- Unused permissions can be removed.
- Service identity use is auditable.
- The architecture identifies service access to customer data, documents, AI and ticket workflows.

### SEC-NFR-006 — Encryption and key management
**Related:** NFR-016, NFR-018, NFR-019, NFR-036  
The platform shall protect sensitive information in transit and at rest using standards appropriate to the selected technology and risk assessment. Keys shall be managed through an approved key-management approach.

**Acceptance criteria**
- Sensitive data crossing trust boundaries is encrypted in transit.
- Sensitive stored data is encrypted at rest.
- Protocols and standards are documented.
- Key access is restricted.
- Key rotation, revocation and recovery are addressed.
- Encryption configuration is tested.
- Sensitive data is not exposed through insecure transport or storage.

### SEC-NFR-007 — Secrets management
**Related:** NFR-024, NFR-031  
Credentials, API keys, tokens, certificates and other secrets shall be stored and managed through an approved secrets-management mechanism.

**Acceptance criteria**
- Secrets are not committed to source control.
- Secrets are stored in an approved protected mechanism.
- Access to secrets is restricted.
- Rotation and revocation are supported.
- Secret exposure is detectable through appropriate checks.
- Production and non-production secrets are separated.
- Responsibilities are documented.

### SEC-NFR-008 — API security and abuse protection
**Related:** NFR-001–NFR-010, NFR-027, NFR-032  
All APIs shall enforce authentication, authorisation, input validation and protection against misuse, malformed input, excessive requests and resource exhaustion.

**Acceptance criteria**
- Protected endpoints require authentication.
- Every endpoint enforces server-side authorisation.
- Inputs are validated against expected formats and business rules.
- Errors do not disclose sensitive implementation details.
- Request and resource limits are defined.
- Abuse protection covers AI requests, document operations, authentication and ticket workflows.
- Tests include unauthorised, malformed and excessive requests.

### SEC-NFR-009 — Secure document upload and ingestion
**Related:** FR-028, FR-032, FR-044; NFR-028, NFR-034  
Documents shall pass through controlled ingestion before being available to users or AI retrieval. Uploaded files and extracted content are untrusted until validated and approved.

**Acceptance criteria**
- File type and size restrictions are enforced.
- Content undergoes appropriate validation and malware scanning.
- Document ownership and source are recorded.
- Approval status is established before authoritative use.
- Unapproved documents are excluded from active AI knowledge retrieval.
- Extracted text and embedded instructions are treated as untrusted.
- Failures are handled safely and recorded.
- Quarantine, rejection and withdrawal are supported.

### SEC-NFR-010 — Security audit protection
**Related:** FR-040; NFR-016, NFR-017, NFR-039  
The platform shall maintain protected security audit records for relevant authentication, authorisation, administrative, data-access and sensitive business actions. Records shall be protected against unauthorised alteration and deletion.

**Acceptance criteria**
- Relevant security events are recorded.
- Records identify actor/service, event, timestamp and affected resource where appropriate.
- Privileged activity is auditable.
- Audit access is restricted.
- Unauthorised users and administrators cannot modify or delete protected records.
- Retention and deletion rules are documented.
- Monitoring identifies relevant suspicious activity.
- Logs do not unnecessarily contain passwords, tokens or sensitive document content.

### SEC-NFR-011 — Protection of derived and replicated data
**Related:** FR-032, FR-040, FR-042, FR-044, FR-047; NFR-018, NFR-036  
Sensitive information shall be protected across its lifecycle, including originals, backups, exports, caches, search indexes, vector databases, logs and AI-derived information.

**Acceptance criteria**
- Data classification defines protection requirements.
- Exports are protected by appropriate access controls.
- Cached and indexed data respects source permissions.
- Deletion and retention address derived information.
- Backup access is restricted and audited.
- Handling of withdrawn/deleted information across dependent systems is documented.
- Lifecycle tests cover relevant copies and derived records.

## 5. AI and retrieval security requirements

### SEC-AI-001 — AI must not make access-control decisions
**Related:** FR-029, FR-032, FR-033; NFR-034, NFR-035  
The AI component shall not decide whether a user is authorised to access documents, records, facilities or business functions. Deterministic application-side services make access decisions.

**Acceptance criteria**
- The AI model is not the authority for permissions.
- Application services establish the authorised user context.
- The AI receives only information the user may access.
- Prompts cannot grant additional permissions.
- Tests verify that instructions to ignore permissions do not expose restricted information.

### SEC-AI-002 — Retrieval-time authorisation
**Related:** FR-028, FR-029, FR-032; NFR-033, NFR-034  
AI retrieval shall enforce document, organisation and facility permissions before content is passed to the model.

**Acceptance criteria**
- Retrieval applies current permissions.
- Unauthorised cross-organisation documents are not returned.
- Facility restrictions are enforced.
- Search results, snippets, metadata and content all respect permissions.
- Vector and semantic search enforce equivalent restrictions.
- Caches cannot reuse one user's authorised results for another unauthorised user.
- Background indexing preserves tenant and ownership metadata.
- Tests attempt cross-organisation and cross-facility retrieval.

### SEC-AI-003 — Document provenance and lifecycle enforcement
**Related:** FR-028, FR-030, FR-032, FR-044; NFR-028, NFR-034; BR-008, BR-010  
AI shall use only documents meeting approved-source, current-version and applicable permission requirements. Provenance, approval, version and lifecycle state shall be maintained through ingestion, retrieval and use.

**Acceptance criteria**
- Authoritative documents have identifiable provenance.
- Approval status and version are available to retrieval.
- Unapproved documents are not treated as authoritative.
- Withdrawn documents are excluded from active retrieval.
- Superseded versions are not presented as current.
- Withdrawal/replacement triggers appropriate index and cache invalidation.
- References identify actual retrieved documents.
- Lifecycle events are auditable.

### SEC-AI-004 — Protection against prompt injection and untrusted content
**Related:** FR-029, FR-031, FR-041; NFR-034; BR-007, BR-010  
User input, uploaded documents, retrieved content, external information and tool outputs shall be treated as untrusted data. Embedded instructions cannot override application policies, permissions or approved AI action boundaries.

**Acceptance criteria**
- User input and retrieved documents are treated as untrusted.
- Document instructions cannot change system policy.
- The AI cannot be instructed to disclose restricted information or bypass approval.
- Tests cover malicious instructions embedded in documents and user messages.
- Tests include conflicting documents, misleading content and multiple file formats.
- Unsafe, conflicting or insufficient information results in refusal or escalation.

### SEC-AI-005 — AI output validation
**Related:** FR-029, FR-030, FR-034–FR-036; NFR-029, NFR-030  
AI output shall not automatically be treated as verified information, executable instructions or trusted business data. Outputs shall be validated before display, storage or downstream use.

**Acceptance criteria**
- AI-generated content is handled as untrusted output.
- Structured outputs are validated against approved schemas.
- Generated values cannot bypass business validation.
- Generated content cannot directly execute application commands.
- Citations are checked against actual retrieved sources.
- Invalid or unexpected outputs are handled safely.
- Ticket fields are validated by application services before submission.

### SEC-AI-006 — AI provider and model security
**Related:** FR-029; NFR-018, NFR-024, NFR-026, NFR-027, NFR-036, NFR-040  
AI providers, models and supporting services shall undergo appropriate security, privacy and operational assessment before approval.

**Acceptance criteria**
- Provider data-handling practices are assessed.
- Prompt/document retention is understood and documented.
- Provider use of submitted data for model training is assessed and controlled according to approved policy.
- Processing locations and contractual obligations are assessed.
- Provider authentication and service access are secured.
- Model/provider changes are governed.
- The provider is approved before production use.
- The architecture identifies data shared with external AI services.

### SEC-AI-007 — AI security regression testing
**Related:** FR-029–FR-036; NFR-026, NFR-029, NFR-034, NFR-040  
Changes to models, prompts, retrieval configuration, knowledge indexes, tools, integrations or orchestration shall trigger appropriate security and quality regression testing.

**Acceptance criteria**
- An AI security evaluation set is maintained.
- Tests cover prompt injection and sensitive information disclosure.
- Tests cover cross-tenant retrieval.
- Tests cover unauthorised tool use and ticket actions.
- Tests cover unsupported answers and fabricated citations.
- Material AI changes trigger relevant tests.
- Results and approval decisions are recorded.
- Failed security tests block release until resolved or formally accepted by an authorised risk owner.

### SEC-AI-008 — AI resource and consumption protection
**Related:** FR-029; NFR-001–NFR-003, NFR-027, NFR-032  
The platform shall protect AI services against excessive requests, repeated execution, resource exhaustion and uncontrolled consumption.

**Acceptance criteria**
- AI request limits are defined.
- Resource and usage limits are enforced by application services or provider controls.
- Repeated or abusive requests are detectable.
- Long-running operations have appropriate limits.
- Usage and cost are monitored.
- Provider limits and service failures are handled safely.
- One customer cannot exhaust resources allocated to other customers.

## 6. Security operations and secure development requirements

### SEC-OPS-001 — Secure software development lifecycle
**Related:** NFR-011, NFR-014, NFR-015, NFR-019, NFR-024  
The platform shall follow a secure development lifecycle incorporating security requirements, secure design, code review, dependency assessment and security testing.

**Acceptance criteria**
- Security requirements are traceable to design and verification.
- Code changes undergo appropriate review.
- Dependencies are assessed for known vulnerabilities.
- Secret detection is included in development workflows.
- Security testing is included in delivery.
- Production deployment permissions are restricted.
- Security exceptions are documented and approved.

### SEC-OPS-002 — Vulnerability management
**Related:** NFR-011, NFR-014, NFR-015, NFR-024, NFR-031  
The organisation shall establish vulnerability management covering application code, dependencies, infrastructure, services and relevant AI components.

**Acceptance criteria**
- Vulnerabilities are identified through appropriate scanning and assessment.
- Findings are recorded and risk-prioritised.
- Remediation ownership is assigned.
- Remediation timelines are defined by an approved policy.
- Critical vulnerabilities are escalated.
- Remediation is verified.
- Accepted risks have an owner, rationale and review date.

### SEC-OPS-003 — Security incident management
**Related:** NFR-006–NFR-008, NFR-016, NFR-017, NFR-031, NFR-036, NFR-039  
The organisation shall establish incident detection, reporting, investigation, containment, recovery and post-incident review covering customer information, staff accounts, documents, AI services, integrations and business actions.

**Acceptance criteria**
- Incident categories are defined.
- Reporting and escalation responsibilities are documented.
- Security events support investigation.
- Containment and recovery procedures are documented.
- Customer data exposure and cross-tenant access are included.
- AI-related incidents, including prompt injection and data disclosure, are included.
- Incident records are protected.
- Post-incident reviews identify corrective actions.

### SEC-OPS-004 — Security verification and testing
**Related:** NFR-019, NFR-023, NFR-026, NFR-033–NFR-035, NFR-040  
The platform shall maintain repeatable security verification mapped to security requirements, threat scenarios and architecture components.

**Acceptance criteria**
- Every approved security requirement has one or more verification methods.
- Tests cover authentication and authorisation.
- Tests cover tenant and facility isolation.
- Tests cover document ingestion and retrieval.
- Tests cover AI prompt injection and unauthorised actions.
- Tests cover ticket approval and submission integrity.
- Tests cover access revocation.
- Evidence is retained.
- Failures are tracked to resolution.

### SEC-OPS-005 — Environment separation
**Related:** NFR-024, NFR-031, NFR-033, NFR-036  
Development, test, staging and production environments shall be separated through appropriate access controls, credentials, data handling and deployment permissions.

**Acceptance criteria**
- Environment credentials and secrets are appropriately separated.
- Production access is restricted.
- Production data is not copied to lower environments without an approved protection process.
- Deployment permissions are restricted.
- Environment configuration is controlled.
- Administrative access is auditable.
- Separation is verified during security testing.

## 7. Security acceptance criteria to add to existing requirements

| Existing requirement | Additional security acceptance criteria |
|---|---|
| FR-001 | Test authentication, account recovery, MFA policy and session security. |
| FR-002 | Approve and test a complete role-permission matrix. |
| FR-003 | Test membership lifecycle, reassignment and multi-organisation access. |
| FR-004 | Test facility ownership and permission inheritance. |
| FR-005 | Verify server-side authorisation for every relevant operation. |
| FR-006 | Restrict who creates/changes simulation data and preserve provenance. |
| FR-007 | Distinguish event, record and ingestion times; protect integrity. |
| FR-008 | Prevent stale or unknown information being presented as confirmed. |
| FR-009 | Apply equivalent permissions and retention to historical records. |
| FR-010 | Enforce authorised creation, validation, approval and audit. |
| FR-011 | Enforce permissions during retrieval. |
| FR-012 | Validate recipient eligibility and protect notification content. |
| FR-013 | Record actor, time, reason and change history. |
| FR-014 | Restrict notification visibility to authorised users. |
| FR-015 | Validate recipients, secure provider integration and minimise content. |
| FR-016 | Restrict preference management and preserve mandatory policy. |
| FR-017 | Enforce authority, validation, approval and audit. |
| FR-018 | Enforce organisation and facility restrictions. |
| FR-019 | Authorise every state transition and record changes. |
| FR-020 | Preserve provenance; do not present estimates as confirmed facts. |
| FR-021 | Record actor, time and correction history. |
| FR-022 | Revalidate recipient eligibility before delivery. |
| FR-023 | Restrict restoration actions and support auditable corrections. |
| FR-024 | Enforce staff authentication, session security and customer/staff separation. |
| FR-025 | Restrict support access by role, purpose and customer scope; audit access. |
| FR-026 | Enforce permissions, validation, history and approval where appropriate. |
| FR-027 | Restrict support access and minimise exposed information. |
| FR-028 | Validate uploads, scan content, verify integrity and control indexing. |
| FR-029 | Protect against prompt injection, data disclosure, abuse and unsafe input. |
| FR-030 | Ensure citations refer to actual retrieved documents. |
| FR-031 | Refuse or escalate where evidence is insufficient, conflicting or unsafe. |
| FR-032 | Enforce retrieval-time authorisation, tenant isolation and permission-aware caching. |
| FR-033 | Make prohibited plant/OT capabilities technically unavailable to AI. |
| FR-034 | Preserve provenance and identify unknown or inferred fields. |
| FR-035 | Bind user approval to the exact final ticket payload. |
| FR-036 | Revalidate permissions and fields at submission; prevent replay and duplicates. |
| FR-037 | Enforce the same tenant/facility restrictions as other business records. |
| FR-038 | Protect privileged administration with MFA, least privilege, approval and audit. |
| FR-039 | Record actor, reason, previous and new values; approval where appropriate. |
| FR-040 | Protect audit records against unauthorised modification and deletion. |
| FR-041 | Treat guidance content as controlled information and untrusted instructions safely. |
| FR-042 | Enforce row-level permissions, authorised filters and controlled exports. |
| FR-043 | Protect scenario identifiers and source metadata; restrict scenario changes. |
| FR-044 | Quarantine and validate documents; invalidate indexes/caches on withdrawal. |
| FR-045 | Enforce permissions for every ticket transition and preserve integrity. |
| FR-046 | Define a measurable revocation objective and active-session behaviour. |
| FR-047 | Restrict feedback access, protect privacy and define reviewer/retention rules. |
| FR-048 | Protect traceability records and revalidate recipient permissions. |

## 8. Security considerations for existing NFRs

| Existing NFRs | Security considerations |
|---|---|
| NFR-001–NFR-005 | Include abuse protection, resource exhaustion and tenant isolation in performance/capacity considerations. |
| NFR-006–NFR-010 | Include fail-secure behaviour, dependency failure, protected backups, integrity and restore testing. |
| NFR-011–NFR-015 | Include usable security, accessible authentication journeys and security documentation. |
| NFR-016–NFR-018 | Include security monitoring, protected logs, data classification, retention and privacy governance. |
| NFR-019–NFR-023 | Include integrity, export protection, interoperability risks, portability and security testability. |
| NFR-024 | Separate environment credentials, secrets, production data and deployment permissions. |
| NFR-025 | Preserve simulation transparency in views, notifications, exports and AI summaries. |
| NFR-026 | Evaluate AI answer quality separately from AI security. |
| NFR-027 | Include AI usage limits and resource controls. |
| NFR-028 | Ensure document withdrawal invalidates indexes and caches. |
| NFR-029 | Prevent fabricated citations and unauthorised metadata disclosure. |
| NFR-030 | Require server-side authorisation and exact action approval. |
| NFR-031 | Restrict support access and audit privileged support activity. |
| NFR-032 | Include quotas and abuse protection. |
| NFR-033 | Cover databases, APIs, search, vector indexes, caches, files, exports, logs and background jobs. |
| NFR-034 | Include untrusted inputs, ingestion, model context, tool outputs and memory/context stores where applicable. |
| NFR-035 | Require deterministic policy enforcement, agent identity and restricted tool registration. |
| NFR-036 | Address deletion/retention across originals, embeddings, caches, logs, derived data and backups. |
| NFR-037 | Include recipient revalidation, idempotency and misdelivery protection. |
| NFR-038 | Address permission-change and ticket approval/submission race conditions. |
| NFR-039 | Restrict diagnostics, redact sensitive data and protect logs. |
| NFR-040 | Separate AI quality/security evaluation and test after model, prompt, tool or retrieval changes. |

## 9. Business rule security clarifications

Preserve BR-001–BR-013. Consider these clarifications during Product Owner review:

| Business rule | Security clarification |
|---|---|
| BR-001 | Define organisation-wide events, facility-specific events and access inheritance. |
| BR-002 | Enforce customer access on every server-side operation. |
| BR-003 | AI must not present restoration estimates as confirmed facts. |
| BR-004 | Protect notification recipient selection and content. |
| BR-005 | Apply simulation labels to all channels, exports and AI responses. |
| BR-006 | Bind explicit ticket confirmation to the exact final payload. |
| BR-007 | Define escalation and prohibited AI behaviour. |
| BR-008 | Remove retired documents from active retrieval and relevant caches. |
| BR-009 | Protect simulation provenance and metadata. |
| BR-010 | Apply controlled ingestion, approval and withdrawal to authoritative documents. |
| BR-011 | Enforce permissions and integrity across ticket lifecycle transitions. |
| BR-012 | Make plant/OT control and policy bypass technically unavailable to AI. |
| BR-013 | Define measurable permission revocation and session invalidation behaviour. |

## 10. AI threat scenarios

These scenarios are requirements-level hypotheses and must be validated against the eventual architecture.

| ID | Threat scenario | Potential impact | Required response |
|---|---|---|---|
| AI-01 | Malicious instructions embedded in an uploaded inspection report. | Prompt injection and unsafe AI behaviour. | Secure ingestion, untrusted-content handling and AI security tests. |
| AI-02 | Customer attempts to retrieve another organisation's documents. | Cross-tenant disclosure. | Server-side authorisation and retrieval-time ACL enforcement. |
| AI-03 | Withdrawn procedure remains in a vector index or cache. | Outdated or unauthorised guidance. | Document lifecycle controls and index/cache invalidation. |
| AI-04 | AI generates a fabricated citation. | Unsupported information is trusted. | Citation validation and provenance. |
| AI-05 | User attempts to make AI submit an unauthorised ticket. | Unauthorised business action. | Deterministic action authorisation and explicit confirmation. |
| AI-06 | Ticket is changed after approval. | Approval does not match submitted action. | Bind approval to exact payload and revalidate. |
| AI-07 | External provider retains sensitive prompts unexpectedly. | Data disclosure and privacy exposure. | Provider assessment, minimisation and contractual controls. |
| AI-08 | Repeated AI requests consume excessive resources. | Cost increase and service degradation. | Rate limits, quotas and monitoring. |
| AI-09 | Staff use broad support permissions unnecessarily. | Insider misuse and privacy breach. | Least privilege, purpose limitation and audit. |
| AI-10 | Model, prompt, retrieval or dependency changes without testing. | Security regression. | Change governance and regression testing. |

Airon Power is not currently confirmed to use multiple autonomous agents or autonomous code execution. Agentic risks must remain conditional unless those capabilities are explicitly introduced.

## 11. Architecture trust boundaries

The Enterprise Architect and Security Architect should explicitly model:
1. Customer and staff users and portals.
2. Authentication and authorisation services.
3. Business application services and customer data.
4. AI orchestration and policy enforcement.
5. Approved document storage and retrieval.
6. Maintenance ticket workflow.
7. External AI providers and third-party services.
8. Audit, monitoring and evaluation services.

The architecture must show where identity is established, permissions are evaluated, tenant boundaries are enforced, documents are approved and retrieved, AI inputs/outputs are controlled, tool calls are authorised, ticket approval is bound to submission, audit records are protected, external providers process information and access revocation propagates.

## 12. Frameworks and standards

Use relevant versions and tailor proportionately. Framework references do not constitute automatic compliance certification.

| Framework / standard | Intended use |
|---|---|
| NIST Cybersecurity Framework 2.0 | Cybersecurity governance and outcomes. |
| NIST SP 800-30 | Risk assessment. |
| NIST SP 800-37 | Risk management and authorisation lifecycle. |
| NIST SP 800-218 (SSDF) | Secure software development. |
| NIST AI Risk Management Framework | AI governance, mapping, measurement and management. |
| NIST AI 600-1 | Generative AI risk management. |
| OWASP ASVS | Application security verification. |
| OWASP API Security Top 10 | API security risks. |
| OWASP Top 10 for LLM Applications | LLM-specific risks. |
| OWASP Top 10 for Agentic Applications | Agent-specific risks, where applicable. |
| STRIDE | Threat identification. |
| PASTA | Risk-centred threat modelling. |
| LINDDUN | Privacy threat modelling. |
| MITRE ATT&CK | Enterprise/cloud attacker behaviour. |
| MITRE ATLAS | AI adversarial threats. |
| NIST SP 800-207 / Zero Trust | Identity- and resource-centred access control. |
| ISO/IEC 27001 and 27002 | Information security management and controls. |
| ISO/IEC 42001 | AI management systems. |
| ISO/IEC 23894 | AI risk management. |

Record the versions used. Verify current editions before relying on them.

## 13. Decisions required

### Product Owner
1. Define customer roles and internal staff roles.
2. Decide whether users can belong to multiple organisations or facilities.
3. Define which staff roles can access customer records and for what purposes.
4. Identify actions requiring additional approval.
5. Decide which documents AI may process.
6. Assign ownership for document upload, approval, replacement and withdrawal.
7. Define permitted AI actions and the ticket approval/submission authority.
8. Decide notification preferences and mandatory notification categories.

### Enterprise Architect
1. Define tenant isolation across application and data layers.
2. Define identity and permission propagation.
3. Define retrieval-time authorisation.
4. Define permission-aware vector indexes, caches and derived data.
5. Define the protected ticket approval/submission workflow.
6. Define encryption, key and secrets architecture.
7. Define audit protection.
8. Define external AI provider isolation.
9. Define access revocation propagation.
10. Show how plant/OT capabilities are technically excluded from AI.

### Business and risk owner
1. Define risk tolerance and residual-risk acceptance authority.
2. Define incident escalation and notification responsibilities.
3. Approve data retention and deletion periods.
4. Set an access revocation objective.
5. Set recovery and availability objectives.
6. Define assurance evidence required before production.

Do not invent numerical targets, retention periods, providers or technologies without an approved basis.

## 14. Readiness and next steps

The current requirements are conditionally ready for security design, not for an approved security baseline or implementation.

Readiness requires:
- Product Owner decisions on roles, access, documents, AI actions and approval.
- Enterprise Architect assessment of architectural feasibility and security constraints.
- Security Architect review of controls, trust boundaries and threat scenarios.
- Traceability from approved requirements to architecture and verification.
- Owners for outstanding risks and decisions.
- Formal approval by authorised stakeholders.

Recommended sequence:
1. Integrate and validate this document in the repository.
2. Product Owner reviews the combined product and security requirements.
3. Enterprise Architect incorporates approved security constraints into the TOGAF architecture.
4. Security Architect performs architecture-level threat modelling and control design.
5. Product Owner approves the requirements baseline.
6. Only then plan Jira epics, stories, tasks and security tests.

## 15. Repository governance

- Preserve FR-001–FR-048, NFR-001–NFR-040 and BR-001–BR-013.
- Preserve the Enterprise Architect skill at `.claude/skills/enterprise-architect/SKILL.md`.
- Keep all security requirements Proposed until approved.
- Do not merge pull requests or mark requirements approved without explicit authorisation.
- Do not create Jira tickets as part of this requirements integration.
- Do not add application implementation code.
- Record conflicts rather than silently resolving them.
- Keep requirement IDs stable and traceable.

**Final recommendation:** Conditionally ready for security design and architecture assessment. Not yet approved for implementation or final security baseline.
