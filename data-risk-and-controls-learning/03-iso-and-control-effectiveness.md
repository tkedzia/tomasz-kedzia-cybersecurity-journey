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
