# Study Notes — 13 November 2026
# Reporting, Prioritisation and Decision-Focused Status Updates

Study time: approximately 60 minutes.

This lesson follows the end-to-end DLP cases from 12 November.

It moves from:

“What should we implement and test?”

to:

“What does the current evidence tell us, what matters most, and what decision is needed?”

The original preparation plan allocates:

- 10 minutes to recall.
- 25 minutes to a synthetic remediation register.
- 15 minutes to a management report.
- 10 minutes to English interview answers.

These notes contain completed examples. No spreadsheet editing, lab, additional research or written assignment is required.

All records, ratings, owners, dates, populations and decisions below are fictional teaching examples.

They do not describe EY or a real banking environment.

---

## 1. The central idea

A useful status report is not a list of activities.

It explains:

- What is in scope.
- What has changed.
- What has been verified.
- What remains exposed or uncertain.
- Who owns the next action.
- Which decision is required.

A project can be progressing while important control gaps remain.

A deployed control can remain unvalidated.

An approved exception can remain an exposure.

A green delivery milestone does not automatically mean low risk.

### Central sentence

> Report implementation, validation and remaining exposure separately.

### What the reader should understand

After reading your report, a manager should be able to answer:

1. Where are we now?
2. What is the evidence?
3. What matters most?
4. What happens next?
5. What do you need me to decide?

---

## 2. Ten-minute recall

### 2.1 Control objective

The intended protection outcome.

Example:

> Restrict inappropriate external sharing of confidential client information while preserving an approved collaboration route.

### 2.2 Implementation

The agreed control has been configured or put in place.

Implementation status does not prove that the intended behaviour occurs.

### 2.3 Validation

Evidence has been reviewed against defined criteria.

The conclusion must be limited to the assessed scope, scenarios and period.

### 2.4 Remaining exposure

The relevant risk or uncertainty that continues after considering actual controls and their limitations.

Do not automatically assign a numerical residual-risk rating merely because a task is complete.

### 2.5 Exception

An authorised deviation under the organisation’s process.

Approval does not make the exposure disappear.

### 2.6 Closure

Closure applies to a defined record and its agreed criteria.

Closing a configuration action is not necessarily the same as closing an exception or accepting remaining risk.

### 2.7 Decision authority

The analyst can prepare options, evidence and recommendations.

The appropriate authority makes decisions outside the analyst’s mandate.

---

## 3. Use separate status dimensions

A single status field often hides important differences.

For example:

> “Complete.”

Complete in what sense?

- Configuration completed?
- Population reconciled?
- Test executed?
- Evidence reviewed?
- Exception removed?
- Operational handover accepted?

### Recommended dimensions

| Dimension | Question |
|---|---|
| Implementation | Has the agreed change been put in place? |
| Validation | What evidence supports the intended behaviour? |
| Remediation | Which actions remain open? |
| Exception | Is a deviation proposed, approved, expired or closed? |
| Decision | Is an approval or escalation outstanding? |
| Risk/exposure | What business concern remains? |

These dimensions can be held in one register or linked records.

The important point is to preserve their meaning.

### Example

Implementation:

> Deployed.

Validation:

> Retest pending.

Exception:

> Not applicable.

Decision:

> Engineering capacity required.

Exposure:

> Intended protection is not yet confirmed.

This is more informative than marking the whole item “done.”

---

## 4. Synthetic register: reporting snapshot

For this worked example, the reporting snapshot is:

> 13 November 2026, 09:00 CET.

There are three control workstreams:

1. External email.
2. Cloud sharing.
3. Endpoint USB transfer.

These are three different workstreams, not three equally weighted units of risk.

Do not average their risk ratings.

### Important correction to the original preparation register

The original workbook contains a field where “Zablokowano” can be read as “blocked.”

That wording could mean either:

- Delivery is blocked by a dependency.
- A transfer was blocked by a control.

For reporting, remove that ambiguity.

In this lesson, use:

> “Implementation blocked by dependency.”

or:

> “Test transfer restricted as expected.”

Never make the reader infer which meaning applies.

---

## 5. Completed synthetic remediation register

### Record EX01: external email

Scope:

> A pilot group of 100 intended users.

Risk scenario:

> Confidential client information may be emailed to an unauthorised external recipient.

Control owner:

> Email Control Owner A.

Implementation status:

> Deployed for the agreed pilot group.

Validation status:

