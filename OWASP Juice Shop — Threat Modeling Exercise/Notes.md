# OWASP Juice Shop — Threat Modeling Exercise

## Summary

Performed an architecture-led threat-modeling exercise for OWASP Juice Shop using STRIDE. The work progressed from an initial threat assessment to a component-based architecture diagram and a standalone HTML threat-model document.

**Status:** Design-review baseline completed. Threats remain hypotheses pending validation against a specific application release and deployment.

## Objectives

- Understand the application’s architecture and security-relevant interactions.
- Identify assets, entry points, trust boundaries, and potential threats.
- Visualize threats over the application architecture.
- Define proposed mitigations and validation criteria.
- Produce a reviewable document containing both the threat description and diagram.

## Architecture Reviewed

| Element | Role in the Model |
|---|---|
| Angular SPA | Browser-based user interface |
| Express / Node.js | Application environment, static-content delivery, and REST API |
| Authentication and authorization | Logical responsibility for identity and permission checks |
| Shop business logic | Logical responsibility for shop operations and business invariants |
| Sequelize + finale-rest | Relational data access and generated CRUD functionality |
| Custom SQL access | Query path requiring separate injection review |
| SQLite | File-based relational data store |
| MarsDB | Additional MongoDB-like data store |
| WebSocket notifications | Challenge-notification channel |
| Google OAuth 2.0 | External identity-provider integration |

Authentication, authorization, and business logic were modeled as logical responsibilities, not asserted to be separately deployed services.

## Work Performed

### 1. Initial STRIDE Assessment

Created an initial threat model covering:

- System scope and architecture.
- Assets requiring protection.
- Security-relevant actors.
- Trust boundaries and data flows.
- Fourteen threat scenarios.
- Qualitative priorities and proposed mitigations.

The initial assessment was subsequently refined to distinguish architecture-supported information from assumptions and unverified vulnerability claims.

### 2. Architecture Visualization in PlantUML

Translated the model into PlantUML and added:

- Application components and data stores.
- External actors and the identity provider.
- Labeled interactions and data flows.
- Trust-zone groupings.
- STRIDE annotations with threat identifiers.

### 3. Diagram-Type Selection

Compared component, deployment, sequence, use-case, activity, class, object, state, and timing diagrams.

Selected a component diagram as the primary view because the objective was to overlay threats onto the application’s logical architecture.

Sequence diagrams were identified as optional follow-up views for examining individual attack scenarios and security checks.

### 4. Component-Diagram Refinement

Converted the earlier architecture diagram into a component-oriented view.

Changes included:

- Replacing deployment nodes with logical components.
- Separating architectural structure from threat annotations.
- Retaining threat identifiers `T1–T14`.
- Introducing a flow inventory for traceability.
- Clarifying that data-flow arrows are a modeling convention rather than strict UML dependency semantics.
- Avoiding an unsupported separate database-host boundary.
- Distinguishing the user-to-administrator authorization boundary from hosting boundaries.

> Flow identifiers are local to each diagram revision and should be reconciled before comparing versions.

### 5. Standalone Threat-Model Document

Generated a self-contained HTML document containing:

- An embedded component architecture diagram.
- Scope, assumptions, and exclusions.
- Stakeholders, threat actors, and protected assets.
- Component, trust-boundary, and data-flow inventories.
- Fourteen STRIDE threat entries.
- Proposed mitigations and validation criteria.
- Proposed accountable roles.
- A prioritized response plan.
- Evidence requirements, open questions, and review gates.

The document was organized around the four questions used in the MDN example:

1. What are we working on?
2. What can go wrong?
3. What are we going to do about it?
4. Did we do a good enough job?

## Threat Register Summary

> The following entries describe scenarios for investigation, not confirmed findings.

