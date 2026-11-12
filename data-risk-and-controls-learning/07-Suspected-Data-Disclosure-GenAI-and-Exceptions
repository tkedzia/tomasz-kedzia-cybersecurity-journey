# Study Notes — 14 November 2026
# Suspected Data Disclosure, GenAI and Exceptions

Study time: approximately 60 minutes.

This lesson combines:

- C4: a DLP alert involving a private email address.
- C5: a business request to upload client documents to a GenAI service.
- Remaining risk and authorised exceptions.
- Clear English interview answers.

It builds on the earlier lessons about DLP, control effectiveness, regional rollout and reporting.

No lab, additional research or written assignment is required.

All scenarios, records, decisions and example statements are fictional teaching material.

They do not describe EY’s environment or a real incident.

---

## 1. The central idea

An analyst must distinguish three situations:

1. A future activity needs assessment.
2. An activity may already have happened and needs incident triage.
3. A control gap or exception needs a governance decision.

These situations can overlap, but they are not interchangeable.

A request to use an AI tool is not automatically an incident.

An alert is not automatically proof of a completed disclosure.

An approved exception does not retrospectively erase a past event.

### Central sentence

> Establish what is known, escalate uncertainty appropriately, and do not claim decision authority you have not been given.

### The role boundary

You may be responsible for:

- Recording facts.
- Identifying affected controls and owners.
- Making a timely handoff.
- Coordinating actions.
- Maintaining a decision and remediation record.

That does not automatically authorise you to:

- Conduct forensic acquisition.
- Search unrelated employee information.
- Contact external recipients.
- Delete evidence.
- Notify regulators or clients.
- Decide legal reportability.
- Accept business risk.

The actual responsibilities depend on the organisation’s operating model and your delegated role.

---

## 2. Ten-minute recall

### 2.1 Event

An observed occurrence.

Example:

> A system records an attempted external email transfer.

### 2.2 Alert

A notification generated because configured conditions were met.

Example:

> A DLP rule generates an alert for recognised confidential content addressed to an external email account.

The alert may require investigation.

It is not the whole conclusion.

### 2.3 Suspected incident

A situation that needs assessment under the organisation’s incident process because it may involve an adverse security event.

Do not wait for complete certainty before making a required escalation.

### 2.4 Confirmed outcome

A conclusion supported by sufficient evidence.

Examples:

- The system restricted the tested activity.
- A message was delivered.
- An external recipient accessed a shared file.

These are different outcomes.

Delivery does not automatically prove that the recipient opened the content.

### 2.5 Legal or contractual notification

A separate assessment involving the relevant facts, obligations and authorised specialists.

Do not use “security incident,” “personal-data breach,” “reportable event” and “client-notification requirement” as interchangeable terms.

### 2.6 Remaining risk

The risk remaining after considering actual controls, their coverage and their effectiveness.

Planned controls describe a target position.

They do not automatically reduce the current evidence-supported exposure.

### 2.7 Exception

An authorised deviation under a defined process.

A technical exclusion is only a setting.

It is not proof that an exception has been approved.

---

## 3. C4: DLP alert involving a private email address

### Scenario

A DLP alert indicates that an employee attempted to email a confidential client file to a private email address.

The alert shows:

- A sender.
- A recipient.
- A timestamp.
- A policy reference.
- A sensitive-content match.

The protective outcome has not yet been verified.

### What you know

The alerting system recorded a match associated with the activity.

### What you do not yet know

- Whether the transfer was restricted.
- Whether the message was delivered.
- Whether there were other recipients.
- Whether the file was correctly classified.
- Whether the activity had any relevant authorisation.
- Whether earlier or related transfers occurred.
- Whether personal information was involved.
- Whether any legal or contractual notification is required.

### First useful statement

> “A DLP alert indicates a potentially inappropriate transfer. The actual transfer outcome and business context remain under assessment.”

Avoid:

> “An employee has leaked client data.”

That conclusion is not yet supported.

---

## 4. C4: immediate response

### Step 1: follow the established escalation route

Route the matter to the designated security or incident-response process.

Provide the available facts promptly.

Do not postpone the handoff while trying to resolve every uncertainty yourself.

### Step 2: identify the outcome requiring verification

The authorised team should establish:

- Which action was configured.
- Which deployment mode applied.
- What action actually occurred.
- Whether the message or attachment left the controlled environment.
- What related evidence is available.

