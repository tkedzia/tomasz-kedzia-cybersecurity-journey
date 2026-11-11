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

A well-written statement alone does not establish the likelihood, impact or existence of a vulnerability. Record the supporting facts and
