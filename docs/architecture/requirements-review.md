# Airon Energy — Enterprise Architecture Requirements Review

**Review status:** Proposed — for Product Owner response  
**Framework:** TOGAF Standard, 10th Edition (tailored)  
**Source reviewed:** `docs/product/requirements.md`, Product Requirements Baseline v0.1, branch `product/requirements-baseline`  
**Review scope:** Product context, scope boundaries, stakeholders, all 42 functional requirements (FR-001–FR-042), all 32 non-functional requirements (NFR-001–NFR-032), eight business rules, MVP proposal, open decisions and assumptions.  
**Completeness:** All numbered FRs and NFRs in the source version were reviewed individually. This is an architecture assessment, not Product Owner approval or a security assurance report.

## 1. Executive assessment

**Recommendation: Conditionally ready for architecture discovery and option analysis; not ready for an approved target architecture or implementation baseline.**

The concept has a coherent initial boundary: a simulation-only customer/staff platform, simulated supply and outage information, and an AI assistant for approved documents and ticket drafting. The requirements are a useful discovery baseline, but they leave important business operating rules and measurable service qualities unresolved.

The most significant architecture drivers are organisation/facility access, simulation data provenance, document governance and retrieval permissions, controlled ticket submission, notification delivery, auditability, data lifecycle, and measurable performance/availability/recovery targets.

## 2. Requirement-by-requirement review register

### 2.1 Functional requirements

| Requirement | TOGAF domain(s) | Review outcome | Finding and recommended action |
|---|---|---|---|
| FR-001 | B/A/T | Clarification required | Define account invitation, authentication assurance, recovery and inactive-account behaviour; method remains an architecture decision. |
| FR-002 | B/A | Change recommended | Replace broad customer/staff distinction with a validated role model and permitted capability matrix. |
| FR-003 | B/D | Clarification required | Define organisation ownership, lifecycle, reassignment and whether users can belong to multiple organisations. |
| FR-004 | B/D | Clarification required | Define facility identifiers, required attributes, ownership and lifecycle. |
| FR-005 | B/D/A | Accepted as architecture input | Core access-scoped facility visibility; define delegated and multi-facility access rules. |
| FR-006 | B/A/D | Accepted as architecture input | Core value and simulation boundary; agree status vocabulary and source provenance. |
| FR-007 | D/A | Change recommended | Timestamp must identify event time versus record/ingestion time and use a consistent timezone convention. |
| FR-008 | B/D/A | Clarification required | Define measurable freshness thresholds and the business meaning of delayed, stale and unavailable. |
| FR-009 | B/D | Clarification required | Define history scope, retention, completeness and whether history is MVP. |
| FR-010 | B/D/A | Clarification required | Define event creator, approval, correction and simulation-generation responsibilities. |
| FR-011 | B/A | Accepted as architecture input | Core customer view; align fields and status vocabulary with FR-010 and FR-013. |
| FR-012 | B/A/T | Change recommended | Clarify the 48-hour target, late-notice exceptions, channel, delivery outcome and whether it is a commitment. |
| FR-013 | B/A | Clarification required | Define which changes trigger notices and how prior notices are superseded. |
| FR-014 | B/A | Clarification required | Decide read/unread, notification history, retention and in-platform delivery behaviour. |
| FR-015 | B/A/T | Change recommended | External channels are optional until approved; define channel, provider, recipient rules, failures, cost and delivery objective. |
| FR-016 | B/A | Change recommended | Define mandatory notification categories and preference/consent policy. |
| FR-017 | B/D/A | Clarification required | Define outage source, authority, affected-facility model and correction process. |
| FR-018 | B/D/A | Accepted as architecture input | Core authorised outage visibility; align with organisation/facility access rules. |
| FR-019 | B/A | Change recommended | Agree a formal state model, permitted transitions, cancellation and reopened-event behaviour. |
| FR-020 | B/D/A | Clarification required | Define who may set estimates, update rules, timezone and when estimates expire. |
| FR-021 | B/D/A | Accepted as architecture input | Define ordering, correction policy and retention for the update history. |
| FR-022 | B/A/T | Clarification required | Define material-change events, deduplication, recipient selection and notification failure behaviour. |
| FR-023 | B/D/A | Accepted as architecture input | Define authority to mark restored and whether restoration can be reversed/corrected. |
| FR-024 | B/A/T | Accepted as architecture input | Separate staff capability is clear; define staff roles and whether separate portal means separate application or logical experience. |
| FR-025 | B/D/A | Clarification required | Define support access boundaries, search scope, purpose limitation and oversight. |
| FR-026 | B/D/A | Clarification required | Define who can create/update events, approval needs, concurrency and audit coverage. |
| FR-027 | B/A | Clarification required | Define delivery states consistently with channel/provider semantics and support permissions. |
| FR-028 | B/D/A | Change recommended | Require document owner, version, approval/effective status and exclusion of retired/unapproved sources. |
| FR-029 | B/A/D | Accepted as architecture input | Define supported formats, user groups, answer scope and fallback/escalation. |
| FR-030 | B/D/A | Accepted as architecture input | Define citation granularity and behaviour when source locations are unavailable. |
| FR-031 | B/A | Accepted as architecture input | Define escalation route and how conflicting or insufficient evidence is communicated. |
| FR-032 | B/D/A/T | Change recommended | Specify current permission enforcement across retrieval and generated responses; Security Architect to validate tenant isolation. |
| FR-033 | B/A/T | Accepted as architecture input | Important safety boundary; translate into explicit capability/tool restrictions and tests. |
| FR-034 | B/A/D | Change recommended | Distinguish extracted facts, AI suggestions and unknown fields; define ticket schema and allowed source data. |
| FR-035 | B/A/T | Change recommended | Require review of final ticket contents and a distinct explicit submission action; define any extra approval. |
| FR-036 | B/A/D | Change recommended | Validate fields and authority; address duplicate submissions and clear failure/success outcomes. |
| FR-037 | B/A/D | Clarification required | Define ticket lifecycle, visibility, ownership and which users can see status. |
| FR-038 | B/A | Clarification required | Define admin roles, user lifecycle, approval and access-revocation timing. |
| FR-039 | B/D/A | Clarification required | Define correction authority, reason capture, downstream notification and history treatment. |
| FR-040 | B/D/A/T | Change recommended | Define auditable event scope, actor/process identity, timestamps, change detail, access and retention. |
| FR-041 | B/A | Defer / validate value | Useful but not core unless usability testing identifies a need; define owner and content maintenance. |
| FR-042 | B/D/A | Defer / validate value | Candidate reporting scope needs user value, metrics, data definitions and permission rules before inclusion. |