A configured block action is not, by itself, proof of a successful block.

### Step 3: preserve relevant information within your remit

Record or reference:

- Alert and case identifiers.
- Timestamps and time zones.
- Policy and rule references.
- Available outcome records.
- Relevant identities.
- Evidence locations.
- Your handoff and actions.

Use the organisation’s approved evidence-handling and retention procedures.

Do not casually download the client file or copy it into a personal workspace.

Do not independently alter the original evidence.

Formal chain-of-custody requirements depend on the case and procedures, but evidence integrity and provenance remain important.

### Step 4: support authorised containment

Possible actions depend on what happened and the available systems.

The authorised incident team may consider measures affecting:

- Sharing permissions.
- Access.
- Accounts.
- Sessions.
- Transfer routes.
- Related activity.

Do not independently disable an account or remove content unless your role and procedure authorise it.

### Step 5: bring in relevant specialists

Depending on the facts, involve:

- Security operations or incident response.
- The control owner.
- The business or information owner.
- Privacy.
- Legal.
- Other authorised communications or contractual specialists.

The analyst does not need to decide every classification before escalating.

---

## 5. C4: three possible outcomes

### Outcome A: the transfer was restricted

A careful conclusion is:

> “The available evidence supports that the specific attempted transfer was restricted.”

Do not conclude:

> “No disclosure was possible.”

Further questions may remain:

- Was another route used?
- Were there earlier related transfers?
- Was the restriction complete?
- Does the attempt indicate a process or training problem?
- Does the event still require treatment under the incident process?

### Outcome B: the message was delivered

A careful conclusion is:

> “The available evidence supports delivery of the message to the external address.”

That does not automatically establish:

- The recipient’s identity.
- Whether the attachment was opened.
- Whether the content was copied.
- Whether the activity meets a particular legal definition.

Escalate the confirmed facts and remaining uncertainty.

### Outcome C: the outcome is unclear

A careful conclusion is:

> “The current evidence does not establish whether the transfer completed.”

Do not translate uncertainty into:

> “Probably blocked.”

Keep the uncertainty visible and support the authorised investigation.

---

## 6. C4 model interview answer

> “I would treat the alert as a potentially significant event and follow the organisation’s incident escalation process. I would not assume that a sensitive-content match proves a completed disclosure.
>
> I would provide the case identifier, time, sender, recipient, policy reference and available action records. The first important question is whether the transfer was actually restricted or whether the information left the controlled environment.
>
> I would preserve or reference relevant records within my permissions and follow the approved evidence-handling process. I would not copy the client document into an unapproved environment or conduct an independent forensic investigation.
>
> The designated incident team would coordinate analysis and any containment. I would support the control owner and relevant business stakeholders, and make sure privacy or legal specialists are involved where the information and obligations require it.
>
> If the message was delivered, I would distinguish delivery from confirmed access or onward disclosure. If the transfer was blocked, I would still avoid claiming that all related exposure had been eliminated.
>
> My follow-up would be to track any control or process gaps, agreed remediation, testing and closure evidence. I would not independently decide regulatory notification or accept remaining risk.”

---

## 7. C5: proposed upload of client documents to GenAI

### Scenario

A business team wants to upload client documents to an external GenAI service to produce summaries.

The team says:

> “This would save several hours each week. Can we allow it?”

No upload is confirmed at this point.

### Start with the legitimate objective

The business wants to reduce time spent preparing summaries.

That objective is not inherently inappropriate.

The assessment concerns whether the proposed data flow and use are acceptable.

### Do not start with either extreme

Avoid:

> “All AI use is prohibited.”

Also avoid:

> “It is a business tool, so it must be safe.”

Start with the information, service, account, configuration and purpose.

---

## 8. Map the GenAI data flow

A simple flow is:

    Client document
          ↓
    Employee account and device
          ↓
    Prompt, upload or connector
          ↓
    GenAI service
          ↓
    Processing and possible storage
          ↓
    Generated output
          ↓
    Employee review and further sharing

Every stage may need assessment.

### 8.1 The source information

Ask:

- What classification applies?
- Does it contain personal information?
- Does it contain client-confidential or commercially sensitive information?
- Is the whole document necessary?
- Are there restrictions on this use?

### 8.2 The service and account

Ask:

- Is this specific service approved?
- Is the proposed account or tenant approved?
- Is it a personal or enterprise account?
- Which features are involved?
- Are file uploads, connectors or shared workspaces enabled?
- Which settings and contractual arrangements apply?

Approval of a product name is not necessarily approval of every account, feature or data category.

### 8.3 Processing and storage

Establish the applicable arrangements for:

- Retention.
- Access.
- Deletion.
- Provider or support access.
- Subprocessors or other recipients.
- Training or service-improvement use.
- Processing locations.
- Logging.
- Sharing.

Do not assume that every GenAI service handles prompts in the same way.

### 8.4 The output

Ask:

- Could the summary still contain sensitive information?
- Who can access it?
- Where is it stored?
- Will it be shared externally?
- Who checks it against the source?

A generated summary does not automatically become public information.

It may retain the sensitivity of its inputs.

---

## 9. Safer options for the business need

Assess options rather than presenting only approval or rejection.

### Option 1: an approved service and use case

Use the approved environment within its permitted:

- Data categories.
- Account configuration.
- Features.
- Access model.
- Retention and contractual conditions.

“Approved service” does not mean unrestricted use.

### Option 2: less information

Consider whether the task can be performed using:

- A smaller extract.
- A non-sensitive template.
- Synthetic content.
- An appropriately reviewed transformation.

Removing names alone does not guarantee anonymisation.

Context and combinations of details may still identify a person or client.

### Option 3: another workflow

The business objective may be met through:

- An existing approved tool.
- A controlled manual process.
- A different integration.
- A narrower pilot using synthetic information.

### Option 4: a formal assessment or exception request

Where a deviation is genuinely needed, prepare the required decision package.

Do not use a policy exception to bypass legal, contractual or other obligations that remain applicable.

### Important technical limitation

DLP visibility and enforcement depend on the actual channel, service, configuration and supporting capabilities.

Do not promise:

> “DLP can inspect and block every prompt.”

Test the relevant route and state the limitations.

---

## 10. C5 model interview answer

> “I would begin with the business objective: the team wants to summarise documents more efficiently. I would then identify the information involved and map how it would move into the service and where the output would go.
>
> I would check whether the specific service, account, features and data category are approved. A product name alone would not establish that the proposed use is acceptable.
>
> I would involve the relevant owners to establish access, retention, processing, deletion and contractual conditions. I would not assume that every AI service uses prompts for training, and I would not assume that an enterprise account removes every disclosure risk.
>
> I would look for the safest workable route. That might be an approved service, a smaller amount of information, synthetic content for evaluation or another existing workflow.
>
> If an exception is needed, I would prepare a scoped proposal with the rationale, exposure, safeguards, owner, duration and review arrangements. The authorised decision-maker would approve or reject it.
>
> I would also check the control coverage for the actual upload or prompt route and validate the intended behaviour with synthetic information. I would not claim that DLP sees every AI interaction.
>
> Finally, I would ensure that outputs are handled according to their sensitivity and reviewed before use. If we discover that an upload already occurred, I would activate the appropriate incident triage rather than treat the request only as a future approval.”

---

## 11. When a request becomes an incident concern

Suppose the team says:

> “We have already uploaded ten client files. We are asking whether we can continue.”

The response now has two tracks.

### Track one: assess the past activity

Use the established incident process to determine:

- What was submitted.
- When.
- By whom.
- Through which account and service.
- Under which settings.
- What access and retention arrangements applied.
- What evidence is available.
- Whether containment or other action is needed.

### Track two: decide future use

Assess whether the workflow can continue safely and lawfully under the organisation’s requirements.

A future approval does not automatically resolve the past event.

### Avoid uncoordinated deletion

Do not immediately instruct the user to delete everything without considering:

- Evidence preservation.
- Whether deletion is supported and effective.
- The provider’s arrangements.
- The incident team’s instructions.

Deletion may be relevant, but it should be coordinated through the appropriate process.

---

## 12. Remaining risk: current versus target

Consider three positions.

### Position one: current state

The business uses an unapproved account, and the organisation has not verified relevant data-handling arrangements or control coverage.

Do not invent a formal rating.

Describe the exposure and uncertainty.

### Position two: proposed target state

The organisation plans to introduce:

- An approved account.
- Restricted access.
- Defined data categories.
- Appropriate retention arrangements.
- User guidance.
- Validated controls.

This is a target design.

It is not evidence of current protection.

### Position three: implemented and assessed state

