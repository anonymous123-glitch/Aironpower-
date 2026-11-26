# Airon Energy — Product Requirements Baseline

**Document status:** Proposed — for product-owner review  
**Version:** 0.1  
**Scope:** Initial product discovery baseline  
**Data boundary:** Fictional training platform; simulated data only  
**Repository:** Aironpower-  
**Important:** This document is a working requirements proposal, not an approved specification. No technology stack, AWS services, implementation design or production integration is prescribed here.

## 1. Purpose and product context

Airon Energy is a fictional energy and utilities training company. The initial concept is a digital platform for industrial customers (including factories and food-processing businesses) to view simulated electricity-supply information, receive planned-maintenance and outage updates, and follow restoration progress. A separate internal staff portal supports authorised employees. An AI assistant is proposed to help plant staff find information in approved safety procedures and inspection reports and prepare maintenance-ticket requests.

This baseline turns that concept into testable candidate requirements. It deliberately does not claim that real-time utility telemetry, real electricity supply, real plant control, or real customer data exists.

## 2. Status and interpretation

| Label | Meaning |
|---|---|
| Proposed | Candidate requirement for review; not yet approved. |
| Confirmed context | Explicitly established in the current project brief. |
| Assumption | A working interpretation that needs validation. |
| Open decision | A question that must be answered before the requirement can be finalised. |
| TBD | A measurable target or policy has not yet been agreed. |

All requirements below are **Proposed** unless the context column explicitly says otherwise. Priority is an initial recommendation, not a commitment.

## 3. Product scope and boundaries

### In the initial product concept

- Customer-facing access to simulated supply status for a customer's permitted facilities.
- Planned maintenance information and notifications.
- Outage information, status updates and restoration progress.
- A separate internal staff portal.
- An AI assistant that searches approved safety procedures and inspection reports.
- A controlled workflow for preparing and submitting maintenance-ticket requests.
- User, organisation and facility concepts needed to support the above.

### Explicit boundaries

- The platform uses simulated data only.
- It must clearly identify simulation and must not imply that displayed information is a live feed from a real electricity network.
- No direct control of generation equipment, plant machinery or operational technology is in scope.
- No production utility integration is assumed.
- No specific cloud provider, AWS service, database, framework, programming language or deployment design is selected by this document.
- Detailed security architecture is deferred to the dedicated security-design phase; product-level access, privacy and safety needs are still captured here.

## 4. Stakeholders and user groups

| User group | Main goal | Candidate capabilities | Questions to validate |
|---|---|---|---|
| Customer organisation administrator | Manage their organisation's platform users and facilities | Invite/manage users; view permitted facilities; manage notification preferences | Can one customer have multiple sites and multiple administrators? |
| Customer / facility user | Understand the simulated supply position affecting their facility | View status, maintenance notices, outage updates and history | Which roles need read-only access? |
| Factory / operations manager | Understand operational impact and plan around maintenance or outages | View impact, timing, updates and restoration estimates | What information is needed to make operational decisions? |
| Plant staff member | Find approved procedural information and raise maintenance requests | Search documents; ask the AI assistant; prepare ticket requests | Which document categories and ticket types are in scope? |
| Airon Energy support / operations staff | Support customers and maintain accurate platform information | Internal views, customer/facility support, event and ticket management | Which staff roles and approval responsibilities exist? |
| Airon Energy administrator | Configure and administer the training platform | Manage staff access, reference data and platform configuration | Which configuration tasks belong to administrators? |

## 5. Functional requirements

The IDs are stable references for discussion and later Jira traceability. Requirements are grouped by capability. Acceptance criteria describe observable outcomes and should be refined with the project owner.

