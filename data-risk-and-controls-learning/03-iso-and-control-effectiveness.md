# Study Notes — 10 November 2026

## ISO and Control Effectiveness

These notes cover the ISO and control-testing session from the EY interview preparation plan: understanding the relationship between ISO/IEC 27001 and ISO/IEC 27002, connecting risk treatment to control selection, understanding the Statement of Applicability, and evaluating whether a control works.

Prepared: 12 November 2026. The title refers to the session date in the learning plan, not the date on which the material was prepared or reviewed.

Study time: approximately 60 minutes. No lab, additional software or written assignment is required. The examples are fictional and are not descriptions of EY’s environment or a banking client’s actual controls.

| Time | Read and review |
|---|---|
| 0–10 minutes | Sections 1–3: ISMS, ISO 27001 and ISO 27002 |
| 10–25 minutes | Sections 4–6: risk treatment, applicability and effectiveness |
| 25–50 minutes | Sections 7–10: completed DLP test plan, evidence and findings |
| 50–60 minutes | Sections 11–14: interview answers, knowledge check and recap |

The core reading includes all explanations needed for the session. Source links at the end are optional follow-up reading.

---

## 1. The central idea

An information security management system connects business needs and risks with decisions, controls, responsibilities, evidence and improvement.

It is not simply a collection of policies.

A useful mental model is:

    BUSINESS CONTEXT AND REQUIREMENTS
                    ↓
    INFORMATION SECURITY RISK ASSESSMENT
                    ↓
    RISK TREATMENT AND NECESSARY CONTROLS
                    ↓
    STATEMENT OF APPLICABILITY
                    ↓
    IMPLEMENTATION AND OPERATION
                    ↓
    MONITORING, ASSESSMENT AND REVIEW
                    ↓
    CORRECTIVE ACTION AND IMPROVEMENT

The central control question is not:

> “Do we have a DLP product?”

It is:

> “Have we selected an appropriate response to the risk, implemented it in the required scope, and obtained evidence supporting its effectiveness?”

### Connection to the Excel plan

This lesson develops:

- G09: ISO in practice.
- G04: control effectiveness and evidence-based closure.
- G01: risk → control → evidence.
- Supporting G06: ownership and approval boundaries.

The expected outcome is to explain one complete example:

    Risk
      ↓
    Treatment decision
      ↓
    Necessary control
      ↓
    Applicability rationale
      ↓
    Implementation
      ↓
    Evidence and conclusion

### Key sentence to remember

> A documented requirement, an implemented control and an effective control are different things.

---

## 2. What an ISMS is

ISMS stands for Information Security Management System.

ISO/IEC 27001 specifies requirements for establishing, implementing, maintaining and continually improving an ISMS.

The management system provides a structured way to manage information security risks in the organisation’s context.

It involves people, processes and technology.

Examples of management-system activities include:

- Defining the scope.
- Understanding relevant requirements.
- Assigning responsibilities.
- Assessing information security risks.
- Planning and carrying out risk treatment.
- Establishing objectives.
- Maintaining necessary documented information.
- Evaluating performance.
- Conducting internal audits.
- Performing management reviews.
- Addressing nonconformities and improving the system.

### 2.1 Scope matters

An ISMS operates within a defined scope.

That scope affects which organisational activities, locations, systems and interfaces are covered by the management system.

For example:

> A certification covering a defined service does not automatically establish that every activity across the whole organisation falls within that certification scope.

A control test also has a scope, but it is not the same thing as the ISMS scope.

Our DLP test might cover:

- One email service.
- One pilot group.
- One policy version.
- A defined set of test scenarios.
- A particular test period.

Do not confuse:

    ISMS scope
        ≠
    Certification scope details
        ≠
    Individual control scope
        ≠
    Test scope

The scopes should be understood and connected, not assumed to be identical.

### 2.2 Certification is not a guarantee of zero incidents

ISO/IEC 27001 certification concerns conformity of the defined ISMS with the standard’s requirements.

It does not mean that the organisation is impossible to compromise.

It does not prove that every control is perfect in every situation.

It does not remove the need for ongoing monitoring, assessment and improvement.

### Interview-ready summary

> “I understand an ISMS as the management framework connecting business context, information security risks, responsibilities, controls and continual improvement. It is broader than technical configuration or a policy library.”