> Agreed point-in-time pilot tests completed and reviewed.

Evidence:

> Policy version, sender-group reconciliation, expected and observed test outcomes, and reviewer record.

Original implementation deadline:

> 12 November 2026.

Current action:

> Complete operational handover and begin the agreed operational review period.

Current action deadline:

> 16 November 2026.

Limitation:

> Pilot evidence does not establish protection for users outside the pilot, every detection scenario, other channels or continuous operation.

Closure position:

> The defined implementation-and-initial-validation action can be considered for closure if its criteria are met. Operational follow-up remains open.

### Record EX02: cloud sharing

Scope:

> 20 collaboration sites.

Risk scenario:

> Inappropriate sharing permissions or links may expose confidential files to unauthorised people.

Control owner:

> Collaboration Control Owner B.

Implementation status:

> Blocked by an unresolved technical dependency.

Validation status:

> No completed validation supporting the intended protective outcome.

Original implementation deadline:

> 6 November 2026.

Current action:

> Resolve the dependency and agree an authorised recovery plan.

Current action deadline:

> The original deadline has passed; a revised commitment is required.

Evidence:

> Dependency record and issue history exist, but they do not demonstrate the intended control is operating.

Decision needed:

> Whether to prioritise engineering capacity, approve another feasible treatment, or escalate the unresolved exposure.

Limitation:

> The available information does not establish that all 20 sites currently permit unauthorised access. It establishes that the planned remediation and its validation are incomplete.

Closure position:

> Open and overdue.

### Record EX03: endpoint USB transfer

Scope:

> 50 intended endpoints.

Risk scenario:

> Confidential information may be copied to an unauthorised removable device.

Control owner:

> Endpoint Control Owner C.

Implementation status:

> Configuration deployed to 45 of the 50 intended endpoints.

Validation status:

> Pending.

Original deadline:

> 15 November 2026.

Current action:

> Resolve the five-device deployment gap, reconcile coverage and execute the agreed tests.

Evidence:

> Deployment records indicate configuration on 45 endpoints. They do not yet establish the intended transfer behaviour.

Decision needed:

> Confirm whether the remaining actions can be completed before the deadline; escalate if capacity is insufficient.

Limitation:

> Deployment records are not proof that transfer restrictions operate as intended.

Closure position:

> Open; deadline not yet passed at the reporting snapshot.

---

## 6. What the register allows you to report

### 6.1 Workstream implementation status

Of the three workstreams:

- One has completed its agreed initial implementation and validation.
- One is blocked and overdue.
- One is partially deployed and unvalidated.

Do not describe this as:

> “Two out of three controls are effective.”

The evidence does not support that conclusion.

### 6.2 Initial validation status

One of three workstreams has completed the agreed initial validation.

That is approximately 33.3 percent of workstreams.

This is a workstream-count measure.

It is not:

- 33.3 percent of organisational risk removed.
- 33.3 percent of information protected.
- A measure of continuous operating effectiveness.

### 6.3 Endpoint implementation coverage

45 of 50 intended endpoints have the configuration deployed.

That is 90 percent deployment coverage for this endpoint scope.

It is not 90 percent DLP effectiveness.

### 6.4 Overdue remediation

One of the three original implementation deadlines has passed without the required completion.

For EX02:

> The 6 November deadline is seven calendar days before the 13 November reporting date.

The precise operational escalation should follow the organisation’s due-date convention.

The report should preserve the original deadline even if a recovery date is later approved.

### 6.5 Do not combine incompatible populations

Do not add:

- 100 users.
- 20 sites.
- 50 endpoints.

Then claim:

> “170 assets in scope.”

Users, sites and endpoints are different units.

A combined count would obscure the actual coverage.

Report each population separately unless a defined aggregation method makes the result meaningful.

---

## 7. Define a metric before using it

A metric needs more than a name.

A useful definition includes:

- Business or control question.
- Scope.
- Unit of measurement.
- Numerator.
- Denominator.
- Data source.
- Reporting date or period.
- Owner.
- Known limitations.
- Intended response when a threshold is crossed.

### Example: endpoint deployment coverage

Question:

> How much of the intended endpoint population has the agreed configuration?

Scope:

> The 50 endpoints in EX03.

Numerator:

> 45 endpoints with recorded deployment of the agreed configuration.

Denominator:

> 50 intended endpoints.

Value:

> 90 percent.

Source:

> Deployment inventory reconciled against the approved scope.

Snapshot:

> 13 November 2026, 09:00 CET.

Owner:

> Endpoint Control Owner C.

Limitation:

> This does not establish correct enforcement or sustained operation.

### Example: completed initial validation

Question:

> How many defined workstreams have met their initial validation criteria?

Numerator:

> One workstream.

Denominator:

> Three workstreams.

Value:

> Approximately 33.3 percent.

Limitation:

> The workstreams differ in scope and significance. The percentage is not a risk-reduction measure.

### Key sentence

> A percentage is useful only when its population and meaning are clear.

---

## 8. KPI, KRI and control-effectiveness measures

These categories can overlap in practice.

Use the organisation’s definitions.

Do not assume that the label alone makes a measure useful.

### 8.1 Key performance indicator

A KPI tracks performance against an objective.

Examples:

- Deployment coverage.
- Remediation actions completed by the agreed date.
- Time taken to review exceptions.
- Completion of required validation activities.

### 8.2 Key risk indicator

A KRI signals exposure or a change that may require attention.

Examples:

- High-priority gaps remaining beyond deadline.
- Expired exceptions still technically active.
- Intended populations without verified protection.
- Repeated control failures awaiting treatment.

A KRI is not automatically a complete quantitative estimate of risk.

### 8.3 Effectiveness measure

An effectiveness measure addresses whether a control or process achieves the intended outcome.

Examples:

- Results of appropriately designed control tests.
- Relevant observed failures and their causes.
- Evidence that required reviews occurred and findings were acted on.

A simple test-pass percentage still needs context:

- Which scenarios?
- Which population?
- Which period?
- How were cases selected?
- What remains untested?

### 8.4 Efficiency or timeliness measure

Examples:

- Time to investigate a reported control failure.
- Time to remediate a confirmed configuration error.
- Time to obtain the required exception decision.

Faster is not automatically better if the decision or investigation is incomplete.

### Example comparison

| Measure | What it tells you | What it does not prove |
|---|---|---|
| 90% endpoint deployment | Configuration progress | 90% prevention effectiveness |
| One overdue workstream | A delivery/treatment delay | The complete magnitude of risk |
| Required tests passed | Expected behaviour in assessed cases | Perfect future protection |
| No alerts this week | No alerts were recorded in the observed system | No inappropriate transfers occurred |

---

## 9. Prioritisation: what should happen first?

Do not prioritise solely by:

- Oldest ticket.
- Loudest stakeholder.
- Easiest item to close.
- Largest population.
- Nearest deadline.

Consider the business and exposure context.

### Useful prioritisation factors

1. Information sensitivity and business importance.
2. Potential consequences.
3. Actual exposure and reachability.
4. Existing controls and their verified limitations.
5. Evidence of failures or active exploitation, where relevant.
6. Time sensitivity and applicable obligations.
7. Dependencies.
8. Feasibility and effect of interim treatment.
9. Uncertainty that materially affects the decision.

Use the organisation’s assessment method.

Do not invent an EY risk-scoring formula.

### Worked priority recommendation

For this fictional scenario:

First:

> Escalate EX02 for a decision because remediation is overdue, the dependency remains unresolved, and there is no validated evidence of the intended protection.

At the same time:

> Confirm the concrete access and sharing exposure rather than assume every site is publicly accessible.

Second:

> Protect capacity for EX03 coverage reconciliation and validation before its approaching deadline.

Third:

> Complete EX01 operational handover while maintaining its agreed protection and follow-up.

This is a recommendation for the stated facts.

If new evidence shows active inappropriate access or another urgent exposure, the order may change.

### Parallel action matters

Prioritisation does not necessarily mean completing everything sequentially.

Different owners can progress separate actions in parallel.

The management decision may concern which scarce specialist resource should be allocated first.

---

## 10. RAG status: red, amber and green

RAG is a reporting convention.

It is not a universal risk model.

Define what each colour means before relying on it.

### Fictional delivery convention

Red:

> A material commitment has been missed or a critical dependency prevents recovery without a decision.

Amber:

> Delivery remains possible, but a material gap or dependency threatens the commitment.

Green:

> The defined delivery criteria are met or delivery is on track with evidence.

### Apply it carefully

EX01:

> Green for the defined initial implementation-and-validation milestone; operational handover remains open.

EX02:

> Red for overdue remediation and unresolved dependency.