| ID | Area | Proposed requirement | Acceptance criteria / observable outcome | Priority | Status / notes |
|---|---|---|---|---|---|
| FR-001 | Identity and access | The platform shall allow an invited user to sign in using an approved authentication process. | A valid invited user can sign in; invalid or inactive accounts cannot access protected areas; the user receives a clear error without sensitive account details being exposed. | Must | Proposed; authentication method is an architecture decision. |
| FR-002 | Identity and access | The platform shall support distinct customer and internal staff user types. | Customer users are directed to customer capabilities and staff users to internal capabilities; role changes take effect on the next authorised request/session refresh. | Must | Proposed; exact roles to validate. |
| FR-003 | Customer organisation | The platform shall associate each customer user with an organisation. | An authorised administrator can see the organisation association; a user cannot select or change their organisation without an authorised process. | Must | Proposed; organisation lifecycle to define. |
| FR-004 | Facility management | The platform shall associate facilities with a customer organisation. | An authorised user can view the facilities associated with their organisation; facility name and identifying details are displayed consistently. | Must | Proposed; facility fields to define. |
| FR-005 | Customer visibility | The platform shall show a customer the facilities they are permitted to access. | The facility list contains only facilities authorised for that user; empty and loading states are clear. | Must | Proposed; access rules need security-phase validation. |
| FR-006 | Supply status | The platform shall display the simulated electricity-supply status for a permitted facility. | The status is visible, understandable and labelled as simulated; an unknown or unavailable status is not presented as normal supply. | Must | Confirmed concept; status vocabulary to agree. |
| FR-007 | Supply status | The platform shall display when the shown supply information was last updated. | Each status view displays a timestamp and timezone or an unambiguous local-time convention. | Must | Proposed; freshness threshold TBD. |
| FR-008 | Supply status | The platform shall distinguish current, delayed, stale and unavailable simulated information. | The interface clearly indicates the data condition; stale or unavailable data is not described as confirmed current information. | Must | Proposed; definitions and thresholds TBD. |
| FR-009 | Supply history | The platform should provide an authorised user with relevant historical supply-status events. | The user can review dated events for a permitted facility; no events and incomplete history are communicated clearly. | Should | Proposed; retention period TBD. |
| FR-010 | Planned maintenance | The platform shall allow an authorised internal user or approved simulated process to record a planned-maintenance event for a facility. | The event includes facility, summary, planned start/end, status and recorded/updated timestamps; invalid or incomplete required fields are rejected. | Must | Proposed; who can originate events is an open decision. |
| FR-011 | Planned maintenance | The platform shall display planned-maintenance details to users authorised for the affected facility. | The notice shows affected facility, description, planned window, current status and last update. | Must | Proposed. |
| FR-012 | Planned maintenance | The platform shall make an eligible planned-maintenance notice available to affected authorised customer users at least 48 hours before the planned start when the event is recorded with sufficient advance notice. | The notice records the planned start, notice-created time and delivery/availability outcome; events recorded with less than 48 hours' notice are clearly marked as late notice and are not represented as meeting the 48-hour target. | Must | Proposed; confirm whether 48 hours is a business commitment or best-effort target, and define the customer-visible notice channel and exception policy. |
| FR-013 | Planned maintenance | The platform shall communicate changes or cancellation of a planned-maintenance event. | Affected authorised users can see the revised details or cancellation and when it changed. | Must | Proposed; notification rules to define. |
| FR-014 | Notifications | The platform shall provide an in-platform notification view for relevant maintenance and outage events. | Users can view relevant notifications, their timestamps and associated facility/event; read/unread state is clear if included in MVP. | Must | Proposed; read/unread behaviour to validate. |
| FR-015 | Notifications | The platform should support one or more external notification channels if approved for the release. | For each configured channel, the platform records the notification attempt and provider outcome where available; failures are visible to authorised support staff and do not silently appear as successful delivery. | Should | Proposed; channel, provider, consent/recipient rules, delivery target, retry policy and cost require Product Owner decisions. External delivery is not assumed for MVP. |
| FR-016 | Notifications | The platform shall allow users to manage only those notification preferences permitted by their role and the approved notification policy. | Preference changes are saved and take effect for subsequent eligible notifications; mandatory categories, if approved, remain enabled and are clearly explained. | Should | Proposed; Product Owner to define mandatory categories, channel preferences and any consent requirements. |
| FR-017 | Outages | The platform shall allow an authorised internal user or approved simulated process to record an outage affecting a facility. | An outage record identifies affected facility, start time, description, status and source/recorded time. | Must | Proposed; outage creation authority to define. |
| FR-018 | Outages | The platform shall display outage information to users authorised for affected facilities. | Users can see affected facility, outage status, reported time and latest update. | Must | Proposed. |
| FR-019 | Outages | The platform shall support a defined outage lifecycle. | An outage can move through agreed states such as reported, investigating, update available, restoration in progress, restored and cancelled; invalid transitions are prevented. | Must | Proposed; final state model to approve. |
| FR-020 | Outages | The platform shall display an estimated restoration time when one is available. | An estimate is clearly marked as an estimate, includes its update time and can be absent without implying a confirmed restoration time. | Must | Proposed; business rule required. |
| FR-021 | Outages | The platform shall display progress updates for an outage until it is marked restored or cancelled. | Authorised users can view a time-ordered update history; the final state and time are visible. | Must | Proposed. |
| FR-022 | Outages | The platform shall notify affected users when material outage information changes. | A configured notification is generated for a status, estimate or restoration change; duplicate or superseded notices are handled according to an agreed rule. | Should | Proposed; material-change rules TBD. |
| FR-023 | Outages | The platform shall record when an outage is marked restored. | The restored status, restoration timestamp and updating actor/process are recorded and displayed to authorised users. | Must | Proposed. |
| FR-024 | Staff portal | The platform shall provide a staff-only portal separate from customer-facing capabilities. | An authorised staff user can access staff functions; customer users cannot access staff-only pages or operations. | Must | Confirmed concept; detailed roles TBD. |
| FR-025 | Staff portal | The platform shall allow authorised staff to search and view customer organisations and their facilities for support purposes. | Staff can find records within their assigned permissions; results show sufficient context to distinguish organisations and facilities. | Should | Proposed; staff access scope to define. |
| FR-026 | Staff portal | The platform shall allow authorised staff to create and update simulated maintenance and outage records. | Changes are validated, saved and reflected in relevant customer views; the change history identifies what changed and when. | Must | Proposed; approval workflow may be required. |
| FR-027 | Staff portal | The platform shall provide staff with a view of notification outcomes for supported events. | Authorised staff can identify queued, sent, failed or otherwise defined delivery states and the associated event. | Should | Proposed; delivery states depend on selected channel. |
| FR-028 | Document knowledge | The platform shall make only in-scope documents with a current approved status available to the AI assistant's knowledge workflow. | Each document has an identifiable owner, status, version and approval/effective date; unapproved, withdrawn, expired or superseded versions are excluded from normal retrieval. | Must | Proposed; document formats, approval authority, review interval and lifecycle policy require decisions. |
| FR-029 | AI assistant | The AI assistant shall allow an authorised plant staff user to ask questions about approved documents. | A user can submit a question and receive a response grounded in accessible approved material, or a clear indication that suitable information was not found. | Must | Proposed; supported document formats TBD. |
| FR-030 | AI assistant | The AI assistant should identify the source document and relevant reference for material answers. | The response provides a usable document title and, where available, page/section reference; unavailable references are not fabricated. | Should | Proposed. |
| FR-031 | AI assistant | The AI assistant shall communicate uncertainty and avoid presenting unsupported answers as established procedure. | When evidence is missing, ambiguous or conflicting, the assistant says so and directs the user to an appropriate human or source document. | Must | Proposed; escalation route TBD. |
| FR-032 | AI assistant | The AI assistant shall retrieve and disclose document content only where the requesting user is authorised to access that document and its parent organisation/facility scope. | Retrieval and response generation are constrained to the user's current permissions; direct questions, summaries and follow-up questions cannot reveal content outside that scope. | Must | Proposed; security architecture must define and verify server-side enforcement, tenant isolation and permission changes. |
| FR-033 | AI assistant | The AI assistant shall not issue instructions to control plant equipment or operational technology. | The assistant provides informational support only; no plant-control action or command is exposed through the assistant. | Must | Boundary requirement. |
| FR-034 | Maintenance tickets | The AI assistant should help an authorised staff user prepare a maintenance-ticket draft using information supplied by the user and permitted source material. | The draft identifies AI-suggested content, distinguishes unknown fields from inferred content, includes source references where relevant, and can be reviewed and edited before submission. | Should | Proposed; ticket fields, categories, priority rules and whether attachments are allowed remain open decisions. |
| FR-035 | Maintenance tickets | The platform shall require an authorised user to review the complete ticket summary and explicitly confirm submission as a separate action. | No ticket is submitted by generation, conversation, navigation or implicit consent; the user sees the final values and receives a ticket reference or clear failure outcome after confirmation. | Must | Proposed; define whether particular ticket categories require an additional human approval step. |
| FR-036 | Maintenance tickets | The platform shall validate ticket fields and the user's authority before accepting a submission. | Missing, invalid or unauthorised values are rejected with actionable feedback; duplicate submission attempts are handled safely; the system does not report success unless a ticket reference has been created. | Must | Proposed; required fields, validation rules, duplicate-handling behaviour and authority matrix TBD. |
| FR-037 | Maintenance tickets | The platform shall allow an authorised user to view the status of a submitted ticket. | The user can find their permitted ticket and see its current status and latest update. | Should | Proposed; ticket lifecycle TBD. |
| FR-038 | Administration | The platform shall allow authorised administrators to manage user access through an agreed user-lifecycle process. | Administrators can invite, deactivate and, where permitted, update users; changes are reflected in subsequent access decisions. | Must | Proposed; exact administrative roles TBD. |
| FR-039 | Administration | The platform shall provide an agreed process for correcting inaccurate simulated facility, supply, maintenance and outage information. | An authorised user can correct a record; the correction and its timestamp are retained; affected views show the corrected state. | Should | Proposed; correction authority and audit detail TBD. |
| FR-040 | Audit and history | The platform shall retain an access-controlled history of material business-record and administrative changes. | The history records the affected record, action, actor or system process, timestamp, and relevant before/after values or change summary; authorised reviewers can search it and ordinary users cannot alter the history. | Must | Proposed; event coverage, sensitive-field treatment, retention and export rules require approval. |
| FR-041 | Help and guidance | The platform should provide contextual guidance for key customer and staff workflows. | Users can access relevant explanations for status meanings, notifications and ticket actions without relying on undocumented behaviour. | Could | Proposed; content ownership TBD. |
| FR-042 | Reporting | The platform may provide basic operational summaries for authorised internal staff. | Agreed summary measures can be filtered by date, customer or facility within the viewer's permissions. | Could | Candidate future capability; metrics and value need discovery. |