### 2.2 Non-functional requirements

| Requirement | TOGAF domain(s) | Review outcome | Finding and recommended action |
| NFR-001 | A/T | Clarification required | No response-time target, percentile, page set or representative load profile; set measurable objectives. |
| NFR-002 | A/T | Clarification required | Define API latency percentiles, endpoints, load and error-rate objectives. |
| NFR-003 | D/A | Accepted as architecture input | Important freshness transparency; align refresh interval and thresholds with FR-007/008. |
| NFR-004 | B/T | Clarification required | Provide initial organisation, facility, user and concurrency estimates. |
| NFR-005 | B/A/D/T | Clarification required | Define growth horizon and plausible growth scenarios; avoid vague 'no redesign' promise. |
| NFR-006 | B/A/T | Clarification required | Define service hours, availability target, measurement window, exclusions and business impact. |
| NFR-007 | A/T | Change recommended | Specify dependency failure modes, safe degradation, timeout/retry behaviour and recovery expectations. |
| NFR-008 | B/D/A | Accepted as architecture input | Align freshness labels and thresholds with FR-007/008 and NFR-003. |
| NFR-009 | B/A/T | Change recommended | Define channels, delivery semantics, retries, duplicate tolerance, escalation and delivery objective. |
| NFR-010 | B/D/T | Clarification required | Set RPO/RTO, backup scope, retention and restoration-test frequency based on business impact. |
| NFR-011 | B/A | Clarification required | Define test tasks, representative users and measurable success threshold. |
| NFR-012 | B/A/T | Clarification required | Confirm WCAG 2.2 AA scope, exceptions, test method and remediation ownership. |
| NFR-013 | B/A/T | Clarification required | Define supported browsers, devices, viewport matrix and support policy. |
| NFR-014 | B/A | Accepted as architecture input | Establish an approved glossary and shared status model across customer/staff experiences. |
| NFR-015 | B/A/T | Clarification required | Define required operational, support, configuration and architecture documentation with owners. |
| NFR-016 | A/T | Change recommended | Define monitored journeys, metrics, alert thresholds, ownership and privacy-safe telemetry. |
| NFR-017 | B/D/A/T | Change recommended | Define events, tamper resistance, access, retention, search and sensitive-field treatment. |
| NFR-018 | B/D/A/T | Change recommended | Complete data inventory, purpose, ownership, retention, deletion and privacy review. |
| NFR-019 | D/A/T | Accepted as architecture input | Define transaction boundaries, invariants and conflict handling for concurrent updates. |
| NFR-020 | B/D/A | Defer / decide scope | Decide whether exports create customer value; if included, define formats, permissions and audit. |
| NFR-021 | B/D/A/T | Clarification required | No external integration is approved; document only candidate interfaces and data contracts when justified. |
| NFR-022 | B/A/T | Defer / qualify | Avoid technology-neutrality as an absolute; assess portability against justified cost and lock-in trade-offs. |
| NFR-023 | B/A/T | Accepted as architecture input | Define verification evidence and traceability for every approved requirement. |
| NFR-024 | D/A/T | Accepted as architecture input | Define dataset separation, environment permissions and promotion/reset approach. |
| NFR-025 | B/A/D | Accepted as architecture input | Ensure simulation labelling appears consistently in views, notifications, exports and AI responses. |
| NFR-026 | B/D/A/T | Change recommended | Define representative evaluation set, source-grounding measures, unanswerable cases, thresholds and owner. |
| NFR-027 | B/A/T | Clarification required | Set latency target and percentile for defined question types and evaluation conditions. |
| NFR-028 | D/A/T | Change recommended | Define approval, supersession, withdrawal, indexing and cache invalidation behaviour. |
| NFR-029 | B/D/A | Accepted as architecture input | Define citation format, source traceability and understandable uncertainty messages. |
| NFR-030 | B/A/T | Accepted as architecture input | Retain as explicit testable constraint aligned to FR-035; do not duplicate ticket workflow detail. |
| NFR-031 | B/A/T | Clarification required | Define support roles, diagnostic access, data minimisation and investigation scenarios. |
| NFR-032 | B/T | Clarification required | Define usage assumptions, budget envelope and cost drivers before platform selection. |

