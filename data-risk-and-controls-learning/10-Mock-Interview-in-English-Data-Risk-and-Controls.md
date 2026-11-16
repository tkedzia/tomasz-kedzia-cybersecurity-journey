# Study Notes
## Mock Interview in English: Data Risk and Controls

Plan session: 17 November 2026  
Estimated study time: 60 minutes  
Format: timed rehearsal, worked answers and self-review

Today’s objective is to bring the earlier lessons together into clear interview answers.

You are not trying to demonstrate expertise in every security technology. You are practising how to:

- Explain your professional relevance.
- Describe your own contribution precisely.
- Approach an unfamiliar control scenario logically.
- Distinguish implementation from verified effectiveness.
- Recognise decisions outside your authority.
- State experience boundaries without dismissing your transferable skills.

The questions below are training questions, not a disclosed EY interview script.

---

## 1. Session Structure

| Time | Activity | Output |
|---|---|---|
| 0–5 minutes | Setup and review of answer structure | Ready to rehearse |
| 5–40 minutes | Mock interview | One uninterrupted attempt |
| 40–55 minutes | Review using the rubric | Evidence-based scores |
| 55–60 minutes | Select two improvement priorities | Focus for 18 November |

### The 35-minute mock interview

| Segment | Duration |
|---|---|
| Introduction | 2 minutes |
| Three core questions | 9 minutes |
| Regional DLP case and challenge | 8 minutes |
| Follow-up questions | 10 minutes |
| Your questions for the panel | 6 minutes |

The times include the interviewer’s questions and brief transitions. You do not need to speak continuously for the entire slot.

### Two ways to use these notes

Guided-reading version:

Read the prompts and model answers to consolidate the material. This prepares you for the mock but does not test unaided recall.

Rehearsal version:

Read each prompt, hide the model answer and respond aloud. Record the attempt if practical. Review the model only after finishing.

Do not count reading a model answer aloud as an independent interview performance.

---

## 2. The Answer Structure to Keep in Mind

For a professional example:

    Context and objective
        ↓
    My responsibility
        ↓
    My specific actions
        ↓
    Result and supporting evidence
        ↓
    Limitation or lesson

For a hypothetical scenario:

    Clarify the objective and scope
        ↓
    Understand data flows and business impact
        ↓
    Establish owners and decision rights
        ↓
    Propose proportionate actions
        ↓
    Test and review evidence
        ↓
    Report remaining risk and required decisions

### Useful distinction

Past experience:

> I coordinated…

Hypothetical approach:

> I would first clarify…

Learning exercise:

> In my personal learning portfolio, I reviewed…

Keep these categories separate.

---

## 3. Opening Question: “Tell Me About Yourself”

Answer target: approximately 90 seconds, within the two-minute segment.

### Model answer

> My background combines enterprise IT delivery with data-protection and security-remediation work in banking.
>
> At Luxoft, working for a Swiss banking client, my experience has included coordinating encryption delivery, progressing compliance remediation and working across technical and business stakeholders.
>
> One example was coordinating the encryption of 65 previously unencrypted DBaaS servers. The work involved ownership gaps, EKM cluster issues and firmware dependencies. I have also coordinated application-storage remediation and contributed to data-protection platform delivery and proof-of-concept work.
>
> I now want to make data risk and controls my main professional focus. This role interests me because it connects control implementation, stakeholder coordination and remediation follow-through.
>
> Alongside my professional work, I am developing my security knowledge through structured learning and documented exercises, including NIST outcome analysis and threat modelling. I distinguish that learning from production experience, particularly in areas such as DLP, where I still need deeper hands-on operational exposure.

### Review questions

- Did I explain my current professional direction?
- Did I include relevant evidence rather than a complete career history?
- Did I make the move towards specialisation clear?
- Did I avoid describing myself as an established production DLP specialist?

---

## 4. Core Question One: “Describe a Data-Protection Initiative You Delivered”

Use the DBaaS encryption example.

Answer target: approximately two minutes, followed by one minute of probing.

### Supported starting point

> In my banking role, I coordinated the encryption of 65 previously unencrypted DBaaS servers. The delivery involved ownership gaps, EKM cluster issues and firmware dependencies, with a compliance timeline to meet.
>
> My responsibility was to coordinate the work across the relevant owners and progress the dependencies towards resolution.

Continue with your actual personal actions.