| FR-043 | Simulated data management | The platform shall identify the source and simulation context for each simulated supply, maintenance and outage event. | Each event records its scenario/source identifier, event time, record time and whether it was manually entered or generated by an approved simulation process; the interface never presents it as verified live utility telemetry. | Must | Proposed; define the event provenance fields and who may create or amend simulation scenarios. |
| FR-044 | Document governance | The platform shall support an approved document lifecycle for AI knowledge sources, including review, approval, version replacement and withdrawal. | An authorised document owner can submit a version for approval, approve or reject it, replace it and withdraw it; status and effective dates are retained and withdrawn/superseded versions are excluded from current answers. | Must | Proposed; approval roles, review frequency and handling of documents already used in prior answers require decisions. |
| FR-045 | Maintenance tickets | The platform shall maintain a defined lifecycle for submitted maintenance tickets. | Each ticket has a unique reference, current status, responsible queue or owner where applicable, creation/update timestamps and a history of status changes; invalid status transitions are rejected. | Must | Proposed; ticket statuses, ownership model, closure rules and escalation process require Product Owner decisions. |
| FR-046 | Identity and access | The platform shall support timely removal or change of a user's access when their account, role, organisation or facility permissions change. | Deactivated users and users whose permissions have been withdrawn cannot continue to access protected functions using stale permissions; the change is recorded for audit. | Must | Proposed; acceptable propagation time and active-session behaviour require security and product decisions. |
| FR-047 | AI assistant governance | The platform shall provide an agreed way for authorised staff to report an AI response that is incorrect, unsupported or based on unsuitable source material. | A report can be associated with the response and relevant source references, routed to an authorised reviewer and tracked to a disposition without granting the reporter access to other users' private content. | Should | Proposed; reviewer roles, feedback data retention and whether user feedback is MVP scope are open decisions. |
| FR-048 | Notifications | The platform shall associate each notification with the business event and recipient eligibility decision that caused it. | An authorised reviewer can trace a notification to its source event, intended recipient group, channel, creation time and available delivery outcome. | Should | Proposed; detailed notification event model and privacy-safe diagnostics to be designed. |