---

## 3. ISO/IEC 27001 versus ISO/IEC 27002

| Topic | ISO/IEC 27001 | ISO/IEC 27002 |
|---|---|---|
| Main purpose | Specifies requirements for an ISMS | Provides guidance on information security controls |
| Main question | What must the management system address? | What can these controls involve, and how can they be implemented? |
| Relationship to certification | An organisation’s ISMS can be certified against ISO/IEC 27001 | ISO/IEC 27002 is guidance, not the equivalent ISMS certification standard |
| Practical use | Establish and evaluate the management system | Inform the design and implementation of controls |

### 3.1 Annex A is a reference set, not the whole ISMS

ISO/IEC 27001 contains management-system requirements and an Annex A reference set of information security controls.

Do not reduce ISO/IEC 27001 to Annex A.

Also, do not assume that every Annex A control must be implemented in exactly the same way by every organisation.

The organisation determines necessary controls through its risk-treatment process and compares them with Annex A to check that necessary controls have not been omitted.

Necessary controls may also come from other sources or be organisation-specific.

### 3.2 What this means for a DLP example

ISO does not tell our fictional organisation to create one particular Microsoft Purview rule with one particular group name.

Instead, the organisation must determine what is necessary to manage its information security risks and requirements.

Product configuration is one implementation layer beneath that decision.

### Key distinction

> ISO/IEC 27001 specifies ISMS requirements. ISO/IEC 27002 provides control guidance. Neither is a substitute for understanding the actual business risk and implementation context.

---

## 4. Risk treatment: deciding what to do about the risk

Risk treatment follows assessment and evaluation.

The organisation decides how to respond, determines necessary controls, plans implementation and obtains appropriate approvals.

Depending on the methodology and circumstances, responses may include:

- Reducing or modifying the risk through controls.
- Avoiding the activity creating the risk.
- Sharing or transferring certain consequences through appropriate arrangements.
- Retaining the risk through an authorised decision.

These options do not automatically remove legal or contractual obligations.

### 4.1 Worked risk

> “An employee may send confidential client information to an unauthorised external recipient, causing inappropriate disclosure and potential client, contractual or reputational harm.”

### 4.2 Worked treatment decision

> “Reduce inappropriate external disclosure while maintaining an approved route for legitimate client collaboration.”

Notice that this is not the same as:

> “Buy DLP.”

A product purchase is not a complete treatment decision.

### 4.3 Necessary controls in the example

A fictional treatment approach might include:

- Classification and handling expectations.
- Appropriate access to the source information.
- An external-email restriction for recognised confidential content.
- An approved secure-sharing route.
- User guidance.
- A managed exception process.
- Monitoring and review of failures.
- Change control and periodic validation.

The DLP rule is one part of a wider response.

### 4.4 A useful treatment plan

A practical treatment plan should make the work and decisions traceable.

For this example, it could record:

| Field | Example |
|---|---|
| Risk reference | R-EMAIL-01 |
| Treatment objective | Reduce inappropriate external disclosure |
| Required changes | Configure scoped email protection and approved collaboration route |
| Owners | Named business risk owner and control owner |
| Implementation actions | Configuration, scope reconciliation, communication and operational handover |
| Dependencies | Classification quality, group membership, technical capabilities and approval |
| Due dates | Defined and tracked |
| Validation | Agreed test cases and review criteria |
| Remaining exposure | Documented and assessed |
| Approval | Appropriate risk-owner and governance decisions |

This is an illustrative working template, not a mandatory ISO form.

---

## 5. The Statement of Applicability

The Statement of Applicability is commonly abbreviated to SoA.

Think of it as a documented explanation of the controls necessary for the ISMS and their applicability.

It is not simply:

- A list of risks.
- A project plan.
- A product inventory.
- A claim that every control is effective.
- A checklist where every Annex A control must be marked “implemented.”

### 5.1 What it needs to communicate

Under ISO/IEC 27001, the SoA addresses:

- Necessary controls.
- Justification for their inclusion.
- Whether the necessary controls are implemented.
- Justification for excluding Annex A controls.

The organisation compares its necessary controls with Annex A to check for omissions.

If necessary controls come from outside Annex A, they still need to be addressed in the SoA.

### 5.2 Illustrative SoA entry