The controls are operating, and relevant evidence has been reviewed.

Now assess remaining risk using the organisation’s methodology.

Potential remaining concerns could include:

- Misclassification.
- Excessive information submission.
- Inappropriate output sharing.
- Unsupported routes.
- Configuration changes.
- Provider access under the applicable arrangements.
- User error.

Do not conclude:

> “Enterprise AI plus DLP means no remaining risk.”

### Key sentence

> A planned safeguard belongs in the target position until implementation and relevant operation are supported by evidence.

---

## 13. Worked exception decision package

This is a fictional proposal, not an approval.

### Request

> Permit a small team to evaluate an AI summarisation workflow for a defined business purpose.

### First decision

Can the evaluation use synthetic or non-sensitive information?

If yes, the confidential-data exception may not be necessary.

The activity may still need normal service or pilot approval.

### If a deviation remains necessary

Record:

| Field | Example |
|---|---|
| Business purpose | Evaluate document-summary efficiency |
| Requirement affected | Defined information-handling or approved-service requirement |
| Scope | Named users, service, account, feature and data category |
| Duration | Limited evaluation period |
| Information | Minimum necessary, with explicit exclusions |
| Current exposure | Documented under the organisation’s method |
| Safeguards | Feasible, owned and validated as required |
| Owner | Named business risk owner |
| Control owner | Named service or data-protection control owner |
| Decision authority | Confirmed delegated approver |
| Review | Defined progress and exposure review |
| Exit | Stop, transition or seek a new authorised decision |
| Evidence | Configuration, test and review references |

### Questions before approval

- Are the applicable obligations satisfied?
- Are the proposed safeguards actually available?
- What do they reduce?
- What remains?
- Who owns the decision?
- How will scope and expiry be enforced?

### Important distinction

Monitoring and training may support the response.

They do not automatically provide protection equivalent to preventing disclosure.

---

## 14. Legal and regulatory boundaries

This lesson teaches escalation discipline, not legal advice.

### Do not give a universal notification deadline

Notification requirements can depend on:

- The relevant law or contract.
- The organisation and its role.
- The type of event.
- The information involved.
- The facts and timing.
- The applicable assessment criteria.

A statement such as:

> “Every DLP alert must be reported to a regulator within a fixed number of hours.”

is not an acceptable general rule.

### Do not transfer banking rules automatically

Your banking background is relevant experience.

It does not establish that a particular banking regulation applies to every EY activity or service.

### Better interview answer

> “I would promptly engage the designated incident, privacy and legal processes. I would provide the facts and timeline needed for the authorised assessment rather than make an unsupported legal determination.”

Internal escalation should not be delayed while waiting for a final legal conclusion.

---

## 15. Ready-to-use status updates

### 15.1 Initial DLP update

> A DLP alert indicates an attempted transfer of recognised confidential content to a private email address.
>
> The actual transfer outcome is not yet verified.
>
> The matter has been routed to the designated incident process, with relevant identifiers and timestamps preserved in the approved case record.
>
> The control owner is supporting verification of policy mode and observed action.
>
> Incident classification and any notification assessment remain with the authorised teams.

### 15.2 GenAI request update

> The business wants to summarise client documents using an external AI service.
>
> The specific account, permitted data categories and applicable processing arrangements have not yet been confirmed.
>
> No approval for confidential uploads is implied by this assessment.
>
> The service and information owners are reviewing approved alternatives, data minimisation and control coverage.
>
> If prior uploads are identified, they will be routed for incident triage separately from the future-use decision.

### Why these updates work

They distinguish:

- Known facts.
- Unknown facts.
- Actions.
- Ownership.
- Decisions still outstanding.

They do not exaggerate certainty.

---

## 16. Common mistakes and corrections

### Mistake 1

> “The rule matched, so the file was blocked.”

Correction:

> “Verify the deployment mode, configured action and observed outcome.”

### Mistake 2

> “The email was delivered, so the recipient read it.”

Correction:

> “Delivery and access are different claims.”

### Mistake 3

> “The user made a mistake, so I will investigate their whole account.”

Correction:

> “Follow the authorised investigation scope and access process.”

### Mistake 4

> “All AI tools train on every uploaded document.”

Correction:

> “Check the specific service, account, feature, settings and applicable terms.”

### Mistake 5

> “The provider does not train on the data, so there is no confidentiality risk.”

Correction:

> “Training use is only one question. Assess access, retention, processing, sharing and other relevant conditions.”

### Mistake 6

> “Removing names makes the document anonymous.”

Correction:

> “Assess whether individuals or clients can still be identified from context or combined details.”

### Mistake 7

> “An exception can approve anything if a senior manager signs it.”

Correction:

> “Confirm delegated authority and applicable obligations. Internal approval does not override law or contract.”

### Mistake 8

> “The upload already happened, so approval will close the issue.”

Correction:

> “Assess the past event and future use separately.”

---

## 17. Interview follow-up questions

### “Would you contact the employee?”

> “I would follow the incident team’s process. Contact may be appropriate, but timing and scope should not undermine evidence preservation or the investigation.”

### “Would you contact the external recipient?”

> “Only through the authorised process. I would not independently contact a recipient or client.”

### “Would you delete the uploaded file?”

> “I would coordinate with the authorised team, taking account of evidence preservation, available deletion mechanisms and the service’s arrangements.”

### “What if the transfer was blocked?”

> “I would record the supported outcome, but still consider related activity, intent, process issues and any follow-up required by the incident model.”

### “Can you accept a temporary risk to keep the business working?”

> “I can prepare the assessment and options. I can approve acceptance only if the appropriate authority has been delegated to me.”

### “How does your experience transfer?”

Use only if accurate:

> “My data-protection and remediation experience helps me structure ownership, actions, evidence and escalation.
>
> I would distinguish that coordination experience from leading a forensic investigation or making privacy-law determinations.
>
> The incident and AI scenarios here are learning exercises, not production incidents I handled.”

---

## 18. Knowledge check

### 1. Does a DLP alert prove a completed disclosure?

No.

It provides a signal requiring context and verification.

### 2. Should you wait for complete certainty before escalating?

No.

Follow the defined escalation criteria promptly.

### 3. Does a blocked transfer prove no related exposure exists?

No.

The conclusion is limited to the supported activity and outcome.

### 4. What matters beyond the GenAI product name?

The actual service, account, features, data, settings, terms, access and processing arrangements.

### 5. Does “not used for training” mean “no disclosure risk”?

No.

Other access, retention and processing questions remain.

### 6. Can a policy exception override an applicable legal obligation?

No.

### 7. What changes when an upload already occurred?

A past-event assessment may be needed in addition to the future-use decision.

### 8. What is the difference between current and target risk?

Current risk considers actual supported controls. Target risk reflects the intended position after improvements.

### 9. Who decides legal reportability?

The authorised specialists and decision-makers under the organisation’s process.

### 10. What is the analyst’s central contribution?

Accurate facts, timely handoff, clear ownership, traceable actions and evidence-supported reporting.

---

## 19. One-minute final recap

> “For a DLP alert, I would establish the available facts and promptly follow the incident process.
>
> I would distinguish a rule match, a completed transfer, recipient access and legal reportability.
>
> I would preserve relevant information within my remit and support authorised analysis and containment.
>
> For GenAI, I would start with the business purpose and data flow, then assess the specific service, account, features and information-handling conditions.
>
> I would look for an approved, minimum-data route rather than assume either unrestricted approval or a universal ban.
>
> Any exception would require a defined scope, owner, safeguards, duration and authorised decision.
>
> I would keep past-event response, future-use approval and remaining-risk acceptance separate.”

### Accurate learning-log entry

> Reviewed fictional DLP-alert and GenAI-upload scenarios. Studied incident escalation, evidence boundaries, data-flow assessment, current versus target risk, exceptions and legal decision boundaries.

### What completing this session does not establish

It does not establish that you:

- Investigated a production incident.
- Performed forensic evidence collection.
- Determined legal reportability.
- Assessed a real AI supplier.
- Approved a confidential-data upload.
- Accepted business risk.

### Source basis and limitations

The preparation plan provides the session topic and C4/C5 scenarios.

NIST SP 800-61 Revision 3 supports:

- Risk-based incident triage.
- Coordinated response.
- Analysis and evidence handling.
- Integration of incident learning into risk management.

NCSC AI guidance supports:

- Understanding data flows.
- Protecting sensitive inputs, outputs and logs.
- Assessing access and data use.
- Communicating system limitations and responsibilities.

The cases, communications, proposed exception record and interview answers are fictional teaching examples.

They are not EY procedures, legal advice or provider-specific assurances.