## 6. Non-functional requirements

Non-functional requirements describe service qualities and operating constraints. Numerical targets are intentionally marked TBD where the project has not agreed them. They must be validated before architecture and testing baselines are approved.

| ID | Quality attribute | Proposed requirement | Measurement / acceptance approach | Priority | Target / open decision |
|---|---|---|---|---|---|
| NFR-001 | Performance | Common customer pages should load within an agreed response-time target under the expected normal user load. | Measure page-load time using representative test journeys and agreed percentile. | Must | Target, percentile and load profile TBD. |
| NFR-002 | Performance | Core read APIs should respond within an agreed latency target under expected load. | Automated performance tests measure API latency and error rate. | Must | Target and percentile TBD. |
| NFR-003 | Performance | Supply-status views should display the timestamp of the underlying data and communicate refresh delays. | Test with current, delayed and stale simulated records. | Must | Refresh interval and stale threshold TBD. |
| NFR-004 | Capacity | The platform should support an agreed initial number of customer organisations, facilities and concurrent users. | Load test using a documented representative dataset and concurrency profile. | Must | Initial capacity model TBD. |
| NFR-005 | Scalability | The product should be capable of growth in organisations, facilities, users, events, documents and AI requests without requiring a fundamental product redesign. | Architecture phase to assess scaling limits and document capacity assumptions. | Should | Growth forecast and planning horizon TBD. |
| NFR-006 | Availability | Customer-facing functions should meet an agreed availability objective during defined service hours. | Monitor service availability and report measured uptime against the agreed objective. | Must | Availability target, service hours and exclusions TBD. |
| NFR-007 | Reliability | The platform should handle unavailable or failed dependencies without showing misleading success states. | Failure tests verify clear errors, safe fallback behaviour and recorded failure outcomes. | Must | Dependency list and recovery behaviour TBD. |
| NFR-008 | Data freshness | The interface must make the age and condition of simulated operational information understandable. | Usability and functional tests verify timestamp, stale and unavailable indicators. | Must | Freshness categories and thresholds TBD. |
| NFR-009 | Notification reliability | Important notifications should have a defined delivery objective and observable delivery outcome. | Measure queued, attempted, delivered and failed notifications where supported. | Must | Channel, delivery objective, retry and escalation policy TBD. |
| NFR-010 | Recovery | The service should have documented backup and recovery objectives appropriate to the training platform. | Demonstrate restoration against agreed recovery point and recovery time objectives. | Should | RPO/RTO and backup retention TBD. |
| NFR-011 | Usability | A first-time customer user should be able to locate their facility and understand its supply status without specialist training. | Task-based usability testing with representative users. | Must | Test tasks and success threshold TBD. |
| NFR-012 | Accessibility | Customer and staff interfaces should target WCAG 2.2 AA, subject to validation of applicable scope. | Accessibility audit and automated/manual testing of key journeys. | Must | Conformance scope and remediation policy to confirm. |
| NFR-013 | Responsive design | Core customer and staff journeys should be usable on agreed desktop, tablet and mobile viewport sizes. | Responsive layout tests across the agreed device/browser matrix. | Should | Supported browsers and viewport matrix TBD. |
| NFR-014 | Consistency | Supply, maintenance and outage statuses should use consistent terminology and visual meaning throughout the platform. | Review UI states and test status labels across customer and staff journeys. | Must | Approved status glossary required. |
| NFR-015 | Maintainability | Product behaviour, configuration and operational procedures should be documented sufficiently for support and future change. | Review required documentation and conduct a support handover exercise. | Should | Documentation standard and ownership TBD. |
| NFR-016 | Observability | The service should provide operational logs, metrics and alerts sufficient to identify key failures and degraded user journeys. | Demonstrate detection of agreed synthetic failure scenarios and alert routing. | Must | Monitoring coverage and alert objectives TBD. |
| NFR-017 | Auditability | Material administrative and business-record changes should be traceable to an actor or system process and timestamp. | Audit review confirms required events can be queried by authorised staff. | Must | Event scope and retention TBD. |
| NFR-018 | Privacy | The product should collect and retain only information necessary for its approved training use cases. | Data inventory and retention review identify purpose, fields, access and deletion rules. | Must | Data inventory and retention schedule TBD. |
| NFR-019 | Data integrity | Business records and event histories should remain internally consistent when created or updated. | Validation and concurrency tests check required fields, relationships and update behaviour. | Must | Consistency rules to define in detailed design. |
| NFR-020 | Data export | The project should decide whether customers or administrators need to export relevant records and in which formats. | Validate export needs through discovery; test approved export scenarios if in scope. | Could | Export scope, format and permissions TBD. |
| NFR-021 | Interoperability | The product should have documented integration requirements and data formats for any future external systems. | Architecture phase documents candidate interfaces, data ownership and failure behaviour. | Should | No external integration is currently confirmed. |
| NFR-022 | Portability | Product requirements and data definitions should avoid unnecessary dependence on a single implementation technology. | Review architecture decisions and identify material vendor-specific constraints. | Could | Balance against justified platform choices. |
| NFR-023 | Testability | Each approved requirement should be verifiable through a defined test, inspection or demonstration. | Requirements review confirms measurable acceptance evidence exists. | Must | Test strategy to be produced later. |
| NFR-024 | Environment separation | Development, test and demonstration environments should use isolated simulated datasets and configuration. | Verify environment separation and confirm no real customer or plant data is used. | Must | Environment model to be designed. |
| NFR-025 | Simulation transparency | Users must be able to recognise that supply, maintenance and outage information is simulated. | Review key screens, notifications, exports and AI responses for clear simulation labelling. | Must | Exact wording and placement to approve. |
| NFR-026 | AI answer quality | The AI assistant should provide answers grounded in approved accessible documents and communicate when evidence is insufficient. | Build an evaluation set of representative questions, expected sources and unanswerable cases. | Must | Quality threshold and evaluation ownership TBD. |
| NFR-027 | AI response time | AI responses should meet an agreed user-experience target for typical questions. | Measure response latency over a representative evaluation set. | Should | Target and measurement method TBD. |
| NFR-028 | AI document currency | The assistant should use the current approved version of a document and avoid presenting retired material as current procedure. | Test document version changes, withdrawal and replacement scenarios. | Must | Document approval and retirement process TBD. |
| NFR-029 | AI explainability to users | The assistant should make source references and limitations visible enough for staff to verify important procedural information. | User evaluation confirms source links/references and uncertainty messages are understandable. | Must | Required citation format and escalation route TBD. |
| NFR-030 | AI action control | AI-generated maintenance-ticket content must remain a draft until an authorised user confirms submission. | Test that generation, editing or conversation alone cannot submit a ticket. | Must | Confirmation workflow is also captured in FR-035. |
| NFR-031 | Supportability | Staff should have enough operational information to investigate user-reported problems without exposing unnecessary customer information. | Run support scenarios and review access to diagnostics. | Should | Support roles and diagnostic fields TBD. |
| NFR-032 | Cost awareness | The product should define measurable usage and cost drivers for hosting, storage, notifications and AI usage before selecting implementation services. | Architecture and product review document expected usage assumptions and cost model. | Should | Usage assumptions and budget constraints TBD. |


