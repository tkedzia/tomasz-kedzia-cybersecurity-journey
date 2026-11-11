Sprawdziłem Twój plik. Dopasowałem poniższą wersję do tego samego układu: tytuł, podtytuł, tabela czasu nauki, numerowane sekcje, separatory `---`, tabele, cytaty i schematy z wcięciem.

Zapisz ją jako `data-risk-and-controls-learning/02-risk-ownership-and-evidence.md`.

Kliknij „Kopiuj” przy bloku poniżej i wklej jego zawartość do edytora pliku. Pierwszą linią pliku ma być `# Study Notes`, a nie trzy backticki ani słowo `markdown`. Objęcie całego dokumentu backtickami spowodowałoby wyświetlenie go jako kodu zamiast sformatowanych notatek. [docs.github](https://docs.github.com/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

```markdown
# Study Notes — 9 November 2026

## Risk, Ownership and Evidence

These notes cover the risk, ownership and evidence session from the EY interview preparation plan: describing business risks, distinguishing responsibilities, reviewing evidence and understanding the difference between remediation closure and risk acceptance.

Prepared: 11 November 2026. The title refers to the session date in the learning plan, not the date on which the notes were prepared or reviewed.

Study time: approximately 60 minutes. No lab, new software or additional research is required. The worked examples are fictional and are not descriptions of EY’s environment or a banking client’s actual controls.

| Time | Read and review |
|---|---|
| 0–10 minutes | Sections 1–3: risk concepts and assessment |
| 10–25 minutes | Sections 4–5: your NIST exercise and ownership |
| 25–50 minutes | Sections 6–9: evidence and three completed risk records |
| 50–60 minutes | Sections 10–13: closure, interview answers, knowledge check and recap |

The examples replace the blank-writing exercise from the plan so that you can learn by reading. Reading them does not establish that you have independently performed a risk assessment or executed control tests.

---

## 1. The central idea

The central task is to connect a business risk with an appropriate response, clear responsibilities and evidence supporting the conclusion.

A useful mental model is:

    BUSINESS OBJECTIVE AND ASSET
                  ↓
    THREAT EVENT + VULNERABILITY / EXPOSURE
                  ↓
    POTENTIAL BUSINESS IMPACT
                  ↓
    RISK ASSESSMENT AND PRIORITY
                  ↓
    RESPONSE + NAMED RESPONSIBILITIES
                  ↓
    IMPLEMENTATION AND OPERATION
                  ↓
    EVIDENCE + REVIEW
                  ↓
    REMAINING RISK + AUTHORISED DECISION

NIST CSF 2.0 connects threats, vulnerabilities, likelihood and impact to understanding inherent risk and prioritising responses through ID.RA-05.

It also calls for responses to be chosen, prioritised, planned, tracked and communicated through ID.RA-06.

### The interview shift

Move from:

> “I tracked the project until it was completed.”

Towards:

> “I tracked the actions, clarified ownership and helped make the outcome traceable to the requirement. I would distinguish implementation completion from validation and from any decision to accept remaining risk.”

The second statement does not claim that you performed every technical test or formally accepted risk.

### Connection to the Excel plan

This lesson follows the session in `03_Plan` and addresses:

- G01: risk → control → evidence.
- G06: ownership and authority to accept risk.
- Supporting G04: control validation and evidence-based closure.
- Supporting G10: concise interview answers in English.

The main completion test is:

> Do not confuse the risk owner with the person configuring the control.

### Key sentence to remember

> A completed action, a validated control and an accepted risk are different conclusions. Each requires an appropriate basis and decision authority.

---

## 2. Risk, control, issue and action are different things

For this lesson, use the following working distinctions:

| Object | Question answered | Example |
|---|---|---|
| Business objective | What must the business accomplish? | Collaborate with authorised clients without disclosing their information inappropriately |
| Asset | What has value or needs protection? | Confidential client information in a working file |
| Threat event | What could happen? | An employee sends the file to an unauthorised external recipient |
| Vulnerability or exposure | What could enable the event? | Missing restrictions, incorrect classification or overly broad exceptions |
| Risk | What uncertainty and potential harm matter, considering likelihood and impact? | Inappropriate disclosure may cause client harm, contractual consequences and loss of trust |
| Control | What reduces or manages the risk? | A scoped DLP rule plus an approved transfer route and operational review |
| Issue | What observed deficiency needs attention? | An enforcement test failed for an in-scope user |
| Action | What work will address the issue? | Correct the rule scope and repeat the test |
| Evidence | What supports the conclusion? | Dated configuration, scope records, test results and operational records |

These are practical distinctions for analysing scenarios. An organisation may use different labels or issue taxonomies.

### 2.1 A better way to describe a risk

Weak description:

> “DLP is missing.”

This describes a possible control gap. It does not explain the complete business risk.

A usable risk-statement template is:

> Because of [vulnerability/exposure], [actor or circumstance] may cause [event] affecting [asset/process], resulting in [business consequences].

Example:

> “Because the regional email control does not cover all intended senders, an employee may transmit confidential client information to an unauthorised external recipient, resulting in inappropriate disclosure and potential contractual or reputational harm.”

Why this is stronger:

- It identifies the relevant exposure.
- It names an event and affected information.
- It connects the event to consequences.
- It gives stakeholders something to assess and prioritise.

A well-written statement alone does not establish the likelihood, impact or existence of a vulnerability. Record the supporting facts and assumptions.

### 2.2 From a risk to an action

    Risk:
    Confidential client information may be disclosed externally.
                              ↓
    Identified issue:
    The intended sender group is not fully included in policy scope.
                              ↓
    Remediation action:
    Correct the scope and validate the relevant scenarios.
                              ↓
    Evidence:
    Scope reconciliation, policy version and test results.
                              ↓
    Decision:
    Close the specific action if its agreed criteria are met.
    Assess remaining risk separately.

### Important limitation

> Closing the scope issue does not prove that every possible disclosure route has been controlled.

Other channels, exceptions, detection limitations and authorised-recipient behaviour may remain relevant.

---

## 3. Inherent, current residual and target risk

### 3.1 Working definitions

| Term | Meaning in this lesson | Practical question |
|---|---|---|
| Inherent risk | Risk before considering the relevant controls, using the organisation’s defined baseline and methodology | What exposure are we assessing before crediting those controls? |
| Current residual risk | Risk remaining after considering controls that are actually implemented and operating, including their coverage and limitations | What exposure remains now? |
| Target residual risk | The intended position after planned improvements | What exposure are we aiming to reach? |

NIST defines residual risk as the portion remaining after security measures have been applied.

A planned safeguard is therefore not sufficient evidence that current residual risk has already decreased.

Example:

    Planned:
    Enable the external-email restriction next week.

    Current:
    The policy remains in simulation.

    Conclusion:
    Do not claim that blocking has reduced today’s exposure.

### 3.2 Risk ratings need a method

For an interview scenario, explain:

1. What is in scope?
2. What evidence informs likelihood and impact?
3. What existing controls are considered?
4. What uncertainties remain?
5. Who reviews the assessment?

Do not invent an EY scoring matrix.

A qualitative label such as “High” is not self-explanatory without the criteria and assumptions behind it.

The worked records later in this lesson deliberately avoid numerical ratings because we lack the facts and approved scoring method required to justify them.

### 3.3 Risk appetite versus risk tolerance

Risk appetite concerns the broad types and amount of risk an organisation is willing to accept in pursuing its objectives.

Risk tolerance expresses acceptable risk or variation more specifically in the relevant context.

The exact relationship and operational thresholds should follow the organisation’s methodology.

Fictional illustration:

| Concept | Illustration |
|---|---|
| Appetite | Very limited willingness to expose confidential client data to unauthorised recipients |
| Operational tolerance | Overdue high-exposure exceptions require escalation to a designated authority within a defined period |

This illustration is not a legal limit or an EY threshold.

Broad statements must be translated into usable decision criteria rather than left as slogans.

### Interview-ready summary

> “I would distinguish the current evidence-supported risk position from the target position after planned remediation. I would use the organisation’s assessment method, record assumptions and confirm who can approve any remaining exposure.”

---

## 4. Revisit your NIST exercise

Your existing exercise uses the right basic structure:

1. Interpret the outcome.
2. Identify risks if the outcome is not achieved.
3. Identify evidence.
4. Identify owners.

The main development opportunity is making each answer more specific and avoiding universal ownership claims.

The three outcomes below come from the final NIST CSF 2.0. Suggested roles and evidence are teaching examples, not mandatory NIST assignments.

### 4.1 RS.CO-02 — incident notification

Official CSF outcome:

> “Internal and external stakeholders are notified of incidents.”

#### Plain-English interpretation

The organisation identifies the relevant recipients and communicates through its incident process in line with applicable policy, contractual and legal requirements.

#### Risks if the outcome is not achieved

- Necessary responders may not coordinate promptly.
- Affected stakeholders may lack information needed to act.
- Applicable notification obligations may be missed.

#### Evidence: process versus execution

| Evidence | What it supports | What it does not establish alone |
|---|---|---|
| Incident communication procedure | A process has been defined | The process was followed for a specific incident |
| Escalation/contact matrix | Intended recipients and routing are documented | Contact information is current and usable |
| Case timeline and notification records | Who was informed, when and through which channel | That all applicable recipients and deadlines were correctly identified |
| Recorded exercise | The communication process was exercised under stated conditions | That a real incident was handled correctly |

#### Ownership refinement

Incident response may coordinate the workflow, while legal, privacy, communications and business roles perform or approve particular notifications.

Confirm the documented model.

Do not conclude that the SOC must own all external notifications simply because the outcome is in the Respond function.

CSF specifies outcomes. It does not prescribe one universal organisational chart.

#### Interview sentence

> “I would check both the communication procedure and the records of its execution. I would also confirm who is authorised to approve each type of external notification.”

### 4.2 ID.AM-05 — asset prioritisation

Official CSF outcome:

> “Assets are prioritized based on classification, criticality, resources, and impact on the mission.”

#### Plain-English interpretation

The organisation distinguishes which assets matter most and uses that prioritisation to guide protection and operational decisions.

Your original answer already connected classification and business importance to protection.

Strengthen it by showing:

- The prioritisation criteria.
- Their actual application to assets.
- The resulting decisions.

#### Example evidence chain

    Defined prioritisation criteria
                  ↓
    Inventory with owner, classification and criticality
                  ↓
    Review / approval appropriate to the process
                  ↓
    Protection or remediation decisions reflecting that priority

Business and asset owners can explain mission impact. Security and risk specialists support the method and threat/control analysis.

The exact accountability must be agreed locally.

#### Important limitation

> A classification policy alone does not demonstrate that every relevant asset has been identified, classified or prioritised.

#### Interview sentence

> “I would look for the criteria, how they were applied to the asset inventory, and evidence that the resulting priorities actually influenced protection and remediation decisions.”

### 4.3 GV.RM-02 — appetite and tolerance

Official CSF outcome:

> “Risk appetite and risk tolerance statements are established, communicated, and maintained.”

#### Plain-English interpretation

Decision-makers have an agreed understanding of acceptable risk and use it consistently.

Those expectations are communicated and kept current.

Your original answer mentioned policies, standards, procedures and training.

Add evidence of the explicit statements, approval, maintenance and actual use in decisions.

#### Example evidence

- Approved appetite/tolerance statements.
- Defined escalation thresholds or limits.
- Records showing communication to relevant decision-makers.
- Review and update records.
- Acceptance or treatment decisions referring to the applicable criteria.

#### Ownership refinement

“All employees” is too broad as an accountable-owner answer.

Employees may need to understand and follow the expectations. Designated leadership and governance roles establish and approve them under the organisation’s model.

#### Interview sentence

> “I would distinguish awareness of risk expectations from authority to define them. The evidence should show approval, communication, maintenance and practical use.”

### What this means for your portfolio

Your exercise demonstrates structured learning and developing risk-analysis skills.

The review scores embedded in that exercise are not independent certification results.

Do not present the exercise as an enterprise audit or proof that you implemented these outcomes in production.

---

## 5. Ownership: five roles you must distinguish

NIST CSF GV.RR-02 calls for cybersecurity risk-management roles, responsibilities and authorities to be established, communicated, understood and enforced.

For this lesson, use the following working model:

| Role | Main question | Example responsibility |
|---|---|---|
| Risk owner | Who is accountable for managing this exposure in the business context? | Ensures the risk is assessed, treated, monitored and escalated |
| Control owner | Who is accountable for the control’s design, operation and maintenance? | Defines operating expectations, reviews coverage and addresses failures |
| Implementer / operator | Who configures or performs the control? | Implements a DLP rule or operates the review process |
| Tester / reviewer | Who evaluates the evidence against the requirement? | Examines scope and compares expected versus actual behaviour |
| Acceptance authority / approver | Who has delegated authority to accept the remaining risk? | Approves or rejects acceptance within defined limits |

One person may hold several roles where permitted.

A role title alone does not establish authority.

The required reviewer independence depends on the assessment purpose and governance model. An engineer’s functional test is not automatically an independent assurance assessment.

### 5.1 The analyst’s place in this model

An analyst may:

- Clarify scope and risk statements.
- Identify and record owners.
- Coordinate assessments and remediation.
- Challenge missing or contradictory evidence.
- Prepare decision-ready reporting.
- Escalate unresolved exposure.

The analyst is not automatically authorised to:

- Accept risk for the business.
- Approve a policy exception.
- Sign off technical architecture.
- Declare legal compliance.
- Provide independent assurance over their own implementation.

These are boundary checks, not a claim that analysts can never hold delegated authority.

### 5.2 RACI in one minute

| Letter | Meaning | Practical interpretation |
|---|---|---|
| R | Responsible | Performs the work |
| A | Accountable | Owns the result or decision |
| C | Consulted | Supplies input |
| I | Informed | Receives updates |

Define RACI by activity, not by assigning one person as accountable for every aspect of a programme.

For example:

    Activity 1: Implement the rule.
    Activity 2: Validate the control.
    Activity 3: Accept a remaining exception.

These are different activities. They may have different accountable roles.

### Interview-ready summary

> “I would identify the risk owner, control owner, implementer, reviewer and acceptance authority separately. I would not assume that the person configuring a control is also authorised to accept the business risk.”

---

## 6. Evidence: start with the claim, not the screenshot

NIST SP 800-53A uses three assessment methods:

| Method | Meaning | Example |
|---|---|---|
| Examine | Review or inspect relevant assessment objects | Read the configuration, policy, inventory and case records |
| Interview | Discuss responsibilities or operation with relevant people | Ask the control owner how exclusions and failures are reviewed |
| Test | Exercise an activity or mechanism under stated conditions and compare actual with expected behaviour | Attempt a synthetic confidential external email and verify the result |

These methods support a conclusion together. They are not interchangeable guarantees.

An interview helps clarify operation but may need corroboration.

A test supports only what its conditions, scope and result justify.

### 6.1 Match the evidence to the claim

| Claim | Useful evidence | Overstatement to avoid |
|---|---|---|
| A policy has been approved | Versioned document and approval record | “The policy is operating effectively” |
| A rule is configured | Configuration export, mode and rule version | “All intended users are protected” |
| Intended users are covered | Membership/scope reconciliation and exclusions | “No sensitive transfer can occur” |
| A tested transfer was restricted | Test record plus observed result and correlated event data | “All future transfers will be restricted” |
| A process operated over a period | Appropriate samples, tickets, review records and follow-up | “One successful test proves continuous effectiveness” |

For Purview specifically, a rule match can occur without a protective action being performed.

A screenshot of a matched rule is therefore not sufficient on its own to establish that a transfer was blocked.

### 6.2 A practical evidence-quality checklist

This is a teaching checklist informed by assessment principles, not a formally named NIST checklist.

1. Relevant: which requirement and claim does it support?
2. Attributable: which system, source and producer supplied it?
3. Scoped: which users, devices, applications or events does it cover?
4. Time-bound: when was it captured, and what period does it represent?
5. Traceable: can it be linked to the control version, test or issue?
6. Sufficient for the conclusion: are coverage and sampling justified?
7. Reviewable: can an authorised reviewer interpret and check it?
8. Protected: is sensitive evidence stored and shared appropriately?

### Important limitation

Evidence can itself contain client or personal data.

Portfolio examples should use synthetic or safely sanitised material and must not disclose employer-confidential records.

### Key sentence to remember

> Evidence is useful when it supports a specific claim within a defined scope and time period. More screenshots do not automatically mean stronger assurance.

---

## 7. Worked risk record 1 — confidential information sent externally

This is a completed fictional design review, not a test report.

### 7.1 Risk record

| Field | Example record |
|---|---|
| Risk ID | R-EMAIL-01 |
| Asset / objective | Confidential client files; legitimate collaboration without inappropriate disclosure |
| Threat event | An employee sends a confidential file to an unauthorised external recipient |
| Exposure | The relevant sender population is not fully covered by the proposed email restriction |
| Risk statement | Because email protection does not cover all intended senders, confidential client files may be sent to unauthorised recipients, causing disclosure and potential client, contractual or reputational harm |
| Assessment basis | Need sender population, transfer patterns, classification quality, existing controls and impact information; no justified rating available yet |
| Proposed response | Mitigate through a scoped DLP rule, approved transfer route, user guidance and monitoring |
| Proposed risk owner | Designated business process owner |
| Proposed control owner | DLP/email protection service owner |
| Implementer | Authorised messaging/DLP engineer |
| Reviewer | Designated control reviewer; independence appropriate to the assessment purpose |
| Acceptance authority | Role authorised under the risk-acceptance policy; not assumed to be the analyst |
| Status | Design prepared; enforcement and scope validation pending |

### 7.2 Evidence and test design

| Method | Proposed activity |
|---|---|
| Examine | Review the approved requirement, rule conditions, deployment mode, group membership and exclusions |
| Interview | Ask business/control owners how legitimate external collaboration and exceptions are handled |
| Test | An in-scope sender attempts to send a synthetic confidential file externally; observe the actual configured restriction |
| Negative test | Verify that a permitted public document is not restricted by this specific confidential-content rule |
| Boundary test | Verify the pilot’s coverage limit using a controlled out-of-scope sender |

These are proposed activities, not completed tests.

### 7.3 Closure criterion

For the implementation action:

> Agreed scope is reconciled, required tests produce their expected results, review is recorded and operational ownership is confirmed.

### 7.4 Remaining exposure

Unlabelled content, unsupported scenarios, exceptions and non-email channels require separate consideration.

Do not label the overall disclosure risk “closed” just because this implementation action is complete.

### What this example teaches

> The engineer implements the rule. The control owner maintains the control. The risk owner manages the exposure. The acceptance authority decides whether remaining risk can be accepted within its mandate.

---

## 8. Worked risk record 2 — inappropriate cloud-file access

### 8.1 Risk record

| Field | Example record |
|---|---|
| Risk ID | R-SHARING-02 |
| Asset / objective | Confidential files in a collaboration site; access limited to authorised recipients |
| Threat event | A person outside the authorised audience accesses a confidential file |
| Exposure | Sharing settings and sensitivity handling do not reliably prevent inappropriate external access |
| Risk statement | Because sensitive cloud files can be shared beyond the authorised audience, an external person may gain access to client information, causing disclosure and potential harm to clients or the organisation |
| Assessment basis | Need inventory of sensitive files/sites, actual permissions, sharing links, existing controls and recipient context |
| Proposed response | Review permissions and sharing settings; implement appropriate DLP restrictions; establish a controlled external-collaboration process |
| Proposed risk owner | Business service or information owner |
| Proposed control owner | Collaboration service/control owner |
| Implementer | Authorised SharePoint/OneDrive administrator |
| Reviewer | Designated access/control reviewer |
| Acceptance authority | Delegated risk authority for any remaining exposure |
| Status | Assessment and configuration validation pending |

### 8.2 Evidence and test design

| Method | Proposed activity |
|---|---|
| Examine | Review site scope, permissions, relevant links, policy configuration and documented external-sharing requirements |
| Interview | Ask the business owner which external recipients actually need access and why |
| Test | A controlled external test identity attempts access to a synthetic protected file; compare the result with the approved requirement |
| Negative test | Verify that a separate permitted sharing case remains usable |
| Record | Capture identity, file, relevant policy version, timestamp, expected/actual result and reviewer conclusion |

These are proposed activities, not completed tests.

### 8.3 Important distinction

> The file may already be in the cloud. The risk event is inappropriate access, not necessarily another upload from a laptop.

### 8.4 Closure criterion

The identified permission/control deficiency is corrected and verified for the agreed scope.

Existing permissions, links and actual access behaviour have been reviewed. A policy creation screenshot is not the only evidence.

### 8.5 Remaining exposure

A legitimately authorised recipient may still mishandle the information.

A control on sharing does not remove every downstream risk.

The case uses the workload distinctions in Microsoft’s DLP reference. The role assignments and validation approach are illustrative.

### What this example teaches

> Validate the access outcome, not merely the existence of the policy intended to influence that outcome.

---

## 9. Worked risk record 3 — required incident notification is missed

### 9.1 Risk record

| Field | Example record |
|---|---|
| Risk ID | R-NOTIFY-03 |
| Asset / objective | Incident response and communication process; timely, appropriate stakeholder notification |
| Threat event / circumstance | An incident occurs and required recipients are not notified appropriately |
| Exposure | The notification matrix is incomplete or outdated and approval responsibilities are unclear |
| Risk statement | Because communication responsibilities and recipient criteria are unclear, required incident notifications may be delayed or omitted, impairing coordination and potentially breaching applicable obligations |
| Assessment basis | Need incident categories, relevant obligations, existing procedures, contact validity and execution history |
| Proposed response | Define recipient/approval criteria, maintain contacts and templates, exercise the process and review execution |
| Proposed process/risk owner | Designated incident-management business/process owner |
| Control owner | Owner of the incident communication procedure |
| Operators / contributors | Incident response, privacy/legal, communications and business roles according to notification type |
| Reviewer | Designated process reviewer or assessor |
| Acceptance authority | Delegated authority; legal requirements cannot simply be waived by internal acceptance |
| Status | Procedure and execution evidence require review |

### 9.2 Evidence and test design

| Method | Proposed activity |
|---|---|
| Examine | Review the current procedure, escalation matrix, obligation mapping and approval responsibilities |
| Interview | Ask relevant participants to explain who makes each notification decision |
| Exercise | Run a tabletop scenario and record decisions, recipients, approvals and timing |
| Examine actual execution | Where available, review incident records against the applicable requirements and approved process |

These are proposed activities, not completed tests.

### 9.3 Closure criterion

The identified procedure/responsibility gap has been corrected and the agreed exercise or review criteria met.

Any execution shortcomings have owners and follow-up actions.

### 9.4 Limits of the evidence

- A tabletop is not evidence that every real incident was handled correctly.
- No actual incidents during a period does not prove the process is effective or ineffective.
- Do not prescribe one universal notification deadline without the incident type, jurisdiction and legal/contractual context.

This record develops your existing RS.CO-02 exercise while avoiding the assumption that one operational team owns every notification.

### What this example teaches

> A communication procedure, an exercised procedure and evidence of correct handling of a real incident support different claims.

---

## 10. Closure, exceptions and escalation

### 10.1 Three different conclusions

    Implementation action complete
                ≠
    Control validated for an agreed scope
                ≠
    Remaining risk formally accepted

An organisation may track these in linked records rather than one ticket.

The record structure is less important than preserving the meaning of each decision.

Before recommending closure, ask:

1. What exact deficiency or action are we closing?
2. What were the agreed acceptance criteria?
3. What was changed, and for which population?
4. What evidence supports the conclusion?
5. Are exceptions or failed tests still open?
6. Who is authorised to review and approve this closure?
7. What remaining risk or BAU activity continues afterwards?

### 10.2 What a useful exception record contains

A proposed exception template:

- Business need and affected requirement.
- Exact scope and duration.
- Risk scenario and current exposure.
- Alternatives considered.
- Compensating measures, if any, and their validation status.
- Named owner and approving authority.
- Approval decision, conditions and date.
- Review/expiry date and monitoring obligations.
- Escalation route if conditions are breached.

An exception is not a synonym for “the risk disappeared.”

An approval to deviate from a policy is also not automatically a complete risk acceptance. Check how the organisation links these processes.

### 10.3 If no one accepts ownership

Do not assign the risk to yourself merely because you maintain the tracker.

Instead:

1. Record the unresolved ownership.
2. Explain the exposure and affected decision.
3. Escalate through the defined governance route.
4. Follow up on an authorised assignment.
5. Track any agreed interim measures.

Silence is not approval.

NIST CSF supports:

- Explicit responsibilities and authorities: GV.RR-02.
- Risk communication: GV.RM-05.
- Tracked responses: ID.RA-06.
- Managed exceptions: ID.RA-07.

It does not define the exact EY escalation chain.

### Interview-ready summary

> “I would not close an unvalidated action simply because the deadline has arrived. I would report the actual status, missing evidence, remaining exposure, responsible owner and decision needed.”

---

## 11. Interview answers to review aloud

### Question 1: “What is the difference between a risk owner and a control owner?”

> “A risk owner is accountable for managing the relevant exposure in its business context. A control owner is accountable for the design and operation of a particular control.
>
> Technical teams may implement that control, while a separate authority may be needed to accept remaining risk.
>
> I would confirm the actual responsibilities and delegated authority rather than infer them from job titles.”

### Question 2: “What evidence would you request before closing remediation?”

> “I would start with the agreed closure criteria.
>
> Then I would request evidence of what changed, the implementation scope, expected and actual test results, remaining exceptions and the appropriate review.
>
> I would keep the evidence traceable to the control version and date. Completing an implementation action would not automatically mean the overall risk was eliminated or accepted.”

### Question 3: “How would you assess risk?”

> “I would identify the business objective and asset, describe the threat event and relevant exposure, and assess likelihood and impact using the organisation’s methodology.
>
> I would consider existing controls and their actual coverage, record assumptions and uncertainty, then agree the response, owners and escalation path.
>
> Planned controls would inform the target position, not be treated as proven current protection.”

### Question 4: “Can a successful DLP test prove the control is effective?”

> “It supports a conclusion for the conditions and scope tested.
>
> I would still check coverage, exclusions, other relevant scenarios and operational evidence over an appropriate period.
>
> I would also distinguish a recorded rule match from evidence that the intended action actually occurred.”

### Question 5: “Who accepts risk?”

> “The person or body with delegated authority under the organisation’s risk governance model.
>
> The business risk owner may have that authority within certain limits, but I would not assume it.
>
> As an analyst, I would support assessment and prepare a clear recommendation and escalation. I would only approve acceptance if that authority had been explicitly delegated.”

### Question 6: “How does your existing experience relate to this work?”

Use only if accurate for your personal contribution:

> “My background includes coordinating encryption delivery and compliance remediation in a banking environment.
>
> The transferable value is resolving ownership and dependency gaps, working across teams and following actions through to a documented outcome.
>
> I distinguish my coordination and testing contribution from architecture ownership, independent assurance and formal risk acceptance. I would explain the evidence actually used in each case rather than claim authority I did not hold.”

Before using a specific professional story, verify:

- What you personally did.
- What others did.
- What was approved.
- What evidence you actually saw.

Do not infer test results or sign-off authority from the fact that a project reached production.

---

## 12. Knowledge check with answers

Pause before reading each answer. No written work is necessary.

### 1. Is “missing DLP” a complete risk statement?

No.

Add the asset, event, exposure and business consequences, then assess likelihood and impact using the relevant method.

### 2. Does the engineer who configures a rule automatically own the risk?

No.

Implementation responsibility, control ownership, business risk ownership and acceptance authority are distinct responsibilities.

### 3. Does an approved procedure prove it was followed?

No.

Review execution evidence such as case records, notifications, timestamps and decisions appropriate to the claim.

### 4. Does a DLP match prove an email was blocked?

No.

Verify the policy mode, applicable action and observed outcome.

### 5. Can a planned control justify reporting a lower current residual risk?

Not simply because it is planned.

Distinguish the target position from the current evidence-supported assessment.

### 6. Does NIST dictate that the SOC owns every incident notification?

No.

CSF defines outcomes, not one universal role allocation. Confirm the documented decision and communication model.

### 7. Is a policy exception the same as eliminating risk?

No.

It can leave or increase exposure. Record the rationale, scope, decision authority, conditions, duration and review.

### 8. What if a remediation deadline has passed but validation is missing?

Report the unvalidated exposure and overdue action honestly, with an owner and escalation.

Do not mark effectiveness as confirmed just to meet the date.

### 9. What if the control owner confirms that everything works?

Use the interview to understand the process, then obtain corroborating records or tests appropriate to the assessment.

An assertion and a demonstrated result are not the same thing.

### 10. What should you be able to say after this session?

> “I can describe a risk in its business context, distinguish the risk owner from the implementer, and identify evidence supporting a particular conclusion.
>
> I can explain the difference between current and target risk, recognise the limits of a test, and distinguish remediation closure from risk acceptance.”

---

## 13. Final one-minute recap

> “I would start with the business objective, asset and risk scenario, not the tool.
>
> I would identify who owns the risk, who owns the control, who implements it and who can approve the remaining exposure.
>
> Then I would define the evidence needed to support the conclusion: relevant records, interviews and tests with clear scope and dates.
>
> I would track unresolved gaps and avoid treating deployment, a successful isolated test or a closed ticket as proof that all risk has disappeared.”

### What completing this session establishes

After reviewing these notes, you have studied:

- Risk statements and business consequences.
- Inherent, current residual and target risk.
- Risk appetite and tolerance.
- Risk ownership, control ownership and decision authority.
- Examine, interview and test as assessment methods.
- Evidence quality and limits of conclusions.
- Three fictional risk-register examples.
- Remediation closure, exceptions and escalation.
- Interview answers connecting these concepts to your existing experience.

An accurate portfolio description is:

> Reviewed risk ownership and evidence principles using my existing NIST exercises and three fictional risk-register examples. Studied how to distinguish implementation, validation and acceptance decisions.

This is conceptual preparation and review of worked examples.

It is not evidence of a production risk assessment, an audit, executed control testing or an actual risk-acceptance decision.

---

## 14. How the learning sources are used

The source links below are optional follow-up reading. You do not need to open them to complete the core 60-minute session.

### 14.1 Materials carried forward from the Excel plan

| Material | Role in this lesson | Limitation |
|---|---|---|
| EY preparation workbook | Lesson structure, competency gaps and interview objective | A preparation aid, not a control standard |
| Your NIST subcategory exercise — workbook S5 | Original interpretations of RS.CO-02, ID.AM-05 and GV.RM-02 | Learning evidence, not an authoritative interpretation or independent assessment |
| Microsoft DLP policy reference — workbook S8 | Policy scope, conditions, actions and recorded matches | Product-specific guidance, not evidence of EY’s architecture |
| Your CV — workbook S2 | Context for connecting the lesson to encryption delivery and compliance remediation | Does not establish specific approval authority or exactly which tests you personally performed |

The workbook’s tutoring register, ISO introduction and Microsoft lab summary remain useful background. They are not all necessary sources for this focused session.

#### EY preparation workbook

[Open the preparation workbook](https://docs.google.com/spreadsheets/d/1PrrRVcf_f_G_TfsFU3kIh7nLjZsSgLwk/edit)

Use it to connect the lesson to:

- `03_Plan`: risk, owner and evidence.
- G01: risk → control → evidence.
- G06: ownership and risk acceptance.
- G04: validation and closure.

#### Your existing NIST exercise

[Open the NIST subcategory analysis](https://github.com/tkedzia/tomasz-kedzia-cybersecurity-journey/blob/main/NIST-Cybersecurity-Framework/Assignments/NIST-CSF-Subcategory-Analysis%3A-Risks%2C-Evidence%2C-and-Ownership.md)

Use it to compare your earlier answers with the refinements in Section 4.

The scores and review comments in that file are not independent certification results.

#### Microsoft Purview DLP policy reference

[Open the DLP policy reference](https://learn.microsoft.com/en-us/purview/dlp-policy-reference)

Use it to understand:

- Policy scope.
- Conditions and actions.
- Deployment modes.
- Recorded matches versus actions performed.

### 14.2 Primary guidance added to strengthen the lesson

#### NIST Cybersecurity Framework 2.0

[Open the final NIST CSF 2.0 publication](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf)

Relevant outcomes:

| Outcome | Connection to the lesson |
|---|---|
| GV.RM-02 | Risk appetite and tolerance |
| GV.RM-05 | Communication about cybersecurity risk |
| GV.RR-02 | Roles, responsibilities and authorities |
| ID.AM-05 | Asset prioritisation |
| ID.RA-05 | Threats, vulnerabilities, likelihood and impact |
| ID.RA-06 | Choosing, planning, tracking and communicating responses |
| ID.RA-07 | Managing changes and exceptions |
| RS.CO-02 | Incident notification |

The framework defines outcomes. It does not prescribe one universal ownership model or EY-specific procedures.

#### NIST SP 800-53A Rev. 5

[Open Assessing Security and Privacy Controls in Information Systems and Organizations](https://csrc.nist.gov/pubs/sp/800/53/a/r5/final)

Use it for the assessment-method distinction:

- Examine.
- Interview.
- Test.

This lesson uses those methods as a learning structure. It does not constitute a formal assessment conducted under the publication.

#### NIST IR 8286A Rev. 1

[Open Identifying and Estimating Cybersecurity Risk for Enterprise Risk Management](https://csrc.nist.gov/pubs/ir/8286/a/r1/final)

Use it for documenting risk scenarios involving:

- Assets.
- Threats.
- Vulnerabilities.
- Likelihood.
- Impact.
- Enterprise risk-register context.

The example records in this lesson are teaching templates, not prescribed NIST forms.

#### NIST glossary

- [Risk](https://csrc.nist.gov/glossary/term/risk)
- [Risk appetite](https://csrc.nist.gov/glossary/term/Risk_Appetite)
- [Risk tolerance](https://csrc.nist.gov/glossary/term/risk_tolerance)
- [Residual risk](https://csrc.nist.gov/glossary/term/residual_risk)

The glossary aggregates definitions from different publications. Interpret each definition in the context of its cited source and the organisation’s chosen methodology.
```