The preparation material does not establish enough detail to write a complete, truthful account of every action or validation step for you.

### Details to supply from your own experience

- How did you establish or clarify ownership?
- Which dependency did you personally help resolve?
- How did you track actions and escalate blockers?
- Who confirmed the technical result?
- What evidence did you actually review?
- Who made the completion or compliance decision?

### Supported result

> The recorded delivery outcome was encryption of the 65 servers in scope.

### Boundary to preserve

> That outcome concerns the encryption scope. I would not treat it as proof that every possible data-exfiltration risk had been removed.

### Possible interviewer challenge

> What did you personally do, rather than the engineering team?

Answer with a specific action you actually performed.

Do not replace “we” with “I” if the action belonged to someone else.

A useful distinction is:

> I coordinated the dependency and escalation. The technical team performed the configuration, and the relevant reviewer confirmed the result.

Use that wording only where it accurately describes the work.

---

## 5. Core Question Two: “How Do You Know a Control Is Effective?”

Answer target: approximately 90 seconds, followed by discussion.

### Model answer

> I would start with the control objective and the risk it is intended to address.
>
> First, I would assess whether the design can address that risk in the relevant context. Next, I would confirm that the control has actually been implemented across the agreed scope.
>
> I would then look for evidence of operation: defined tests, expected and observed results, relevant operational records and how failures or exceptions were handled over an agreed period.
>
> For a data-transfer control, I would want to check both that prohibited activity is restricted as intended and that permitted activity is handled correctly. I would also identify channels or users outside the control’s coverage.
>
> My conclusion would state what the evidence supports and what it does not. A deployment status or configuration screenshot alone would not establish sustained effectiveness.

### Hypothetical example

A rule is configured to restrict a defined class of confidential files sent to unapproved external recipients.

| Question | Example evidence |
|---|---|
| Is the design appropriate? | Defined data criteria, recipients, channel, actions and exception path |
| Is it implemented? | Configuration and evidence of the users or systems in scope |
| Does the tested behaviour match the objective? | Expected and observed results for prohibited and permitted transfers |
| Is it operating over time? | Relevant event samples, alert handling, failures and exception reviews |
| What remains uncertain? | Untested channels, unsupported file types or incomplete telemetry |

This is an illustrative assessment structure, not an EY test template.

---

## 6. Core Question Three: “A Stakeholder Refuses a Control. What Do You Do?”

Answer target: approximately two minutes, followed by probing.

### Model answer

> I would first understand the reason for the objection. A control may be disrupting a legitimate process, or there may be disagreement about the requirement, ownership or implementation.
>
> I would clarify the business objective, the affected data flow and the impact of the restriction. Then I would work with the control owner and technical team to assess options that preserve the protection objective.
>
> Depending on the facts, those options could include an approved transfer method, a scoped configuration change or a documented temporary exception.
>
> I would make the remaining exposure and required decision explicit. I would not disable a global control solely because a local stakeholder objected, and I would not accept business risk unless that authority had been formally delegated.
>
> My contribution would be to organise the facts, coordinate the options, identify the appropriate decision-maker and track the agreed actions.

### Follow-up

> What if the stakeholder is very senior?

Suggested response:

> I would keep the discussion factual and respectful, explain the business impact and protection objective, and use the established escalation route. Seniority alone would not replace the required approval process.

---

## 7. Main Case: Regional DLP Rollout

Total case segment: eight minutes.

Suggested split:

- One minute: clarifying questions.
- Four minutes: proposed approach.
- Three minutes: interviewer challenge and response.

### Fictional scenario

A global requirement calls for restrictions on sending confidential client documents to unapproved external recipients.

A regional business team needs to exchange documents with external clients.

The deadline is approaching. Regional stakeholders report that the proposed restriction may interrupt legitimate work.

You are asked to support the regional implementation.

This is a teaching scenario, not a description of EY’s environment.

### Clarifying questions

Ask a small number of focused questions:

1. Which data, users, channels and recipients are in scope?
2. Which external exchanges are authorised business processes?
3. What is mandatory in the global requirement, and what can be adapted locally?
4. Who owns the control, business process and exception decision?
5. What has already been tested?

If answers are unavailable, state the assumptions you will use.

Do not invent a technology stack.

### Model approach