| NFR-033 | Security and tenant isolation | The platform shall prevent users and service processes from accessing customer, facility, ticket or document data outside their authorised scope. | Security tests cover direct API requests, altered identifiers, search, AI retrieval, exports and staff access; unauthorised cross-organisation access is denied and security-relevant attempts are logged. | Must | Proposed; exact role/tenant model and assurance tests to be defined by Security Architect. |
| NFR-034 | AI security and source trust | The AI knowledge workflow shall treat retrieved documents and user-supplied content as untrusted data rather than executable instructions. | Evaluation tests include malicious or conflicting document content and prompt-injection attempts; the assistant does not follow embedded instructions to disclose data, bypass policy or perform unauthorised actions. | Must | Proposed; threat model, testing corpus and acceptable residual-risk criteria to be defined in security design. |
| NFR-035 | AI action boundary | AI capabilities shall be technically and operationally constrained to the approved informational and ticket-drafting functions. | Tests demonstrate that the assistant cannot invoke plant/OT controls, alter authoritative business records or submit tickets without the separately authorised workflow. | Must | Proposed; detailed enforcement and tool permissions are security/architecture decisions. |
| NFR-036 | Data lifecycle | The platform shall apply approved retention, archival and deletion rules to personal data, business records, documents, AI interaction records and audit records. | A data inventory maps each category to purpose, owner, retention rule and disposal method; approved deletion/retention scenarios can be demonstrated, including relevant legal or operational holds where applicable. | Must | Proposed; retention periods, lawful basis and hold rules require stakeholder/privacy review; do not invent periods. |
| NFR-037 | Notification reliability | Notification processing shall avoid silently losing or falsely confirming notifications during retries or dependency failures. | Tests cover retry, timeout, duplicate delivery attempts and provider failure; the recorded state distinguishes queued, attempted, provider-accepted, failed and unknown outcomes where the channel supports those distinctions. | Must | Proposed; delivery semantics, retry limits, duplicate tolerance and service objectives TBD. |
| NFR-038 | Concurrency and consistency | Concurrent updates to shared business records shall not silently overwrite conflicting changes or create invalid lifecycle states. | Concurrent-update tests demonstrate conflict detection or a documented resolution rule for maintenance events, outages, tickets and user permissions. | Must | Proposed; consistency model and conflict behaviour to be defined during architecture. |
| NFR-039 | Operational support | The service shall provide sufficient operational diagnostics to investigate failures while minimising exposure of customer data, credentials and sensitive document content. | Support scenarios can be investigated using role-appropriate logs and metrics; secrets and unnecessary sensitive content are not exposed in routine diagnostics. | Must | Proposed; logging fields, access roles, redaction and retention to be designed. |
| NFR-040 | AI quality assurance | The AI assistant shall be evaluated against a maintained set of representative answerable, unanswerable, ambiguous, conflicting-source and adversarial test cases before release and after material knowledge or model changes. | Evaluation results record test-set version, outcome, source-grounding, uncertainty handling and identified regressions; release thresholds are approved before production-like use. | Must | Proposed; evaluation ownership, dataset governance and pass thresholds TBD. |