The example below is a teaching format. It is not an official ISO template or a complete SoA.

| Field | Illustrative entry |
|---|---|
| Control reference | Organisational control DP-01, mapped to the relevant Annex A reference |
| Control description | Restrict inappropriate external transfer of confidential client information |
| Necessary? | Yes |
| Inclusion rationale | Supports treatment of external-disclosure risk and relevant handling requirements |
| Implementation status | Partially implemented: regional email pilot only |
| Implementation reference | DLP policy configuration, scope definition and operating procedure |
| Owner | Designated data-protection control owner |
| Related risk | R-EMAIL-01 |
| Supporting evidence reference | Test plan and evidence record |
| Known limitation | Other channels and excluded populations require separate treatment |

Some fields above, such as the owner and evidence links, are useful additions rather than a claim that ISO mandates this exact table.

### 5.3 Applicability is not effectiveness

A control may be necessary but not yet implemented.

A control may be implemented but insufficiently validated.

A control may operate but have known coverage gaps.

Therefore:

    Necessary
        ≠
    Implemented
        ≠
    Effective across the required scope

### 5.4 Excluding a control requires a reason

Do not justify an exclusion simply by saying:

> “We do not own the relevant product.”

Control applicability concerns the organisation’s requirements and risks, not whether a particular product has been purchased.

Likewise, a control being performed by a supplier does not automatically mean it is irrelevant to the organisation’s ISMS.

### Interview-ready summary

> “The SoA connects necessary controls and applicability decisions with the risk-treatment approach. It explains inclusion and exclusion decisions and records implementation status. I would not treat the SoA itself as proof of operating effectiveness.”

---

## 6. Three questions about control effectiveness

For practical analysis, separate design, implementation and operation.

This is a useful assessment structure, not a claim that ISO requires these exact three labels.

### 6.1 Design: can the control address the risk?

Ask:

> “If implemented and operated as intended, would this control reasonably address the stated risk within its defined scope?”

For the email example:

- Does the rule recognise the relevant information?
- Does it consider external recipients?
- Is the configured response appropriate?
- Are exceptions controlled?
- Is there a legitimate business route?
- Does the design address the intended population?

A design gap might be:

> The rule only detects a label, but there is no reliable process for labelling the documents the requirement is intended to protect.

### 6.2 Implementation: is it actually in place?

Ask:

> “Was the agreed design implemented in the required scope?”

Check:

- Actual rule settings.
- Deployment mode.
- Included and excluded users.
- Relevant groups and locations.
- Technical prerequisites.
- Ownership and operating instructions.

An implementation gap might be:

> The rule design covers the region, but the deployed group excludes part of the intended population.

### 6.3 Operation: does it work as intended in practice?

Ask:

> “Does the control perform its intended function under relevant conditions, and is there evidence of its operation over the assessed period?”

Check:

- Observed test results.
- Relevant operational records.
- Exception handling.
- Failure investigation.
- Reviews and changes.
- Continuing scope accuracy.

An operating gap might be:

> The rule restricts a test transfer, but operational failures are not reviewed or assigned for follow-up.

### Summary

| Dimension | Main question | Example evidence |
|---|---|---|
| Design | Is the proposed mechanism appropriate? | Requirement, risk linkage, rule logic and documented assumptions |
| Implementation | Is the mechanism in place in the required scope? | Configuration, mode, membership and deployment records |
| Operation | Does it perform as intended? | Test outcomes, operational samples, review records and follow-up |

A successful test in one dimension does not automatically answer every other question.

---

## 7. Completed DLP control specification

This is a fictional control design used throughout the remaining sections.

### 7.1 Control objective

> Restrict unauthorised external email sharing of recognised confidential client information by users in the agreed pilot scope, while preserving approved business collaboration.

### 7.2 Control specification

| Component | Proposed design |
|---|---|
| Control ID | DP-EMAIL-01 |
| Location | Corporate email service |
| Protected content | Content recognised through agreed sensitivity-label or detection criteria |
| Population | Defined pilot sender group |
| Trigger | Relevant confidential content addressed to an external recipient |
| Expected response | Configured external-sharing restriction without routine user override |
| User guidance | Explain the restriction and approved alternative |
| Reporting | Record matches and observed actions; generate configured alerts |
| Exceptions | Approved, scoped, time-bound and reviewed |
| Owner | Named DLP/email control owner |
| Operation | Review relevant failures, changes and exceptions |
| Validation | Test rule logic, scope, actual response and legitimate use cases |