| ID | Threat Scenario |
|---|---|
| T1 | Login injection and authentication bypass |
| T2 | Forged or misused session credentials |
| T3 | Credential attacks and account-recovery abuse |
| T4 | Cross-site scripting |
| T5 | Mass assignment and overposting |
| T6 | Basket and checkout business-logic abuse |
| T7 | Insufficient security auditability |
| T8 | Broken object-level authorization |
| T9 | Sensitive client-side or error information exposure |
| T10 | SQL or document-query injection |
| T11 | Application resource exhaustion |
| T12 | Missing function-level authorization |
| T13 | Unsafe server-side processing, conditional on feature availability |
| T14 | Unprotected transport or incorrectly bound OAuth integration |

Each threat was associated with:

- Affected components or flows.
- Protected assets.
- A proposed response.
- A validation criterion.
- An accountable role.

## Risk-Assessment Approach

The document used a qualitative workshop scoring model:

- Likelihood: `1–3`.
- Impact: `1–3`.
- Inherent score: `Likelihood × Impact`.

| Score | Priority |
|---|---|
| 1–2 | Low |
| 3–4 | Medium |
| 6 | High |
| 9 | Critical |

These scores estimate the significance of a scenario if the hypothesized control failure exists. They are not CVSS scores, measured exploitability, or proof that the application contains a particular vulnerability.

Residual risk was not assessed because controls were not implemented or tested during the exercise.

## Key Modeling Decisions

- Browser input and caller-supplied identifiers are untrusted.
- Client-side restrictions are not treated as server-side authorization controls.
- SQLite and MarsDB do not automatically imply a separate database server or network boundary.
- The OAuth interaction remains high-level until callback and token-exchange behavior are verified.
- No production banking system, real payment provider, or blockchain service was added to the model.
- Release-specific JWT flaws, library versions, and exploit behavior were not treated as verified findings.
- Training-lab containment was separated from hypothetical application hardening.

## Deliverables Produced

| Deliverable | Description |
|---|---|
| Initial threat assessment | Architecture-led STRIDE analysis |
| PlantUML architecture model | Components, interactions, trust zones, and threat annotations |
| Component-diagram revision | Logical architecture view with a threat-model overlay |
| Standalone HTML document | Threat-model description, embedded diagram, register, response plan, and review criteria |

> The supplied PlantUML PNG could not be retrieved during document generation. The embedded diagram was reconstructed from the UML source in the conversation, rather than verified against the linked image.

## Limitations

- No application release, source commit, or container image digest was pinned.
- No source-code security audit was performed.
- No penetration testing, exploitation, or runtime verification was performed.
- Deployment topology, TLS configuration, session handling, OAuth exchanges, and operational controls remain unverified.
- Proposed owners are role placeholders, not assigned individuals.
- The document is a review baseline, not a security sign-off.
- These notes summarize completed work; they do not indicate that artifacts have been committed or pushed to GitLab.

## Follow-Up Checklist

- [ ] Add the reviewed PlantUML source and HTML document to the repository.
- [ ] Record the target Juice Shop release, commit, and image digest.
- [ ] Document the actual deployment and exposed endpoints.
- [ ] Reconcile components, flow IDs, and threat references with the implementation.
- [ ] Verify session handling and OAuth integration.
- [ ] Inventory generated CRUD endpoints and writable fields.
- [ ] Verify object-level and function-level authorization.
- [ ] Confirm applicable upload, parser, template, and outbound-request features.
- [ ] Execute the proposed validation checks in an authorized, isolated environment.
- [ ] Record evidence and mark each threat as confirmed, not observed, not applicable, or unresolved.
- [ ] Assign owners and remediation dates.
- [ ] Reassess residual risk and obtain review approval.

## Source Material

- [OWASP Juice Shop — Architecture Overview](https://help.owasp-juice.shop/introduction/architecture.html)
- [MDN — Example Threat Model](https://developer.mozilla.org/en-US/docs/Web/Security/Threat_modeling/Example_threat_model)
- [PlantUML — Component Diagram Syntax](https://plantuml.com/component-diagram)
- Earlier threat-model descriptions and PlantUML revisions from this exercise.