## 7. Cross-cutting business rules

| ID | Proposed business rule | Related requirements | Decision needed |
|---|---|---|---|
| BR-001 | Every supply, maintenance and outage record displayed to users must be associated with a facility. | FR-004, FR-006, FR-010, FR-017 | Confirm whether any organisation-wide events are also needed. |
| BR-002 | Customer users may only view facilities and events they are permitted to access. | FR-005, FR-011, FR-018 | Define organisation, site and delegated-access rules. |
| BR-003 | A displayed restoration time is an estimate unless explicitly confirmed as restored. | FR-020, FR-023 | Agree wording and update rules. |
| BR-004 | The 48-hour planned-maintenance notice is a target where sufficient advance notice exists, not a promise for late-notice events. | FR-012, FR-013 | Approve exception wording and handling. |
| BR-005 | Simulated information must be visibly identified as simulated. | FR-006, NFR-025 | Approve consistent labels across UI and notifications. |
| BR-006 | An AI-generated maintenance ticket is not submitted until a user explicitly confirms submission. | FR-034, FR-035 | Define whether some ticket categories require additional approval. |
| BR-007 | AI responses must not be treated as a replacement for approved safety procedures or qualified human judgement. | FR-029, FR-031, FR-033 | Agree user-facing disclaimer and escalation route. |
| BR-008 | Retired or unapproved documents must not be treated as current approved reference material. | FR-028, FR-032, NFR-028 | Define document owner and approval lifecycle. |


| BR-009 | Simulated operational information must retain provenance and must never be represented as verified live utility telemetry. | FR-006–FR-008, FR-043, NFR-025 | Define approved simulation sources, event timestamps and user-facing labels. |
| BR-010 | Only the current approved version of a knowledge document may be used as current procedural reference material; withdrawn or superseded versions must be excluded. | FR-028, FR-044, NFR-028, NFR-034 | Define document owner, approver and withdrawal process. |
| BR-011 | A maintenance ticket remains a draft until an authorised user explicitly confirms the final submission; ticket status changes follow an approved lifecycle. | FR-034–FR-037, FR-045, NFR-030, NFR-035 | Define ticket categories, statuses and any additional approval rules. |
| BR-012 | The AI assistant is informational and drafting support only; it must not control plant/OT systems or bypass authorised business workflows. | FR-033–FR-036, NFR-034, NFR-035 | Confirm prohibited actions and security test coverage. |
| BR-013 | A change to a user's organisation, role, facility access or active status must be reflected in subsequent authorisation decisions. | FR-003, FR-005, FR-038, FR-046, NFR-033 | Define permission-change propagation and session invalidation expectations. |

## 8. MVP recommendation

This is a proposed starting scope, subject to product-owner approval.