### 7.3 Assumptions to record

- The required labels or detectors exist.
- The test content is synthetic.
- The test users and recipients are controlled.
- The selected location supports the intended conditions and actions.
- Necessary permissions and prerequisites are in place.
- Other policies may affect the final outcome.
- The pilot does not cover every leakage channel.

### Important limitation

> This specification is a proposed design. It does not prove that a policy has been configured or that enforcement has succeeded.

---

## 8. Completed test plan

The test plan below specifies what would be tested and how results would be interpreted.

No tests have been executed as part of these notes.

### 8.1 Test objective

> Establish whether the configured control produces the expected result for the agreed scenarios and identify scope or behaviour limitations.

### 8.2 Preconditions

Before executing tests:

1. Obtain authorisation.
2. Confirm test scope.
3. Record the policy version and deployment mode.
4. Confirm the identities and group memberships.
5. Prepare synthetic test files.
6. Confirm the permitted external test recipient.
7. Record relevant interacting policies.
8. Agree expected outcomes and evidence requirements.
9. Confirm how test-generated alerts will be handled.

### 8.3 Test cases

| ID | Scenario | Expected result | Purpose |
|---|---|---|---|
| T01 | In-scope sender attempts external email with recognised confidential content while enforcement is enabled | The configured restriction occurs | Validate the core protective action |
| T02 | In-scope sender emails a permitted public document externally | This confidential-content rule does not restrict it | Check legitimate business activity |
| T03 | In-scope sender emails confidential content internally | This external-sharing rule does not trigger solely because content is confidential | Check recipient-context logic |
| T04 | Out-of-scope sender performs the same external activity | This pilot policy does not supply coverage | Demonstrate the scope boundary |
| T05 | In-scope sender sends confidential content that does not meet the configured detector or label condition | The result follows actual detection capability; any gap is documented | Examine detection limitations |
| T06 | A user attempts a routine override where the design does not permit one | No routine override is available | Validate the configured override behaviour |
| T07 | A matching event occurs in simulation | Simulated results are recorded; do not conclude an enforcement block occurred | Distinguish simulation from enforcement evidence |
| T08 | A controlled approved-sharing scenario uses the agreed alternative route | The authorised business process remains usable | Validate the wider treatment design |

These are example expected results, not universal Purview behaviour.

Verify the exact configuration and location-specific semantics before execution.

### 8.4 Positive and negative tests

In this lesson:

- A positive test checks a scenario expected to trigger the control.
- A negative test checks a scenario expected not to trigger that specific control.

A negative test is not the same as a failed test.

For example:

> A public brochure passing through without this rule blocking it can be a successful negative test.

### 8.5 Boundary tests

Boundary tests explore the edges of the control.

Examples include:

- An excluded sender.
- A missing label.
- An unsupported activity.
- A different sharing channel.

An expected lack of coverage outside the pilot may pass the boundary test while still revealing exposure that matters to the broader rollout.

Therefore:

> “The test behaved as expected” is not the same as “the whole business requirement is covered.”

### 8.6 Record actual results

For each executed test, capture:

- Test ID.
- Date and time.
- Test identity and relevant scope.
- Synthetic content characteristics.
- Policy version and mode.
- Expected result.
- Observed result.
- Relevant event identifiers.
- Pass, fail or inconclusive status.
- Reviewer and follow-up.

If the evidence is insufficient, use “inconclusive” rather than inventing a pass.

---

## 9. From testing to an evidence-supported conclusion

NIST SP 800-53A distinguishes examine, interview and test as assessment methods.

Use them together where appropriate.

### 9.1 Examine

Review:

- The requirement and risk linkage.
- The configuration.
- The included population and exclusions.
- Deployment mode.
- Change records.
- Operating procedures.
- Relevant review and exception records.

### 9.2 Interview

Ask the control owner:

- How do you know the scope remains accurate?
- Who reviews failures?
- How are exceptions approved and expired?
- What happens when classification is wrong?
- How are changes tested?
- Which scenarios are not covered?

An explanation may require corroboration.