## 3. Cross-cutting findings

| Severity | Finding | Related requirements | Required action |
|---|---|---|---|
| High | Customer, organisation and facility access model is not sufficiently defined for a multi-customer product. | FR-002–FR-005, FR-025, FR-032, FR-038; NFR-018 | Product Owner confirms roles, organisation/facility relationships and support access; Security Architect defines enforcement and assurance. |
| High | AI knowledge sources need controlled ownership, approval, versioning and withdrawal. | FR-028–FR-032; NFR-026, NFR-028, NFR-029 | Define document lifecycle and source-of-truth; ensure superseded content is not returned as current. |
| High | AI-generated ticket content must be separated from authorised submission and ticket lifecycle. | FR-034–FR-037; NFR-030 | Define fields, statuses, owner, explicit confirmation and any additional approval categories. |
| High | Security and trust boundaries for AI, retrieved documents and tenant data need explicit architecture/security work. | FR-032–FR-036; NFR-017, NFR-018 | Threat model prompt injection, malicious/poisoned documents, cross-tenant disclosure and unauthorised actions; define tests. |
| High | Availability, recovery and performance are not measurable yet. | NFR-001, NFR-002, NFR-004–NFR-010, NFR-027 | Product Owner supplies user/load assumptions and business impact; agree measurable targets before baseline. |
| Medium | The 48-hour notice statement and external notification capability could be interpreted as a committed delivery promise. | FR-012–FR-016, FR-022; NFR-009 | Clarify commitment, exceptions, channel, recipient rules and delivery outcomes. |
| Medium | Simulated data creation, provenance and timestamps are underspecified. | FR-006–FR-008, FR-010, FR-017; NFR-003, NFR-008, NFR-025 | Define event provenance, simulation scenarios and event-time versus record-time semantics. |
| Medium | Business-record concurrency and event lifecycle transitions need explicit treatment. | FR-019, FR-026, FR-039, FR-040; NFR-007, NFR-019 | Define state transitions, conflict handling and correction history. |
| Medium | Audit, retention, privacy and support diagnostics need joined-up data governance. | FR-040; NFR-016–NFR-018, NFR-031, NFR-036 | Inventory data categories, purpose, access, retention, deletion and audit requirements. |
| Medium | Reporting and export are not yet justified by validated user needs. | FR-042; NFR-020 | Validate value and defer if not needed for MVP. |
| Low | Help content and broad portability wording could create scope without clear measurable value. | FR-041; NFR-022 | Validate through discovery; retain proportionate documentation and portability principles. |