> I would begin by confirming the control objective and mapping the relevant regional data flows. I would want to understand which documents are involved, who sends them, which recipients are approved and which channels support the business process.
>
> Next, I would compare the global requirement with current regional practice and identify the gaps. I would separate mandatory protection outcomes from implementation choices that can be adapted.
>
> I would establish the relevant owners and decision rights, involving security, business and privacy or legal specialists where the facts require their input.
>
> For legitimate external collaboration, I would explore an approved transfer route or another scoped implementation that preserves the objective. If a temporary exception is necessary, I would coordinate a record of its scope, justification, owner, mitigants, approval, expiry and review.
>
> Before expanding enforcement, I would support a controlled pilot with agreed test cases and success criteria. Tests would cover prohibited transfers, permitted activity, exception handling and the availability of appropriate event evidence.
>
> I would also coordinate user communication and support so that people understand both the restriction and the approved way to complete their work.
>
> Finally, I would report implementation coverage separately from validation results and unresolved risk. I would confirm the operational owner, monitoring and escalation process before treating the rollout as complete.

### Challenge: “The region reports many false positives. Should we disable the rule?”

> I would first verify whether the reports are genuine detection errors. A correctly detected confidential document may still be part of an authorised process that needs a different policy treatment.
>
> I would collect safe examples and compare the intended policy with the observed behaviour. With the control owner and technical team, I would assess a targeted change, test it against both prohibited and permitted activity, and monitor the result.
>
> If there is an urgent business-continuity impact, I would escalate for an authorised, scoped decision. I would not assume that disabling the entire policy is the only option.

### Challenge: “The deadline is tomorrow. Can you mark it complete?”

> I would report exactly what is implemented, what is validated and what remains unresolved. If required evidence is missing, I would not describe the control as effective simply to meet the deadline. I would identify the owner, interim protection and decision needed.

---

## 8. Follow-Up Questions

Use this ten-minute segment to test whether your explanations remain precise under pressure.

### 8.1 “How is DLP different from encryption?”

> Encryption protects confidentiality under defined access and key-management conditions. DLP concerns identifying sensitive information and applying policy actions to activities involving it.
>
> An authorised user may be able to read and export data encrypted at rest, so storage encryption alone does not address every exfiltration path.

Be ready to name four channels:

- Email.
- Web upload.
- Removable media.
- Cloud sharing.

Do not claim that one policy or product configuration covers all four automatically.

### 8.2 “Who can accept the remaining risk?”

> The authorised decision-maker under the organisation’s governance model. I would clarify that authority rather than assume that the analyst, technical implementer or security team automatically has it.

### 8.3 “How would you report progress?”

> I would state the reporting date and agreed scope, then distinguish implementation coverage, validation status and remaining exposure. Material issues would have an owner, next action, target date and any decision required.

Hypothetical example:

> Of 20 in-scope applications, 16 have the configuration implemented and 10 have completed the agreed validation. Six implemented applications still require validation, and four remain unimplemented. Two overdue high-priority items require an ownership decision.

The numbers are fictional. Their purpose is to show the denominator and keep deployment separate from validation.

### 8.4 “What is your practical ISO experience?”

> I have completed ISO 27001 training, but I have not led an ISO implementation or audit. I understand the connection between an information security management system, risk treatment, control selection and supporting evidence. My documented framework exercises have focused on NIST outcomes.

### 8.5 “Have you worked with Dataguard?”

> My documented experience does not establish a production Dataguard implementation. My closest professional work is in encryption, data-protection delivery and related proof-of-concept activity. I would like to understand what Dataguard covers in your environment and where this role’s responsibilities begin and end.

### 8.6 “What did your portfolio teach you?”

> My NIST work helped me distinguish risk ownership, control implementation and approval authority. My threat-modelling work helped me separate a threat hypothesis from a confirmed vulnerability.
>
> These were learning exercises, not professional audits or proof of production control effectiveness.

---

## 9. Your Questions for the Panel

Use the final six minutes for selected questions and follow-ups.

1. What does Dataguard cover in this team?
2. How is responsibility divided between global control design, regional implementation and ongoing operation?
3. What would success look like after three and six months?
4. What evidence is required before remediation can be closed?
5. How much of the role involves analysis and validation compared with programme coordination?

Ask questions that have not already been answered.

### Example follow-up

If the interviewer says the role owns remediation tracking:

> Does that include assessing whether the closure evidence is sufficient, or is that review performed by a separate control or assurance team?