### 9.3 Test

Observe relevant behaviour under controlled conditions.

Compare actual and expected outcomes.

For DLP:

> A recorded rule match does not, by itself, prove that the intended protective action occurred.

Check mode, action and observed outcome.

### 9.4 One successful test is not a period of operation

A controlled test supports a point-in-time conclusion within its tested conditions.

To evaluate operation over a period, consider evidence such as:

- Relevant operational events.
- Review records.
- Changes and retesting.
- Exception lifecycle records.
- Failure resolution.
- Scope maintenance.

Define the period and justify sampling.

There is no universal sample size that proves effectiveness for every control.

### 9.5 Example of a careful conclusion

> “The configured restriction operated as expected in the completed test scenarios for the pilot group and recorded policy version. This supports the tested behaviour within that scope. It does not establish coverage for excluded users, unrecognised content, other channels or all activity over a longer period.”

### Weak conclusion to avoid

> “DLP works, so the data is secure.”

---

## 10. Findings, corrective action and retesting

Suppose a test produces an unexpected result.

Do not jump immediately from “failed test” to a universal claim about the entire ISMS.

First establish what happened.

### 10.1 Worked finding

Fictional observation:

> A sender intended to be covered by the pilot successfully performs an external transfer that the test expected to restrict.

Initial analysis:

- Was the sender actually in scope?
- Did the file match the intended condition?
- Was the policy in enforcement mode?
- Was the action configured correctly?
- Did another setting affect the outcome?
- Is the observed evidence reliable?

### 10.2 Describe the finding clearly

| Element | Example |
|---|---|
| Requirement | Intended pilot senders must receive the specified restriction for recognised confidential content |
| Observation | One intended sender was excluded from the deployed group |
| Evidence | Membership reconciliation and controlled test record |
| Exposure | The sender does not receive the expected protection from this policy |
| Owner | Named control owner |
| Action | Correct membership and investigate the provisioning process |
| Validation | Repeat the affected test and assess related population coverage |

### 10.3 Correction versus corrective action

A correction fixes the detected problem.

A corrective action addresses its cause to prevent recurrence.

In the example:

- Correction: add the missing user.
- Corrective action: fix the process that caused intended users to be omitted.
- Verification: repeat the test and check the relevant population or process.

Do not assume the root cause before investigation.

### 10.4 Closure

A useful closure decision addresses:

1. The requirement.
2. The observed deficiency.
3. The correction or treatment.
4. Supporting evidence.
5. Retest results.
6. Broader implications.
7. Remaining exposure.
8. Appropriate review and approval.

A closed issue does not automatically establish that all related risk has disappeared.

### Important distinction

Whether a deficiency constitutes an ISO nonconformity depends on the applicable requirement and objective evidence in the assessment context.

Do not label every technical defect a certification failure.

---

## 11. Monitoring, audit and management review

These activities have different purposes.

| Activity | Main purpose | Example |
|---|---|---|
| Operational monitoring | Observe performance and identify issues in ongoing operation | Review DLP failures, alerts, exclusions and exception expiry |
| Control testing | Evaluate specified behaviour against defined expectations | Execute controlled synthetic transfer tests |
| Internal audit | Evaluate conformity and effective implementation of the ISMS within its programme and scope | Review traceability from requirements to operation and evidence |
| Management review | Evaluate the ISMS at management level and determine relevant decisions and changes | Review performance, issues, risks, resources and improvement needs |

One operational dashboard does not replace all these activities.

Likewise, an internal audit does not replace daily control ownership.

### 11.1 Metrics need interpretation

Possible implementation metric:

> Percentage of the intended sender population included in the configured scope.

Possible validation metric:

> Percentage of required test scenarios completed with the expected results.

Possible operational indicators:

- Unresolved control failures.
- Overdue exceptions.
- Scope discrepancies.
- Time taken to investigate relevant alerts.
- Repeated problems after changes.

These are illustrative indicators, not mandatory ISO metrics.

### 11.2 Misleading conclusions

Avoid:

> “There were no alerts, so there were no leaks.”

Also avoid:

> “All users are in scope, so the control is effective.”

Coverage, detection, protective action and operational follow-up are related but different.

### Interview-ready summary

> “I would report implementation coverage, validation results and unresolved exposure separately. I would also explain the measurement period and limitations rather than present one percentage as proof of security.”