| Capability | Proposed MVP scope | Reason |
|---|---|---|
| User and organisation model | Basic customer/staff distinction and organisation-to-facility association | Required foundation for all role-based journeys. |
| Customer supply view | Simulated status, timestamp and unavailable/stale state | Core customer value proposition. |
| Planned maintenance | Record, display and communicate planned maintenance, including 48-hour target handling | Core stated business need. |
| Outage tracking | Record outage, show status and updates, show estimate where available, mark restored | Core stated business need. |
| Staff portal | Basic authorised management of simulated facility, maintenance and outage information | Needed to operate and demonstrate customer workflows. |
| Notifications | In-platform notices; decide whether email/SMS is essential for MVP | Avoid committing to an external provider before requirements are validated. |
| AI document assistant | Search a small, approved sample set of simulated procedures and reports; show source references and uncertainty | Demonstrates the AI use case without overextending scope. |
| Maintenance ticket workflow | Prepare, review and explicitly confirm a simulated ticket submission | Demonstrates controlled assistance and a complete user journey. |
| Advanced reporting | Defer unless discovery establishes a critical need | Not necessary to prove the initial value proposition. |
| External utility integrations | Excluded from MVP | No real data source or integration is currently confirmed. |

## 9. Open product decisions and discovery questions

These questions should be answered before requirements are baselined.

| ID | Question | Why it matters | Suggested owner |
|---|---|---|---|
| Q-001 | Is the primary customer a factory, a multi-site industrial group, or both? | Determines organisation, facility and user model. | Project owner / Product Owner |
| Q-002 | What is the main customer problem Airon Energy is solving better than existing alternatives? | Establishes product-market rationale and prioritisation. | Project owner / Product Owner |
| Q-003 | What does “live” supply status mean in a simulated training environment? | Prevents misleading freshness expectations and defines refresh behaviour. | Product Owner |
| Q-004 | What supply status values should be supported? | Defines UI, business rules and event lifecycle. | Product Owner |
| Q-005 | Who creates and approves planned-maintenance and outage events? | Defines staff workflows and accountability. | Product Owner |
| Q-006 | What happens when less than 48 hours' notice is possible? | Defines exception handling and customer expectations. | Product Owner |
| Q-007 | Which notification channels are essential for MVP? | Affects user experience, cost and integration requirements. | Project owner / Product Owner |
| Q-008 | Are customer users able to manage their own colleagues? | Determines onboarding and administration scope. | Product Owner |
| Q-009 | What are the staff roles and which tasks may each perform? | Defines internal workflows and permissions. | Project owner / Product Owner |
| Q-010 | Which safety procedures and inspection-report formats are in scope for the AI assistant? | Determines document workflow and evaluation scope. | Product Owner |
| Q-011 | Who approves, updates and retires AI knowledge documents? | Prevents outdated or unapproved material being used. | Product Owner |
| Q-012 | Which maintenance ticket fields, categories and statuses are required? | Defines the ticket lifecycle and acceptance criteria. | Product Owner |
| Q-013 | Does ticket submission require approval for particular categories? | Defines the business workflow. | Project owner / Product Owner |
| Q-014 | What is the initial expected number of organisations, facilities and users? | Enables measurable capacity and performance targets. | Project owner |
| Q-015 | What availability and recovery expectations are appropriate for a training platform? | Enables proportionate non-functional targets. | Project owner / later architect |
| Q-016 | Is email/SMS notification delivery required in MVP, or is in-platform notification sufficient? | Controls MVP scope and external dependencies. | Project owner / Product Owner |
| Q-017 | What customer-facing reports or exports are genuinely needed? | Avoids building low-value features. | Product Owner |
| Q-018 | What is the intended commercial model and who is the buyer? | Influences onboarding, tenant model and product packaging. | Project owner |

## 10. Assumptions register

| ID | Assumption | Impact if incorrect | Validation |
|---|---|---|---|
| A-001 | A customer organisation may have multiple facilities. | Changes data model and customer journeys. | Confirm with project owner. |
| A-002 | Multiple users may belong to one customer organisation. | Changes user administration and access requirements. | Confirm with project owner. |
| A-003 | Some internal staff will create or update simulated operational events. | Determines staff portal scope and workflow. | Confirm staff roles. |
| A-004 | The AI assistant uses a curated set of approved documents rather than unrestricted file access. | Changes document ingestion and assistant behaviour. | Confirm document workflow. |
| A-005 | In-platform notifications are required even if external channels are deferred. | Affects MVP notification capability. | Confirm MVP scope. |
| A-006 | The platform is a training demonstration, not a live utility operations system. | Defines data, safety and integration boundaries. | Confirm continued simulation-only scope. |

## 11. Requirement traceability and future handovers

Each requirement ID should be retained when the work is moved into Jira and linked to:

- Its product objective and user journey.
- The relevant epic and user story.
- Acceptance tests or demonstration evidence.
- Architecture decisions and diagrams produced in the architecture phase.
- Security requirements and threat scenarios produced in the security phase.

The Product Owner should not select implementation technologies. The Enterprise and Solution Architect will use approved requirements to evaluate system structure, data flows, integrations and infrastructure. The Security Architect will then define detailed security controls and validate the security implications of the product and architecture.