The purpose is to understand the work, not to demonstrate a memorised list.

---

## 10. Review Rubric

Use the workbook’s 0–3 training scale.

| Score | Meaning |
|---|---|
| 0 | No usable answer |
| 1 | Generalities, major omissions or errors |
| 2 | Correct explanation with a relevant example |
| 3 | Concise explanation with evidence, limitations and a clear follow-up response |

The working target is at least 2 in each criterion actually tested, with no unresolved critical errors.

This is a preparation threshold, not an EY hiring standard.

### Review table

| Criterion | What to check | Score | Timestamp or observation |
|---|---|---|---|
| Introduction and motivation | Clear professional bridge and deliberate specialisation | — | — |
| DBaaS example | Scope, personal contribution, result and boundary | — | — |
| DLP | Correct explanation without confusing it with encryption | — | — |
| Regional case | Scope, owners, pilot, tests, adoption, exceptions and operation | — | — |
| Evidence and effectiveness | Design, implementation and operation kept distinct | — | — |
| Risk ownership | No assumed authority to accept risk | — | — |
| Reporting | Date, denominator, validation and decision needed | — | — |
| Framework experience | Training and exercises not presented as audits | — | — |
| Experience boundaries | No unsupported production claims | — | — |
| Panel questions | Specific questions about actual responsibilities | — | — |

Mark criteria not covered as “not tested,” not as passed.

### Examples of useful review observations

Specific:

> At 12:40, I said “we tested it” but did not explain my contribution or the evidence.

Specific:

> In the regional case, I described the pilot but forgot operational ownership.

Too vague:

> I need to be more confident.

---

## 11. Critical Errors to Correct Before the Real Interview

Check whether you:

- Presented a learning scenario as professional delivery.
- Claimed production DLP experience without a real example.
- Assumed Dataguard means a particular product.
- Claimed that encryption eliminates exfiltration risk.
- Described deployment as proof of sustained effectiveness.
- Assumed every reported block is a false positive.
- Accepted risk without a stated mandate.
- Treated an alert as a confirmed breach.
- Described threat-model hypotheses as confirmed vulnerabilities.
- Invented personal actions, test results or approval details.

If you notice an overstatement during the interview, correct it directly.

> Let me make that more precise. I coordinated that activity; the engineering team performed the technical change.

Or:

> That was a learning exercise rather than a production implementation.

---

## 12. Select Two Priorities for 18 November

Do not finish with a long list of weaknesses.

Choose two improvements based on what actually happened.

### Example priority one: make personal contribution explicit

Observed issue:

> My professional story described the team’s work but not my actions.

Correction:

> Prepare a two-minute account with two specific actions I performed, the actual outcome and the source of confirmation.

### Example priority two: complete the control lifecycle

Observed issue:

> My case answer stopped at deployment.

Correction:

> Repeat the case and include validation, remaining exceptions, operational ownership and monitoring.

These are examples, not an assessment of your performance.

### Your review record

| Item | Entry |
|---|---|
| Strongest answer | To be assessed |
| Most important factual correction | To be assessed |
| Priority one | To be selected |
| Priority two | To be selected |
| Answer or case to repeat on 18 November | To be selected |

---

## 13. Final Reminder

You do not need to sound like someone who has already performed every part of the role.

You need to explain:

- What you have genuinely done.
- Why it is relevant.
- How you approach an unfamiliar problem.
- What evidence would support your conclusion.
- Which decisions belong to someone else.
- Where you still need development.

A useful closing summary is:

> I bring relevant data-protection delivery and remediation experience. I want to deepen the analytical and control-validation side of that work, and I am clear about the difference between my professional experience and my current learning activities.

---

## Source and Scope

The preparation workbook supplies:

- The 17 November mock-interview schedule.
- The professional examples and experience boundaries.
- The question themes and synthetic case patterns.
- The readiness criteria and 0–3 training scale.
- The plan to select two improvement priorities for 18 November.

The model wording, combined case narrative and hypothetical reporting figures are teaching material.

They are not confirmed EY interview questions, internal procedures or production test results.

After guided reading, an accurate statement is:

> Reviewed a mock-interview script and model answers for a data risk and controls role.

After actually completing the timed attempt, an accurate statement is:

> Completed an English mock interview and identified two evidence-based improvement priorities.

Neither statement by itself establishes practical control-operation competence.