---

## 12. Interview answers to review aloud

### Question 1: “What is the difference between ISO 27001 and ISO 27002?”

> “ISO 27001 specifies requirements for an information security management system. ISO 27002 provides guidance on information security controls.
>
> I would use ISO 27001 to understand the management-system expectations and ISO 27002 to inform appropriate control implementation. Neither replaces the need to understand the organisation’s risks and requirements.”

### Question 2: “What is a Statement of Applicability?”

> “The Statement of Applicability records necessary controls, why they are included, their implementation status, and justifications for excluding Annex A controls.
>
> It connects control applicability with risk treatment. I would not treat it as a risk register or as proof that every listed control operates effectively.”

### Question 3: “Must every Annex A control be implemented?”

> “Not automatically. The organisation determines necessary controls through its treatment process and compares them with Annex A to check for omissions.
>
> Exclusions need appropriate justification, and necessary controls can also come from outside Annex A.”

### Question 4: “How would you assess control effectiveness?”

> “I would start with the objective and scope. Then I would assess whether the design addresses the risk, whether it was implemented as intended, and whether evidence supports its actual operation.
>
> I would combine document and configuration review, discussions with relevant owners, and appropriate testing. I would state the period, coverage and limitations of the conclusion.”

### Question 5: “How would you test a DLP control?”

> “I would agree the requirement, scope, policy version, mode and expected outcomes.
>
> I would use synthetic content and controlled identities to test a scenario expected to trigger the restriction, a permitted scenario, and relevant scope or detection boundaries.
>
> I would capture observed actions and correlated records rather than rely only on rule matches.”

### Question 6: “What if a test fails?”

> “I would verify the evidence, determine the affected requirement and scope, and investigate the cause.
>
> I would assign the issue to the appropriate owner, track correction and any necessary corrective action, then retest. I would report remaining exposure honestly rather than close the issue solely because the deadline had arrived.”

### Question 7: “What is your practical ISO experience?”

Use only if accurate:

> “I have completed ISO 27001 training, but I have not led an ISO implementation or certification audit.
>
> My related professional experience is in data-protection delivery, testing and compliance remediation. I am developing the ability to connect those activities more explicitly to risk treatment, control applicability and evidence-supported conclusions.”

Do not describe this reading session as implementation or audit experience.

---

## 13. Knowledge check with answers

Pause before reading each answer. No written work is necessary.

### 1. Is an ISMS just a set of policies?

No.

It is a management system involving context, responsibilities, risks, controls, evaluation and improvement.

### 2. Does ISO 27002 provide the same certification basis as ISO 27001?

No.

ISO 27001 specifies ISMS requirements. ISO 27002 provides control guidance.

### 3. Is Annex A the whole of ISO 27001?

No.

The management-system requirements are broader than the reference control set.

### 4. Is the SoA a risk register?

No.

It addresses necessary controls, inclusion rationale, implementation status and justified exclusions.

### 5. Does a control being applicable mean it is effective?

No.

Necessity, implementation and effectiveness are different conclusions.

### 6. Is a negative test a failed test?

No.

A negative test checks a scenario expected not to trigger a particular control.

### 7. Does a successful simulated match prove enforcement?

No.

Verify the deployment mode and actual protective action.

### 8. Can one successful test prove continuous operation?

No.

It supports a conclusion for the tested conditions. A period-based conclusion needs appropriate supporting evidence.

### 9. Is adding one missing user always sufficient corrective action?

Not necessarily.

It may correct the immediate issue. Investigate whether the process that caused the omission also needs improvement.

### 10. Does a certificate mean the organisation cannot suffer an incident?

No.

Certification does not guarantee zero incidents or flawless operation of every control.

### 11. Does using a supplier automatically make a control irrelevant?

No.

Outsourced operation does not automatically remove the organisation’s need to address the control and related responsibilities.

### 12. What is the most important conclusion boundary?

> State exactly what the evidence supports, within which scope and period, and what remains unverified.

---

## 14. Final one-minute recap