EX03:

> Amber because deployment is incomplete, validation is pending and the deadline is approaching.

### Important limitation

These are delivery statuses.

Do not translate:

- Green into “no risk.”
- Red into “confirmed breach.”
- Amber into a formal medium-risk rating.

A useful dashboard may display:

- Delivery status.
- Validation status.
- Exposure or assessed risk.

Keep the labels distinct.

---

## 11. The five-sentence management report

The plan’s target is a report containing scope, risk, ownership and a decision.

### Reusable structure

Sentence one:

> State the scope and snapshot.

Sentence two:

> State verified progress and its limitations.

Sentence three:

> Identify the most important unresolved exposure or uncertainty.

Sentence four:

> Name the owners, next actions and dates.

Sentence five:

> State the decision required and when it is needed.

### Completed report

> As of 13 November 2026 at 09:00 CET, the review covers three workstreams: email protection for 100 pilot users, cloud-sharing remediation across 20 sites and USB-transfer controls for 50 endpoints.
>
> Email pilot implementation and initial validation are complete within the agreed scope, while endpoint deployment covers 45 of 50 devices but remains unvalidated.
>
> Cloud-sharing remediation is seven calendar days beyond its original deadline, with an unresolved dependency and no completed validation of the intended protective outcome.
>
> Collaboration Control Owner B must present the recovery plan today, Endpoint Control Owner C must resolve the five-device gap and complete validation against the 15 November commitment, and Email Control Owner A must complete handover by 16 November.
>
> A decision is required by 14:00 CET today on prioritising engineering capacity for cloud remediation or selecting another authorised treatment, with any continuing exposure handled through the appropriate risk-governance process.

The decision deadline and commitments are fictional.

### Why the report works

It includes:

- A date and time.
- Clearly separated populations.
- Implementation versus validation.
- An overdue item.
- Named ownership.
- Concrete next steps.
- A specific decision.

It does not claim a percentage of risk reduction.

---

## 12. Reporting uncertainty and poor data

A report can be accurate without claiming certainty.

### Example: unknown endpoint population

Weak statement:

> “Deployment is 100 percent.”

Problem:

The denominator may be incomplete.

Better statement:

> “Deployment is recorded for all 45 devices currently reconciled to the inventory. Five additional intended devices remain unresolved, so full coverage is not confirmed.”

### Example: missing evidence

Weak statement:

> “The control is ineffective.”

Problem:

Missing evidence does not always prove failure.

Better statement:

> “The intended behaviour has not yet been demonstrated by the required evidence.”

### Example: stale data

Weak statement:

> “The exception is approved.”

Problem:

The record may be outdated or expired.

Better statement:

> “The available approval record expired on the stated date; current authorisation has not been confirmed.”

### Good reporting practice

Make material uncertainty visible.

Identify:

- What is known.
- What is not known.
- Why it matters.
- Who will resolve it.
- When an update is expected.

Do not turn uncertainty into an unsupported green status.

---

## 13. Escalating without exaggeration

An escalation is a request for an appropriate response.

It should not be an emotional summary of frustration.

### Weak escalation

> “The team is not responding and the programme is at risk. Please help.”

### Better escalation

> “Cloud-sharing remediation remains blocked by the identified integration dependency and is seven days beyond the original deadline.
>
> The control owner cannot commit to recovery without engineering capacity.
>
> The current record does not demonstrate the intended protection across the 20-site scope.
>
> We need a decision on resource allocation or another authorised treatment by 14:00 today.
>
> I recommend prioritising the dependency assessment and confirming interim safeguards while the recovery plan is agreed.”

### Do not silently rewrite history

If the original due date is missed:

- Preserve the original date.
- Record the reason.
- Record the revised commitment separately.
- Record who approved the change.
- Keep the exposure visible.

A revised date does not mean the original commitment was met.

---

## 14. Interview answers

### “How would you report progress to leadership?”

> “I would state the scope and reporting date, then separate implementation coverage, validation and remaining exposure.
>
> I would highlight overdue or material issues, assign a named owner and next action, and make the required decision explicit.
>
> I would not present deployment percentage as proof of control effectiveness or risk reduction.”

### “Which metrics would you use?”

> “I would select measures that answer the relevant decision: coverage for implementation, evidence against agreed criteria for validation, and overdue gaps or expired exceptions as exposure indicators.
>
> I would define the numerator, denominator, period and data source. I would also explain what the measure cannot establish.”