## 4. Architecture domain assessment

- **Business Architecture:** Core user groups and capabilities are visible, but role ownership, event authoring/approval, support operating model, document stewardship and ticket ownership need decisions.
- **Data Architecture:** Organisation/facility relationships, event provenance, timestamps, document metadata/version lifecycle, ticket history, audit data and retention need definition.
- **Application Architecture:** Customer/staff capabilities, notification workflow, document ingestion/retrieval, AI interaction, ticket workflow and support/admin functions need service boundaries and interaction contracts.
- **Technology Architecture:** No stack should be selected yet. Availability, performance, recovery, identity, integration, observability, data protection and cost targets must inform later option assessment.

## 5. Recommended requirement changes

A proposed requirements revision accompanies this review. It preserves existing IDs where possible, refines ambiguous statements, and adds:
- FR-043–FR-048 for simulated-data provenance, document lifecycle, ticket lifecycle, access revocation, AI response feedback and notification traceability.
- NFR-033–NFR-040 for tenant isolation, AI source trust, AI action boundaries, data lifecycle, notification reliability, concurrency, operational diagnostics and AI quality assurance.
- BR-009–BR-013 for simulation provenance, document currency, ticket confirmation/lifecycle, AI action boundaries and permission-change propagation.

All additions and refinements remain Proposed and require Product Owner review.

## 6. Priority questions for the Product Owner

1. What are the customer organisation, facility and user relationships? Can a user belong to more than one organisation or facility?
2. Which customer and staff roles exist, and which actions can each perform?
3. Who creates, approves and corrects simulated supply, maintenance and outage events?
4. Is the 48-hour planned-maintenance notice a firm business commitment or a best-effort target?
5. Are external email/SMS/push notifications required for MVP, or is in-platform notification sufficient?
6. Who owns, approves, reviews and withdraws AI knowledge documents?
7. What ticket fields, categories, statuses and approval paths are required?
8. What are the expected initial organisation, facility, user and concurrent-user counts?
9. What availability, performance, backup and recovery outcomes are appropriate for a simulation/training platform?
10. What data is retained, for what purpose, and for how long? Which data must be deletable?
11. What AI answer-quality threshold and evaluation process are acceptable for the intended use?
12. Are reporting, export and contextual help essential to MVP, or can they be deferred?

## 7. Assumptions and constraints

- Simulation-only data remains a confirmed product boundary; no live utility telemetry or plant/OT control is assumed.
- No cloud provider, vendor, database, framework or external notification provider is selected by this review.
- Detailed security control design is assigned to the Security Architect, but security outcomes remain architecture drivers.
- Numerical targets and retention periods are not invented; they require business decisions supported by impact and usage assumptions.

## 8. Readiness gate

Before target architecture is approved:
- Product Owner reviews and responds to all proposed changes.
- Organisation/facility/role model is sufficiently defined.
- Simulation event source and provenance are defined.
- Document approval/withdrawal and ticket lifecycle are agreed.
- Notification commitment and channels are decided.
- Measurable performance, capacity, availability and recovery objectives are agreed or explicitly deferred with rationale.
- Data inventory, privacy, retention and deletion needs are identified.
- Security Architect completes threat modelling and defines security requirements and assurance approach.
- All remaining assumptions and decisions have owners and target dates.

**Final recommendation:** Proceed with TOGAF Preliminary/Phase A discovery and proportionate architecture option analysis using explicitly labelled assumptions. Do not approve a target architecture, select implementation technologies or begin implementation until the readiness gate is satisfied.