> “ISO 27001 defines requirements for an information security management system, while ISO 27002 provides control guidance.
>
> I would connect the business risk to a treatment decision, determine necessary controls, and record applicability decisions and implementation status in the SoA.
>
> To evaluate a control, I would distinguish design, implementation and operation. I would agree test criteria, use appropriate evidence and state the limits of the conclusion.
>
> A configured rule, a simulated match, a successful isolated test and effective ongoing operation are not the same thing.
>
> If a deficiency is found, I would track correction, investigate the cause where appropriate, retest and report remaining exposure.”

### What completing this session establishes

After reviewing these notes, you have studied:

- The purpose of an ISMS.
- ISO 27001 versus ISO 27002.
- Risk treatment and necessary controls.
- The Statement of Applicability.
- Design, implementation and operation.
- A fictional DLP control and completed test-plan design.
- Evidence, findings, corrective action and retesting.
- Monitoring, internal audit and management review.
- Interview explanations and their limitations.

An accurate portfolio description is:

> Reviewed ISO 27001 and ISO 27002 concepts and a fictional DLP control-testing example. Studied how to connect risk treatment, control applicability, implementation and evidence-supported effectiveness conclusions.

This is conceptual preparation and review of worked examples.

It is not evidence of a production deployment, an ISO implementation, a certification audit or executed DLP tests.

---

## 15. How the learning sources are used

The links below are optional follow-up reading. They are not additional tasks required to complete the core session.

### 15.1 Materials carried forward from the preparation plan

#### EY preparation workbook

[Open the preparation workbook](https://docs.google.com/spreadsheets/d/1PrrRVcf_f_G_TfsFU3kIh7nLjZsSgLwk/edit)

The lesson follows the original ISO and control-testing session:

- Recall.
- ISO overview.
- DLP test plan.
- English interview answers.

The workbook is a preparation aid, not an authoritative control standard.

#### Microsoft Learn — ISO 27001 overview

[Open the ISO 27001 overview used in the plan](https://learn.microsoft.com/en-us/azure/compliance/offerings/offering-iso-27001)

Useful for orientation, but not a substitute for the full standard or a detailed conformity assessment.

#### Microsoft Purview DLP policy reference

[Open the DLP policy reference](https://learn.microsoft.com/en-us/purview/dlp-policy-reference)

Used for:

- Policy scope.
- Conditions and actions.
- Location-specific behaviour.
- Deployment modes.
- The distinction between matches and actions performed.

#### Previous learning notes

[Data Leakage and DLP](./01-data-leakage-and-dlp.md)

[Risk, Ownership and Evidence](./02-risk-ownership-and-evidence.md)

These provide the fictional policy and risk context used here.

### 15.2 Primary and supporting guidance

#### ISO/IEC 27001

[Open the official ISO 27001 overview](https://www.iso.org/standard/27001)

Used for the purpose of the ISMS and the risk-based management-system approach.

These notes paraphrase concepts. They do not reproduce the full standard or provide a complete implementation checklist.

#### ISO/IEC 27002

[Open the official ISO 27002 overview](https://www.iso.org/standard/75652.html)

Used for the distinction between ISMS requirements and control guidance.

#### ISO/IEC JTC 1/SC 27 — Statement of Applicability discussion

[Open the SC 27 journal containing SoA auditing-practice discussion](https://committee.iso.org/files/live/sites/jtc1sc27/files/resources/ISO-IEC%20JTC%201-SC%2027_N23202_SC27%20Journal%20Vol%203%20Issue%204.pdf)

Used to clarify:

- Necessary controls.
- Comparison with Annex A.
- Justification of exclusions.
- Controls sourced outside Annex A.

The journal is supporting interpretation, not a replacement for the standard’s normative requirements.

#### NIST SP 800-53A Rev. 5

[Open Assessing Security and Privacy Controls](https://csrc.nist.gov/pubs/sp/800/53/a/r5/final)

Used for:

- Examine.
- Interview.
- Test.
- Evidence-supported control assessment.

NIST assessment methods are used here as a practical learning structure. They are not presented as an ISO-mandated test template.

#### Microsoft Purview — create and deploy a DLP policy

[Open the deployment guidance](https://learn.microsoft.com/en-us/purview/dlp-create-deploy-policy)

Used for the incremental approach to policy evaluation and deployment.

The test cases, ownership model, SoA entry, metrics and closure examples in this lesson are fictional teaching designs. They are not prescribed ISO forms or confirmed EY procedures.