### “How would you prioritise remediation?”

> “I would use the organisation’s method to consider business impact, actual exposure, existing controls, time sensitivity and dependencies.
>
> I would verify the evidence rather than prioritise solely by ticket age or stakeholder pressure.
>
> Where resources are constrained, I would present the trade-off and required decision.”

### “What if an owner asks you to mark an item green?”

> “I would compare the evidence with the agreed status criteria.
>
> If implementation is complete but validation is pending, I would report those separately.
>
> I would not change the conclusion solely to improve the dashboard.”

### “What if you cannot verify the data?”

> “I would identify the uncertainty, its effect on the report and the owner responsible for resolving it.
>
> Where material, I would qualify the metric or withhold the unsupported conclusion rather than invent precision.”

### “How does your background relate?”

Use only where accurate:

> “My delivery and remediation experience is relevant to consolidating status, clarifying dependencies and coordinating actions across teams.
>
> For this role, I would make the connection to risk and control evidence explicit: what changed, what was verified, what remains open and who must decide.”

---

## 15. Common mistakes and corrections

### Mistake 1

> “The programme is 90 percent secure.”

Correction:

> “Endpoint deployment covers 45 of 50 intended devices; validation is pending.”

### Mistake 2

> “The deadline moved, so the item is no longer overdue.”

Correction:

> “The original commitment was missed. A recovery date has been agreed and is tracked separately.”

### Mistake 3

> “There were no alerts, so the control is effective.”

Correction:

> “No alerts were recorded. We still need to understand coverage, telemetry, actual activity and validation.”

### Mistake 4

> “An exception is approved, so the risk is closed.”

Correction:

> “The deviation has an authorised decision; its conditions, exposure, review and expiry remain relevant.”

### Mistake 5

> “All workstreams count equally, so the completion rate represents risk reduction.”

Correction:

> “A workstream completion rate describes delivery. The workstreams differ in scope and business significance.”

### Mistake 6

> “The report explains the problem; management will know what to do.”

Correction:

> “State the decision required, options, recommendation, authority and deadline.”

---

## 16. Knowledge check

### 1. What must accompany a percentage?

Its scope, numerator, denominator, reporting date or period, and meaning.

### 2. Is deployment coverage an effectiveness measure?

Not by itself.

It describes implementation coverage.

### 3. Can users, sites and endpoints be added into one meaningful coverage denominator?

Not without a justified aggregation method.

They are different units.

### 4. Does missing validation prove the control failed?

Not necessarily.

It means the required conclusion is not yet supported.

### 5. Does an overdue ticket automatically represent the highest risk?

No.

Consider business impact, exposure, controls and urgency.

### 6. Is RAG status a universal risk rating?

No.

Define what the colours represent.

### 7. What should happen when a due date is revised?

Preserve the original date and record the recovery commitment and decision separately.

### 8. What is the purpose of the final sentence in the management report?

To make the required decision explicit.

### 9. Can an analyst accept remaining risk because the report is overdue?

No.

Authority must be established.

### 10. What is the main lesson?

Report what the evidence supports and make action and decision ownership clear.

---

## 17. One-minute recap

> “I would report scope and date first.
>
> I would separate implementation, validation and remaining exposure.
>
> Every percentage would have a defined denominator and limitation.
>
> I would prioritise using business impact, exposure, existing controls, deadlines and dependencies—not simply the easiest item to close.
>
> For each material issue, I would name the owner, next action and target date.
>
> The report would end with the specific decision needed, rather than leave management to infer it.”

### Accurate learning-log entry

> Reviewed a synthetic remediation register and a completed five-sentence management report. Studied metric definitions, denominators, implementation versus validation, risk-informed prioritisation, uncertainty and decision-focused escalation.

### What this session does not establish

Reading these notes does not establish that you:

- Verified production metrics.
- Audited control effectiveness.
- Assessed an employer’s residual risk.
- Approved a revised deadline.
- Accepted business risk.
- Produced an actual management report for EY.

### Source basis

The preparation workbook provides the 13 November session structure and target outcome.

NIST SP 800-55 Volume 1 supports the distinction between implementation, effectiveness, efficiency and impact measures, and the selection of measures suited to organisational needs.

The register, calculations, priorities, RAG definitions, report and interview answers are fictional teaching examples.

They are not EY reporting standards or a mandatory NIST dashboard.