## 12. Approval checklist

Before this document becomes a baselined product requirements specification, the project owner should confirm:

- [ ] The business concept and target customer are sufficiently clear.
- [ ] The user groups and organisation/facility model are agreed.
- [ ] The MVP scope is approved.
- [ ] Functional requirements have been reviewed.
- [ ] Non-functional targets have owners and measurable values where needed.
- [ ] The 48-hour maintenance-notice rule and exceptions are agreed.
- [ ] Supply and outage status definitions are agreed.
- [ ] AI document scope and maintenance-ticket workflow are agreed.
- [ ] Simulation-only boundaries are accepted.
- [ ] Open questions and assumptions have owners.
- [ ] Requirements are ready for architecture handover.

**Current recommendation:** Treat this as a Product Requirements Baseline v0.1 for review. Do not treat it as a final specification or use it as an instruction to begin implementation until the open decisions have been resolved and the project owner has approved the relevant scope.


## 13. Enterprise Architecture review — proposed amendments

**Review status:** Proposed architecture review; not an approval of the requirements baseline.  
**Review scope:** All 42 original functional requirements (FR-001–FR-042), all 32 original non-functional requirements (NFR-001–NFR-032), the eight original business rules, product scope, assumptions, open decisions and MVP proposal.  
**Outcome:** Conditionally suitable for continued architecture discovery, but not ready for an approved target architecture or implementation baseline.

### 13.1 Main findings and changes made

| Finding | Affected IDs | Action in this revision |
|---|---|---|
| Maintenance notice timing, channel and exception handling were not sufficiently separated. | FR-012, FR-015, FR-016, BR-004 | Clarified the 48-hour notice condition and made external channels explicitly release-dependent. |
| Simulation data provenance and scenario ownership were not specified. | FR-006–FR-008, FR-010, FR-017, NFR-025 | Added FR-043 and BR-009. |
| Knowledge document lifecycle was incomplete. | FR-028–FR-032, NFR-028 | Strengthened FR-028 and added FR-044 and BR-010. |
| AI-generated ticket content and actual submission needed stronger separation. | FR-034–FR-037, NFR-030 | Strengthened draft, confirmation and validation requirements; added FR-045 and BR-011. |
| Access changes and tenant boundaries needed explicit product outcomes. | FR-003–FR-005, FR-032, FR-038, NFR-018 | Strengthened FR-032 and added FR-046 and NFR-033. |
| AI source content can be misleading or contain malicious instructions. | FR-028–FR-035, NFR-026, NFR-028 | Added NFR-034 and NFR-035; detailed controls remain for Security Architecture. |
| Notification delivery and record consistency needed failure/concurrency treatment. | FR-014–FR-022, FR-026, FR-040 | Added FR-048, NFR-037 and NFR-038. |
| Retention, diagnostics and AI evaluation lacked explicit governance. | FR-040, NFR-016–NFR-018, NFR-026–NFR-031 | Added NFR-036, NFR-039 and NFR-040. |
| Audit scope and ticket status ownership were too broad or incomplete. | FR-035–FR-040, NFR-017 | Strengthened FR-040 and added FR-045. |

### 13.2 Requirements retained but requiring Product Owner decisions

The following matters have deliberately not been assigned invented values or policies:

- NFR-001–NFR-006 and NFR-027: performance, capacity, availability and AI response-time targets.
- NFR-010: recovery point, recovery time and backup retention.
- NFR-012–NFR-013: accessibility conformance scope and supported browser/device matrix.
- NFR-018 and NFR-036: data categories, retention periods and disposal rules.
- NFR-026 and NFR-040: AI quality measures, test-set ownership and release thresholds.
- FR-012 and FR-015: whether external notifications are in MVP, channel choice and delivery objectives.
- FR-019 and FR-045: outage and ticket lifecycle states, transitions and ownership.
- FR-028 and FR-044: document approval roles, review frequency and withdrawal authority.
- FR-034–FR-036: ticket fields, categories and additional approval requirements.
- NFR-033–NFR-035: security design and assurance criteria, to be developed with the Security Architect.

### 13.3 Architecture readiness recommendation

**Conditionally ready for architecture discovery and option analysis. Not ready for architecture baseline approval or implementation.**

Before target architecture is baselined, the Product Owner should resolve or explicitly accept the key decisions above, especially the operating model for simulated data, user/organisation/facility relationships, notification commitments, document governance, ticket lifecycle, service quality targets and data retention. The Security Architect must then define and validate the detailed security requirements and controls.

The Enterprise Architect may proceed with clearly labelled assumptions and options, but must not treat unresolved targets, integrations, roles or policies as approved facts.

### 13.4 Change control

This revision preserves the existing FR/NFR identifiers and adds FR-043–FR-048 and NFR-033–NFR-040. Existing requirement wording has been refined where the architecture review found material ambiguity. All changes remain **Proposed** and require Product Owner review. No implementation technology, cloud service, vendor or production integration has been selected.
